# Sub Domain Finder

Pre requisites:

```
## Install subfinder
sudo apt-get install subfinder

## Install Assetfinder
sudo apt-get install assetfinder

## Install Aquatone
wget https://github.com/michenriksen/aquatone/releases/download/v1.7.0/aquatone_linux_amd64_1.7.0.zip
unzip aquatone_linux_amd64_1.7.0.zip
sudo mv aquatone /usr/local/bin/

## Install httpx-toolki
sudo apt-get install httpx-toolkit

## Install waybackurls
go install github.com/tomnomnom/waybackurls@latest
sudo mkdir /usr/local/go/
sudo mkdir /usr/local/go/bin/
sudo cp ~/go/bin/waybackurls /usr/local/bin/

## Add wordlists
cd /usr/share/wordlists/
git clone https://github.com/danielmiessler/SecLists.git

## Install Chromium
sudo apt-get install chromium
```

```
subfinder -d target.com | tee subf.txt (subdomain enumeration 1)
assetfinder -subs-only target.com | tee ast.txt (subdomain enum 2)

amass enum -passive -d target.com | tee amass.txt (subdomain enum 3)
cat subf.txt ast.txt amass.txt | sort -u | tee subdomains.txt (sorting sudomains)
cat subdomains.txt | httpx-toolkit | tee liveSubdomains.txt (filtering live subdomains)

cat liveSubdomains.txt | aquatone (taking screenshot of subdomains)
cat liveSubdomains.txt | gau | tee gau.txt (fetching urls 1)
cat liveSubdomains.txt | waybackurls | tee wayback.txt (fetching urls 2)

cat gau.txt wayback.txt | sort -u | fff | tee urls.txt (sorting)
cat urls.txt | gf xss | tee xss.txt (xss vulnerable endpoints)
cat urls.txt | gf ssrf| tee ssrf.txt (ssrf vulnerable endpoints)

ffuf -u https://target.com/FUZZ -w /root/Desktop/wordlistlocationfile.txt (fuzzing)

cat urls.txt | gf upload-fields| tee upload-fields.txt (upload-fieldsss vulnerable endpoints)
cat urls.txt | gf sqli| tee sqli.txt (sqli vulnerable endpoints)
cat urls.txt | gf rce| tee rce.txt (rce vulnerable endpoints)
```
