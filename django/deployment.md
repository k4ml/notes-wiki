## Static files
These days I just wrap static files through Whitenoise and let Cloudflare handle the rest. But this won't work for media files as all the files to be served by Whitenoise are scanned upon startup.

Refer https://github.com/evansd/whitenoise/issues/32
