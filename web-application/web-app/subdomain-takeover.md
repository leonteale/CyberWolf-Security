# Subdomain takeover

{% embed url="https://github.com/martinvw/subdomain-takeover-tools" %}

## Create a subdomain list

```
subfinder -d domain.com -v -o subfinder.output.fle
```

## Test for domain takeover

```
subtake -f subfinder.output.fle -ssl -a -o subtake.results.txt -c /home/kali/go/src/github.com/jakejarvis/subtake/fingerprints.json -v
```

### Install subtake:

```
go get github.com/jakejarvis/subtake
```

