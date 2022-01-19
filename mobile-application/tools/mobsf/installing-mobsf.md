# Installing MobSF



```
git clone https://github.com/MobSF/Mobile-Security-Framework-MobSF --depth=1
```

After applying this command Mobile Security Framework will be cloned on our system. It is a big tool (around 300MB) so it will take time depending on our internet speed.

[![Moblie Security Framework cloning github](https://1.bp.blogspot.com/-oUDZKE7mdS0/X9Yx4mEm-XI/AAAAAAAAEKc/Ndi-PvQHPZYWa\_4pqg1p1PXs6o4Yf\_eAACLcBGAsYHQ/w640-h148/MobSF%2BGitClone.png)](https://1.bp.blogspot.com/-oUDZKE7mdS0/X9Yx4mEm-XI/AAAAAAAAEKc/Ndi-PvQHPZYWa\_4pqg1p1PXs6o4Yf\_eAACLcBGAsYHQ/s688/MobSF%2BGitClone.png)

After cloning the tool we just navigate inside it's directory by using **cd** command:

```
cd Mobile-Security-Framework-MobSF
```

Now we can see the files by using **ls** command:

[![Moblile Security Framework files Kali Linux](https://1.bp.blogspot.com/-UXwMqtOOXgc/X9YyTwNQ9MI/AAAAAAAAEKk/joEdlxOvb6Mf8GOsZA0AIrvknnUfl6fmQCLcBGAsYHQ/w640-h92/mobsf%2Bfiles.png)](https://1.bp.blogspot.com/-UXwMqtOOXgc/X9YyTwNQ9MI/AAAAAAAAEKk/joEdlxOvb6Mf8GOsZA0AIrvknnUfl6fmQCLcBGAsYHQ/s1268/mobsf%2Bfiles.png)

This tool is available for Windows, Mac and Linux. Windows have setup.bat and run.bat files but Mac and Linux user can follow our article. We need to run setup.sh file.

To run the setup.sh file we run following command:

```
./setup.sh
```

This command will install all the required dependencies to run Mobile Security Framework, as we can see in the following screenshot.

[![Mobile Security Framework setup.sh setting up](https://1.bp.blogspot.com/-VA8aKEfeV7A/X9Y1HNk4R8I/AAAAAAAAEKw/JUY8MFOMwu07IDWObLmFtA5cGbuGmwl-gCLcBGAsYHQ/w640-h216/mobsf%2Bsetting%2Bup.png)](https://1.bp.blogspot.com/-VA8aKEfeV7A/X9Y1HNk4R8I/AAAAAAAAEKw/JUY8MFOMwu07IDWObLmFtA5cGbuGmwl-gCLcBGAsYHQ/s827/mobsf%2Bsetting%2Bup.png)

This setting up also might take some minutes depending on our internet speed.

After the installation complete we can use this tool by using run.sh command. As we previously told that this is a web based tool so we need to run it on our localhost server. To run it on our localhost with port 8000 (we can use any other port) by using following command:

```
./run.sh 127.0.0.1:8000
```

And Mobile Security Framework will started on [127.0.0.1:8080](https://www.kalilinux.in/2020/12/127.0.0.1:8080) as we can see in the following screenshot:

[![Mobile Security Framework running on Kali Linux](https://1.bp.blogspot.com/-ChmqdGIfT3w/X9bRASXW8jI/AAAAAAAAEK8/cxome9xELr4k4M9PSY4xSZCPwV7X2Pf-QCLcBGAsYHQ/w640-h106/mobsf%2Brunning.png)](https://1.bp.blogspot.com/-ChmqdGIfT3w/X9bRASXW8jI/AAAAAAAAEK8/cxome9xELr4k4M9PSY4xSZCPwV7X2Pf-QCLcBGAsYHQ/s710/mobsf%2Brunning.png)

If we run only ./run.sh command without any localhost IP and port then it will start on [0.0.0.0:8000](https://www.kalilinux.in/2020/12/0.0.0.0:8000) by default.
