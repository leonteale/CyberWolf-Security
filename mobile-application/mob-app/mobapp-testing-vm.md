# MobApp Testing VM

### Download MobApp testing VM

You can download the mobile application testing VM here: xx

### Installed apps and commands

```
Mobfs
	~/mobfs/README.txt
	####This should already be installed
	sudo docker pull opensecurity/mobile-security-framework-mobsf:latest

	####So to run it, simply type:
	sudo docker run -it --rm -p 8000:8000 opensecurity/mobile-security-framework-mobsf:latest

	####Then browse to http://localhost:8000

	####Theres also an online version here:
	https://mobsf.live/

	
Mobsfscan
	/home/kali/.local/bin/mobsfscan


Qark
	/home/kali/.local/bin/qark
	
	
Apktool
	/usr/bin/apktool
		## Decompile the app
			$ apktool d mobapp.apk
		## Check for debuggable or backups
			$ cat mobapp/AndroidManifest.xml | grep -iE 'debuggable|backup'
	
xmind
	/snap/bin/xmind
	
Drozer
	agent: /home/kali/drozer-agent-2.3.4.apk
	client: 
		####This should already be installed
		sudo docker pull fsecurelabs/drozer
		
		####So to run it, simply type
		adb -a nodaemon server start   				(in your normal kali terminal)
		adb forward tcp:1234 tcp:31415				(in your normal kali terminal)
	
		sudo docker run -it fsecurelabs/drozer      		(in your normal kali terminal)
		drozer console connect --server <kali IP>:1234		(run this in the docker container terminal)
```
