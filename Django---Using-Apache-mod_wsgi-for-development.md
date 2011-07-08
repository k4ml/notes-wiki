This is a note on how I setup local development environment using Apache2 and mod_wsgi. Using Django built-in `runserver` suck at 1 thing. If I have 1 browser window opening the page and try to open another page in Incognito mode (I'm using Chrome), the one in Incognito mode would failed to open until I click some link in the first window and let it load first.

Django built-in dev server is known to be single thread so it could be the reason. I've try some other development server that claimed to be multi-threaded but still having this problem at certain time. It didn't happen all the time but when it did, it really turned me off. Just recently I tried gunicorn hoping it would not have the same problem but it still does.

We use apache and mod_wsgi in production so using it for development still a good thing, right ? What make most of us turn to development server for development is the hassle to setup the apache config for every new project. I've been using a technique to start new apache process for every project I'm working on for PHP in the past. Basically all my php project would have the following structure:-

```bash
$ ls website.com
server lib www
$ ls www
index.php
$ ls server
run.py
```

`www` is the `DocumentRoot` and `run.py` is just a python script that start new apache2 process (default at port 8000) and set `www` as `DocumentRoot`. So everytime I want to start development on website.com, I just did:-

```bash
$ cd website.com/server
$ ./run.py
Running apache at 127.0.0.1:8000
```

This setup was inspired by how Webfaction setup their local apache for users. I kept this script at https://github.com/k4ml/devserver. Problem with apache is there's no command line options that we can pass to alter how it start and find out the modules. What this script does is generate the config file and start a process using that config. You have to properly setup apache/mod_wsgi first since this script assume everything already there. I started using this on Ubuntu 8.04 and then 10.04 so it simply `sudo apt-get install apache2 libapache2-mod-wsgi`.

For Django and mod_wsgi the config a bit different and I don't have time yet to make the script truly generic so it can used both in PHP and Django project. So here is the modified version:-

```bash
#!/usr/bin/env python

from optparse import OptionParser
from os.path import join, dirname, abspath
from string import Template

import os, sys

parser = OptionParser()
parser.add_option("-a", "--address", default="127.0.0.1", help="Server address to listen")
parser.add_option("-p", "--port", default="8000", help="Server port to listen")
parser.add_option("-k", "--command", help="Apache2 command to run")
parser.add_option("-b", "--httpd", default="apache2", help="Apache2 httpd binary to run")
parser.add_option("-m", "--module", default="/usr/lib/apache2", help="Apache2 modules path")

(option, args) = parser.parse_args()

APP_ROOT = abspath(join((dirname(__file__)), '../'))
DOC_ROOT = join(APP_ROOT, 'www')
SERVER_ROOT = join(APP_ROOT, 'server')
CONFIG_FILE = join(SERVER_ROOT, 'apache2.conf')

if option.command in ('stop', 'restart'):
    ret = os.system("%s -f %s -k %s" % (option.httpd, CONFIG_FILE, option.command))
    if ret == 0:
        print "%s apache2 ..." % option.command
    sys.exit()

CONFIG = """
ServerRoot $module
LoadModule alias_module modules/mod_alias.so
LoadModule auth_basic_module modules/mod_auth_basic.so
LoadModule authn_file_module modules/mod_authn_file.so
LoadModule authz_default_module modules/mod_authz_default.so
LoadModule authz_groupfile_module modules/mod_authz_groupfile.so
LoadModule authz_host_module modules/mod_authz_host.so
LoadModule authz_user_module modules/mod_authz_user.so
LoadModule autoindex_module modules/mod_autoindex.so
LoadModule cgi_module modules/mod_cgi.so
LoadModule dir_module modules/mod_dir.so
LoadModule env_module modules/mod_env.so
LoadModule mime_module modules/mod_mime.so
LoadModule negotiation_module modules/mod_negotiation.so
LoadModule rewrite_module modules/mod_rewrite.so
LoadModule setenvif_module modules/mod_setenvif.so
LoadModule status_module modules/mod_status.so
LoadModule userdir_module modules/mod_userdir.so
LoadModule wsgi_module modules/mod_wsgi.so

TypesConfig /etc/mime.types

KeepAlive Off
Listen $address:$port
LogFormat "%h %l %u %t \\"%r\\" %>s %b \\"%{Referer}i\\" \\"%{User-Agent}i\\" \\"%{Host}i\\"" combined
CustomLog $server_root/access_log combined
ErrorLog ${server_root}/error_log

ServerName localhost
 
PidFile ${server_root}/apache2.pid

DirectoryIndex index.html

DocumentRoot $doc_root
<Directory />
    Options FollowSymLinks
    AllowOverride None
</Directory>
<Directory ${doc_root}>
    Options Indexes FollowSymLinks MultiViews
    AllowOverride All
    Order allow,deny
    allow from all
</Directory>

WSGIProcessGroup rki
WSGIDaemonProcess rki python-path=/home/kamal/python/env/kedai/lib/python2.6 threads=5 display-name=rki
WSGIScriptAlias / ${app_root}/wsgi/dev_run.wsgi
WSGISocketPrefix ${app_root}/wsgi
"""

if 0:
    print "Option are: ", option
    print "Args are: ", args
    print APP_ROOT
    print CONFIG_FILE

template = Template(CONFIG)
template_vars = {
    'doc_root': DOC_ROOT,
    'server_root': SERVER_ROOT,
    'address': option.address,
    'port': option.port,
    'httpd': option.httpd,
    'module': option.module,
    'app_root': APP_ROOT,
}

f = open(CONFIG_FILE, "w")
try:
    f.write(template.substitute(template_vars))
finally:
    f.flush()
    f.close()

ret = os.system("%s -f %s -k start" % (option.httpd, CONFIG_FILE))
if ret == 0:
    print "Running apache at %s:%s" % (option.address, option.port)
```

This script assume your django project has the following structure and use virtualenv to install the python modules.:-

```bash
$ ls website.com
website server setup.py wsgi www
$ ls server
run.py
$ ls wsgi
dev_run.wsgi
```

It would find wsgi entry point in `wsgi` directory and assume `www` as `DocumentRoot` where I host the media files. I then use `django-staticfiles` app to populate `www` directory with static content from apps. You need to alter this line since it very specific to me:-

```apacheconf
WSGIDaemonProcess rki python-path=/home/kamal/python/env/kedai/lib/python2.6 threads=5 display-name=rki
```
