---
description: >-
  Internet Information Services is an extensible web server software created by
  Microsoft for use with the Windows NT family. IIS supports HTTP, HTTP/2,
  HTTPS, FTP, FTPS, SMTP and NNTP.
---

# IIS

## Enumeration

### Software and service versions

```
nmap [IP] -sV
nmap --script http-enum -sV -p 80 [IP]
nmap --script http-headers -sV -p 80 [IP]
nmap --script http-methods --script-args http-methods.url-path=/webdav/ [IP]
nmap --script http-webdav-scan --script-args http-methods.url-path=/webdav/ [IP]

whatweb [IP]

http [IP]
```

### Directory enumeration

See [this page](../web-application/directory-brute-forcing.md) on directory brute forcing.&#x20;

## End of life versions

[\* Extended Security Updates (ESU)](https://docs.microsoft.com/en-us/lifecycle/faq/extended-security-updates#esu-availability-and-end-dates) are available through specific volume licensing programs for these Windows products, for up to an additional three years past the end of support. Contact your Microsoft partner or account team to learn more.

| Version                                        |   Start Date |     End Date |
| ---------------------------------------------- | -----------: | -----------: |
| IIS 10 on Windows Server 2019                  | Nov 13, 2018 |  Jan 9, 2029 |
| IIS 10 on Windows Server (Semi-Annual Channel) | Oct 17, 2017 |              |
| IIS 10 on Windows Server 2016                  | Oct 15, 2016 | Jan 12, 2027 |
| IIS 10 on Windows 10, Enterprise and Education | Jul 29, 2015 |              |
| IIS 10 on Windows 10 Pro                       | Jul 29, 2015 |              |
| IIS 8.5 on Windows Server 2012 R2              | Nov 25, 2013 | Oct 10, 2023 |
| IIS 8.5 on Windows 8.1                         | Nov 13, 2013 | Jan 10, 2023 |
| IIS 8 on Windows Server 2012                   | Oct 30, 2012 | Oct 10, 2023 |
| IIS 7.5 on Windows 7\*                         | Oct 22, 2009 | Jan 14, 2020 |
| IIS 7.5 on Windows Server 2008 R2\*            | Oct 22, 2009 | Jan 14, 2020 |
| IIS 7.0 on Windows Server 2008\*               |  May 6, 2008 | Jan 14, 2020 |
| IIS 6.0 on Windows Server 2003                 | May 28, 2003 | Jul 14, 2015 |
