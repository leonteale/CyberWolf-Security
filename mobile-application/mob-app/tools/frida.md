# Frida

## Install Frida

{% code overflow="wrap" %}
```bash
pip install frida-tools
```
{% endcode %}

### check the device architecture&#x20;

{% code overflow="wrap" %}
```bash
adb shell getprop ro.product.cpu.abi
```
{% endcode %}

### Download extract and install the right Frida Gadget

{% code overflow="wrap" %}
```bash
wget https://github.com/frida/frida/releases/download/16.3.3/frida-gadget-16.3.3-android-x86_64.so.xz
xz -d frida-gadget-16.3.3-android-x86_64.so.xz
adb push frida-gadget-16.3.3-android-x86_64.so /data/local/tmp/
```
{% endcode %}

### Set Permissions on the Gadget:

{% code overflow="wrap" %}
```bash
adb shell
su chmod 755 /data/local/tmp/frida-gadget-16.3.3-android-x86_64.so
export FRIDA_GADGET_LIB=/data/local/tmp/frida-gadget-16.3.3-android-x86_64.so
```
{% endcode %}

### Create a Frida script to get the databases

The script can be found on my GitHub here: [frida\_list\_databases.js](https://github.com/leonteale/pentestpackage/blob/master/Utilities/frida\_list\_databases.js)

### Download and extract the Frida server:

{% code overflow="wrap" %}
```bash
wget https://github.com/frida/frida/releases/download/16.3.3/frida-server-16.3.3-android-x86_64.xz
xz -d frida-server-16.3.3-android-x86_64.xz
chmod +x frida-server-16.3.3-android-x86_64
adb push frida-server-16.3.3-android-x86_64 /data/local/tmp/
```
{% endcode %}

### Run the Frida server on the device:

{% code overflow="wrap" %}
```bash
adb shell
su
cd /data/local/tmp
./frida-server-16.3.3-android-x86_64 &
```
{% endcode %}

### Make sure the app has launched on the device (or the activity has been started manually

{% code overflow="wrap" %}
```bash
adb shell am start -n com.wezaam.hermesE/.MainActivity
```
{% endcode %}

### Attach to the process:

{% code overflow="wrap" %}
```bash
frida -U -p $(adb shell ps | grep -i com.wezaam.hermes | awk '{print $2}') -l frida_list_databases.js
```
{% endcode %}

