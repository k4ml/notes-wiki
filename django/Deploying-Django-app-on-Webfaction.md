This is s shortnote on how I deploy my django app on Webfaction shared hosting.

## Apache setup
When we create a new Django app from Webfaction control panel, they actually setup a self contained apache2 environment in our `webapps` directory.

```console
$ pwd
$ /home/username/webapps
$ ls django
apache2  bin  lib  myproject  myproject.wsgi
```

* `myproject` - A default Django project created by Webfaction so that we can have a Django app running out of the box for us. We're not going to use this.
* `lib` - Python lib directory containing Django installation. We also not going to use this.
* `bin` - Apache start/stop script.
* `apache2` - This is the apache2 environment that we will use to host our Django application.

Let's take a look what inside the `apache2` directory.

```console
$ ls apache2
bin  conf  lib  logs  modules
```
That's a pretty standard layout of apache2 installation. We can find `httpd.conf`, the main apache config file inside `conf` directory. `modules` directory contain all apache modules available to this environment. The httpd.conf provided by webfaction quite simple, enough to bootstrap apache for hosting our apps. Let's take a brief look.

```apacheconf
ServerRoot "/home/k4ml/webapps/django/apache2"

LoadModule dir_module modules/mod_dir.so
LoadModule env_module modules/mod_env.so
......
```
First we setup the `ServerRoot` directive to this environment and then load all required modules.

```apacheconf
KeepAlive Off
Listen 40333
LogFormat "%{X-Forwarded-For}i %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\"" combined
CustomLog /home/k4ml/logs/user/access_django_log combined
ErrorLog /home/k4ml/logs/user/error_django_log
ServerLimit 2
```
Important part here is the `Listen` directive where we instruct apache to listen on port number 40333. This custom port number would automatically assigned when you created this django app/environment through the control panel. The main webfaction serving front end, be it apache or nginx would proxy all request to the domain that we configured through the control panel to the process listen at this port, in our case - our own apache instance.

```apacheconf
SetEnvIf X-Forwarded-SSL on HTTPS=1
NameVirtualHost *:40333
<VirtualHost *:40333> 
    ServerName yourdomain.com
    DocumentRoot /home/username/django/appname/appname/media

    WSGIScriptAlias / /home/username/django/appname/bin/django.wsgi
</VirtualHost>
```

Finally it setup the default virtual host for this server. We would come up to the wsgi part in the next section. This kind of setup show how simple it is for Webfaction to provide every users on the shared host their own apache process running with privilege rather than the main apache process in a typical shared hosting setup.

### Ubuntu way
I like how Ubuntu layout their apache installation with all virtual hosts definition in seperate file in `sites-available` directory with the active virtual host symlink from `sites-enabled` directory. This make automating setting up new virtual hosts easy, you just need to upload the complete virtual host definition file rather than have to edit the main config file. To disable any virtual host, just remove the symlink from `sites-enabled` directory.

So I modified the setup a bit, create two extra directory - `sites-available` and `sites-enabled` and include it from the main config file.

```apacheconf
Include /home/username/webapps/django/apache2/conf/sites-enabled/
```

The virtual hosts definition then moved to separate single file in `sites-available` for each virtual hosts. You might git error such as:-

```
[warn] _default_ VirtualHost overlap on port 40333, the first has precedence
```

When restarting apache. Make sure you set `NameVirtualHost *:40333` in the main apache config.

## Django app
I use [Buildout][1] to organize my Django project. This is how my `buildout.cfg` look alike:-

```ini
[buildout]
parts = 
    django
    misc

[misc]
recipe = zc.recipe.egg
interpreter = python
eggs =
    Werkzeug

[django]
recipe = djangorecipe
version = 1.2
download-cache = /tmp/buildout/cace
wsgi = True
project = ccupu
settings = settings
interpreter = python
eggs =
    django_extensions
```

[Djangorecipe][2] is used as the recipe to buildout on how to setup the actual Django project layout.

and the directory layout:-

```console
$ ls ccupu
bin  bootstrap.py  buildout.cfg  ccupu  db  develop-eggs  parts
```

The source code is hosted at Github at webfaction server, I just git clone from github url and run `bootstrap.py` to get back the identical environment as in my development machine.

The next step is to add new virtual host definition to our apache config.

```apacheconf
<VirtualHost *:40333> 
    ServerName yourdomain.com
    DocumentRoot /home/username/django/appname/appname/media

    WSGIScriptAlias / /home/username/django/appname/bin/django.wsgi
</VirtualHost>
```

### Issues
For unknown reason to me, adding `-python-path` parameter to `WSGIDaemonProcess` directive didn't work.

```apacheconf
<VirtualHost *:40333>
    ServerName example.com
    WSGIDaemonProcess django processes=5 python-path=/home/username/webapps/django:/home/username/webapps/django/lib/python2.6 threads=1
    WSGIScriptAlias / /home/username/webapps/django/myproject.wsgi
</VirtualHost>
```

So I have to add the path to Python library directory in myproject.wsgi.

```python
import os
import sys
import site

site.addsitedir('/home/username/webapps/django/lib/python2.6')
site.addsitedir('/home/username/webapps/django')

from django.core.handlers.wsgi import WSGIHandler

os.environ['DJANGO_SETTINGS_MODULE'] = 'myproject.settings'
application = WSGIHandler()
```

[1]:http://www.buildout.org/
[2]:http://pypi.python.org/pypi/djangorecipe
