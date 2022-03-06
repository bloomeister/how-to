# ACME Challenge

## HTTP-01 NGINX

> File `pycon.conf`:

```
server {
    listen 80;
    server_name ws.pycon.one;
    location / {
        default_type text/plain;
        return 200 "ws.pycon.one PyconWS";
    }
    location ~ ^/\.well-known/acme-challenge/([-_a-zA-Z0-9]+)$ {
        default_type text/plain;
        return 200 "$1.pmmzUiT3HicqDUM-xo_u3WZ28hgMN_tQAH-2Xn1mlCY";
    }
}

server {
    listen 80;
    server_name pycon.one;
    location / {
        default_type text/plain;
        return 200 "pycon.one PyconWS";
    }
    location ~ ^/\.well-known/acme-challenge/([-_a-zA-Z0-9]+)$ {
        default_type text/plain;
        return 200 "$1.pmmzUiT3HicqDUM-xo_u3WZ28hgMN_tQAH-2Xn1mlCY";
    }
}

server {
    listen 80;v
    server_name www.pycon.one;
    location / {
        default_type text/plain;
        return 200 "www.pycon.one PyconWS";
    }
    location ~ ^/\.well-known/acme-challenge/([-_a-zA-Z0-9]+)$ {
        default_type text/plain;
        return 200 "$1.pmmzUiT3HicqDUM-xo_u3WZ28hgMN_tQAH-2Xn1mlCY";
    }
}
```

**SUCCESS:**

```
IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at:
   /etc/letsencrypt/live/pycon.one/fullchain.pem
   Your key file has been saved at:
   /etc/letsencrypt/live/pycon.one/privkey.pem
   Your certificate will expire on 2022-05-25. To obtain a new or
   tweaked version of this certificate in the future, simply run
   certbot again. To non-interactively renew *all* of your
   certificates, run "certbot renew"
 - If you like Certbot, please consider supporting our work by:

   Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
   Donating to EFF:                    https://eff.org/donate-le
```

# Using Free Let’s Encrypt SSL/TLS Certificates with NGINX

## 1. Download the Let’s Encrypt Client

```
$ apt-get update
$ sudo apt-get install certbot
$ apt-get install python3-certbot-nginx
```

## 2. Set Up NGINX

1. Assuming you’re starting with a fresh NGINX install, use a text editor to create a file in the /etc/nginx/conf.d directory named domain‑name.conf (so in our example, www.example.com.conf).
2. Specify your domain name (and variants, if any) with the server_name directive:

```
server {
    listen 80 default_server;
    listen [::]:80 default_server;
    root /var/www/html;
    server_name example.com www.example.com;
}
```

3. Save the file, then run this command to verify the syntax of your configuration and restart NGINX:

```
$ nginx -t && nginx -s reload
```

## 3. Obtain the SSL/TLS Certificate

1. Run the following command to generate certificates with the NGINX plug‑in:

```
$ sudo certbot --nginx -d example.com -d www.example.com
```

2. Respond to prompts from certbot to configure your HTTPS settings, which involves entering your email address and agreeing to the Let’s Encrypt terms of service.

```
Congratulations! You have successfully enabled https://example.com and https://www.example.com

-------------------------------------------------------------------------------------
IMPORTANT NOTES:

Congratulations! Your certificate and chain have been saved at:
/etc/letsencrypt/live/example.com/fullchain.pem
Your key file has been saved at:
/etc/letsencrypt/live/example.com//privkey.pem
Your cert will expire on 2017-12-12.
```

> If you look at `domain‑name.conf`, you see that certbot has modified it:

```
server {
    listen 80 default_server;
    listen [::]:80 default_server;
    root /var/www/html;
    server_name  example.com www.example.com;

    listen 443 ssl; # managed by Certbot

    # RSA certificate
    ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem; # managed by Certbot

    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot

    # Redirect non-https traffic to https
    if ($scheme != "https") {
        return 301 https://$host$request_uri;
    } # managed by Certbot
}
```

## Automatically Renew Let’s Encrypt Certificates

Let’s Encrypt certificates expire after 90 days. We encourage you to renew your certificates automatically. Here we add a cron job to an existing crontab file to do this.

1. Open the crontab file.

```
$ crontab -e
```

2. Add the certbot command to run daily. In this example, we run the command every day at noon. The command checks to see if the certificate on the server will expire within the next 30 days, and renews it if so. The --quiet directive tells certbot not to generate output.

```
0 12 * * * /usr/bin/certbot renew --quiet
```

3. Save and close the file. All installed certificates will be automatically renewed and reloaded.
