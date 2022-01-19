# Install ALFA AWUS1900 on Kali

**Update and upgrade your OS**

`sudo apt-get update; apt-get upgrade`

**Update dependencies**

`sudo apt-get dist-upgrade -y`

**Verifying that the antenna is connected to the VM or PC.**

`sudo lsusb`

![](https://www.briansantacruz.me/wp-content/uploads/2019/07/1Capture.png)

_**Realtek RTL8814AU** should show up on the list._

**Installing the drivers**&#x20;

`sudo apt install realtek-rtl88xxau-dkms`

**Reboot and connect**

`sudo reboot`
