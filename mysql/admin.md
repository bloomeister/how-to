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
