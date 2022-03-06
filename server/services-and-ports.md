# Linux

## Run **Node.js** script _as service_ (Ubuntu)

1. Create `.service` file:

```
[Unit]
Description="My Express App"

[Service]
# set the working directory to have consistent relative path
WorkingDirectory=/project/absolute/path

# start the server file (file is relative to WorkingDirectory)
ExecStart=/usr/bin/node server.js

# if process crashes, always try to restart
Restart=always

# let 500ms between the crash and the restart
RestartSec=500ms

# send log to syslog
StandardOutput=syslog
StandardError=syslog

# nodejs process name in syslog
SyslogIdentifier=MyApp

User=nobody
# Use 'nogroup' group for Ubuntu/Debian
# use 'nobody' group for Fedora
Group=root

# set the environtment
Environment=NODE_ENV=production PORT=8080

[Install]
# start node at multi user system level (= sysVinit runlevel 3)
WantedBy=multi-user.target
```

> Example with **Banksquare** app:

```
[Unit]
Description=Banksquare
After=network.target

[Service]
ExecStart=/usr/bin/node /var/www/banksquare.vn/webapp/build/server.js
Restart=always
User=nobody
# Use 'nogroup' group for Ubuntu/Debian
# use 'nobody' group for Fedora
Group=root
Environment=PATH=/usr/bin:/usr/local/bin
Environment=NODE_ENV=production
WorkingDirectory=/var/www/banksquare.vn/webapp/build

[Install]
WantedBy=multi-user.target
```

2. File location:
   `/etc/systemd/system/my-app.service`.
3. Use `systemctl` to start:

```
systemctl enable my-app.service
systemctl start my-app.service
```

---

## List all **known** services

```
systemctl
systemctl | more
systemctl | grep httpd
systemctl list-units --type service
systemctl list-units --type mount
```

Or

```
systemctl list-unit-files
```

> Sample output:

```
[root@112.78.3.117]# systemctl list-unit-files
UNIT FILE                                     STATE
proc-sys-fs-binfmt_misc.automount             static
dev-hugepages.mount                           static
dev-mqueue.mount                              static
proc-sys-fs-binfmt_misc.mount                 static
sys-fs-fuse-connections.mount                 static
sys-kernel-config.mount                       static
sys-kernel-debug.mount                        static
tmp.mount                                     disabled
brandbot.path                                 disabled
systemd-ask-password-console.path             static
systemd-ask-password-wall.path                static
session-5097973.scope                         static
arp-ethers.service                            disabled
autovt@.service                               enabled
banksquarewebapp.service                      enabled
```

---

## View processes associated with a particular service (cgroup)

```
systemd-cgtop
```

> Sample output:

```
Path                                            Tasks   %CPU   Memory  Input/s Output/s

/                                                  85    0.3    1.9.G        -        -
/system.slice/banksquarewebapp.service              1      -        -        -        -
/system.slice/NetworkManager.service                2      -        -        -        -
/system.slice/auditd.service                        1      -        -        -        -
/system.slice/crond.service                         1      -        -        -        -
/system.slice/dbus.service                          1      -        -        -        -
/system.slice/lvm2-lvmetad.service                  1      -        -        -        -
/system.slice/polkit.service                        1      -        -        -        -
/system.slice/postfix.service                       3      -        -        -        -
/system.slice/rsyslog.service                       1      -        -        -        -
/system.slice/sshd.service                          1      -        -        -        -
/system.slice/...tty.slice/getty@tty1.service       1      -        -        -        -
/system.slice/systemd-journald.service              1      -        -        -        -
/system.slice/systemd-logind.service                1      -        -        -        -
/system.slice/systemd-udevd.service                 1      -        -        -        -
/system.slice/tuned.service                         1      -        -        -        -
/system.slice/wpa_supplicant.service                1      -        -        -        -
/user.slice/user-0.slice/session-2.scope            1      -        -        -        -
/user.slice/user-1000.slice/session-1.scope         4      -        -        -        -
```

---

## List services and their open port

1. Use `netstat`:

```
netstat -tulpn
```

| Options | Description                       |
| ------- | --------------------------------- |
| -t      | TCP socket                        |
| -u      | UDP socket                        |
| -l      | Listening sockets                 |
| -n      | numeric: Don't resolve host names |

> Sample output:

```
Proto Recv-Q Send-Q Local Address           Foreign Address         State
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN
tcp        0      0 127.0.0.1:25            0.0.0.0:*               LISTEN
tcp        0      0 0.0.0.0:443             0.0.0.0:*               LISTEN
tcp        0      0 127.0.0.1:9000          0.0.0.0:*               LISTEN
tcp6       0      0 :::3306                 :::*                    LISTEN
tcp6       0      0 :::22                   :::*                    LISTEN
tcp6       0      0 :::3000                 :::*                    LISTEN
tcp6       0      0 :::443                  :::*                    LISTEN
tcp6       0      0 :::33060                :::*                    LISTEN
```

---

## Check the status of a service using systemd

Use `systemctl status my-app.service`:

```
systemctl status banksquarewebapp.service
```

## List all the running node processes
```
ps aux | grep node
```

## Firewalld

Print the whole Firewalld configuration:

```
sudo firewall-cmd --list-all
```

> The open ports and services are listed in the `services:` and `ports:`

Just want to see what services are allowed to have open ports:

```
sudo firewall-cmd --list-services
```

To see only the ports that are open

```
sudo firewall-cmd --list-ports
```

Open Server Ports For Remote Access:

```
sudo firewall-cmd --zone=public --permanent --add-port=4000/tcp
sudo firewall-cmd --reload
```

Close Server Ports And Deny Remote Access:

```
sudo firewall-cmd --zone=public --permanent --remove-port=PORT/tcp
sudo firewall-cmd --reload
```
