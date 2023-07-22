# Nginx

## Show

### Version

```
$ nginx -v
# or
$ nginx -V
```

### Status

```
$ systemctl status nginx
```

### Check Nginx Configuration Syntax

```
$ sudo nginx -t
# or
$ sudo nginx -T
```

### Reload Nginx Service

To tell Nginx to reload its configuration, use the following command.

```
$ sudo systemctl reload nginx #systemd
OR
$ sudo service nginx reload   #sysvinit
```

### Start Nginx Service

To start the Nginx service, run the following command. Note that this process may fail if the configuration syntax is not OK.

```
$ sudo systemctl start nginx #systemd
OR
$ sudo service nginx start   #sysvinit
```

### Enable Nginx Service

The previous command only starts the service for the meantime, to enable it auto-start at boot time, run the following command.

```
$ sudo systemctl enable nginx #systemd
OR
$ sudo service nginx enable #sysv init
```

### Restart Nginx Service

```
$ sudo systemctl restart nginx #systemd
OR
$ sudo service nginx restart   #sysv init
```

### Show Nginx Command Help

```
$ systemctl -h nginx
```
