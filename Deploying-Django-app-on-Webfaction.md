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
* `apache` - This is the apache2 environment that we will use to host our Django application.