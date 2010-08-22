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