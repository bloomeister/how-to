# MySQL Administration

## Show all grants for a specific user

Use `SHOW GRANTS [FOR user]`:

```
SHOW GRANTS FOR 'admin'@'localhost';
```

Additions:

```
SHOW GRANTS;
SHOW GRANTS FOR CURRENT_USER;
SHOW GRANTS FOR CURRENT_USER();
```

## Create new user for external access

Some systems like Ubuntu, MySQL is using the UNIX `auth_socket` plugin by default. Basically it means that: `db_users` using it, will be "authenticated" by _the system user credentials_. You can see if your root user is set up like this by doing the following:

```
sudo mysql -u root

mysql> USE mysql;
mysql> SELECT User, Host, plugin FROM mysql.user;
```

| User      | plugin                |
| --------- | --------------------- |
| root      | auth_socket           |
| mysql.sys | mysql_native_password |

As you can see in the query, the root user is using the `auth_socket` plugin.

**There are two ways to solve this:**

1. Set the `root` user to use the `mysql_native_password` plugin;
2. Create a new `db_user` with your `system_user` (recommended).

### Option 1:

```
sudo mysql -u root

mysql> USE mysql;
mysql> UPDATE user SET plugin='mysql_native_password' WHERE User='root';
mysql> FLUSH PRIVILEGES;
mysql> exit;

sudo service mysql restart
```

### Option 2:

```
sudo mysql -u root

mysql> USE mysql;
mysql> CREATE USER 'YOUR_SYSTEM_USER'@'localhost' IDENTIFIED BY 'YOUR_PASSWD';
mysql> GRANT ALL PRIVILEGES ON *.* TO 'YOUR_SYSTEM_USER'@'localhost';
mysql> UPDATE user SET plugin='auth_socket' WHERE User='YOUR_SYSTEM_USER';
mysql> FLUSH PRIVILEGES;
mysql> exit;

sudo service mysql restart

```

> One change as of MySQL 8.0.4 is that the new default authentication plugin is '`caching_sha2_password`'. The new '`YOUR_SYSTEM_USER`' will have this authentication plugin and you can log in from the Bash shell now with "`mysql -u YOUR_SYSTEM_USER -p`" and provide the password for this user on the prompt. There isn’t any need for the "`UPDATE user SET plugin`" step.
