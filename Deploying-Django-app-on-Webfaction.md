This is s shortnote on how I deploy my django app on Webfaction shared hosting.

## Apache setup
When we create a new Django app from Webfaction control panel, they actually setup a self contained apache2 environment in our `webapps` directory.

```Bash session
  $ pwd
  $ /home/username/webapps
  $ ls django
  $ 
```