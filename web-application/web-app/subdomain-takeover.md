# Subdomain takeover

{% embed url="https://github.com/martinvw/subdomain-takeover-tools" %}

## Create a subdomain list

```
subfinder -d domain.com -v -o subfinder.output.fle
```

## Test for domain takeover

### subtake

#### Install subtake

```
go get github.com/jakejarvis/subtake
```

#### Run subtake

```
subtake -f subfinder.output.fle -ssl -a -o subtake.results.txt -c /home/kali/go/src/github.com/jakejarvis/subtake/fingerprints.json -v
```

### sub404

#### install sub404

```
git clone https://github.com/r3curs1v3-pr0xy/sub404.git
cd sub404
pip3 install -r requirements.txt
```

#### Run sub404

```
python3 sub404.py -d domain.com
python3 sub404 -f subfinder.output.fle
```

