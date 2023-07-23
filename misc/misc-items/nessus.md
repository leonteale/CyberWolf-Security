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

```
curl --request GET \
  --url 'https://www.tenable.com/downloads/api/v2/pages/nessus/files/Nessus-10.5.3-ubuntu1404_amd64.deb' \
  --output 'Nessus-10.5.3-ubuntu1404_amd64.deb'
```



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

