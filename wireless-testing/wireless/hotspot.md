# Hotspot

Start a hotspot

{% code overflow="wrap" %}
```bash
nmcli device wifi hotspot ifname wlan0 con-name MyHotspot ssid MyHotspot band bg password MyPassword123
```
{% endcode %}

Stop the hotspot

{% code overflow="wrap" %}
```bash
nmcli connection down MyHotspot
```
{% endcode %}

Show hotspot passord

{% code overflow="wrap" %}
```bash
nmcli dev wifi show-password
```
{% endcode %}
