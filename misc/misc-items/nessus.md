# Nessus



## Download Nessus

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

## Update Nessus

