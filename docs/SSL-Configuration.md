##SSL Configuration notes

If your Zoneminder instance has SSL configured and you need zmNinja to connect to ZM over SSL you need to:

1) Make sure you generate the certificates correctly

2) Import the certificate into iOS and Android


###Certificate generation

####Self-signed certificates

If you are using self signed certificated, you should make sure the "common name" matches the hostname (or public IP)
of the server you are installing ZM in. If not, zmNinja's SSL handshake will fail.

If you have used 'make-ssl-cert' or a similar tool that automatically generates the cert for you, its very likely
you have certificate that uses the 'unix hostname' of your server. That will not work.

Assuming you are usin apache and have SSL enabled, here is how to regenerate the certs

This will create a self-signed certificate/key pair and store it in /etc/apache2/ssl (you may have to create that directory, or store it elsewhere)

```
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/apache2/ssl/zoneminder.key -out /etc/apache2/ssl/zoneminder.crt
```

Next up, edit your apache ssl config (example /etc/apache2/sites-available/default-ssl.conf)
And add/modify the following lines:

```
SSLCertificateFile /etc/apache2/ssl/zoneminder.crt
SSLCertificateKeyFile /etc/apache2/ssl/zoneminder.key
```

restart apache

```
sudo service apache2 restart
```

####Install certificates (.cer) file in yout phone
Then, you need to install zoneminder.crt in your mobile devices so that zmNinja does not reject the certificate as it is self signed.
The easiest way to do that is simply email it to yourself and open your email in the phone and install the attachment. Works for both
iOS and Android. Make sure you install it and go through the prompts
######Note: Just visiting your https site on mobile safari and "accepting the certificate" DOES NOT mean the certificate is installed in your phone. It simply sets up an exclusion on your browser that does not work for the UI webview inside your app

Once you are done with these steps, zmNinja should be able to connect to ZM via SSL. Make sure you select SSL in the settings

####CA signed certificates
If you purchase a signed certificate, or set up your own root CA zmNinja should just work over SSL. I haven't tried it so far.