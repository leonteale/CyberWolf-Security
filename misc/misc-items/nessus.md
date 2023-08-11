# Nessus



## Download Nessus

{% embed url="https://www.tenable.com/downloads/nessus" %}

licence: [https://community.tenable.com/s/products](https://community.tenable.com/s/products)

Windows

```
curl --request GET \
  --url 'https://www.tenable.com/downloads/api/v2/pages/nessus/files/Nessus-10.5.3-x64.msi' \
  --output 'Nessus-10.5.3-x64.msi'
```

Linux

{% code overflow="wrap" %}
```
wget "https://www.tenable.com/downloads/api/v1/public/pages/nessus/downloads/20393/download?i_agree_to_tenable_license_agreement=true" -O nessus.deb
```
{% endcode %}



## Install Nessus

Windows



Linux

```
sudo dpkg -i Nessus.deb
```

## Start the nessus service

Windows

Linux

<pre><code><strong>sudo /bin/systemctl start nessusd.service
</strong></code></pre>

## Update Nessus

