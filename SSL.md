## Get SHA-1 fingerprint from ssl cert

<pre>
$ openssl x509 -sha1 -in /etc/apache2/ssl/myssl.crt -noout -fingerprint
</pre>
