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