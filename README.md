
# Awesome Bughunting Oneliners

### A  list of bughunting one liners

  

##  Content Discovery/Recon : 

  

### 1 . Using dns.bufferover.run

```
curl -s https://dns.bufferover.run/dns?q=.example.com |jq -r .FDNS_A[]|cut -d',' -f2|sort -u
```

### 2 . Using Crt.sh

```
curl -s https://dns.bufferover.run/dns?q=.hackerone.com |jq -r .FDNS_A[]|cut -d',' -f2|sort -u
```
  

###  3 . Using Certspotter

```
curl https://certspotter.com/api/v0/certs\?domain\=example.com | jq '.[].dns_names[]' | sed 's/\"//g' | sed 's/\*\.//g' | uniq
```
  

### 4 . Using Certspotter (With port scanning)

```
curl https://certspotter.com/api/v0/certs\?domain\=example.com | jq '.[].dns_names[]' | sed 's/\"//g' | sed 's/\*\.//g' | uniq | dig +short -f - | uniq | nmap -T5 -Pn -sS -i - -p 80,443,21,22,8080,8081,8443 --open -n -oG -
```

###  5 .  Sublist3r One Liner

```
. <(cat domains | xargs -n1 -i{} python sublist3r.py -d {} -o {}.txt)
```

###  6 . Grab Titles of webpages 

```
for i in $(cat Webservers.txt ); do echo "$i | $(curl --connect-timeout 0.5 $i -so - | grep -iPo '(?<=<title>)(.*)(?=</title>)')"; done 
```

###  7 . Enumerate hosts from SSL Certificate 

```
echo | openssl s_client -connect https://targetdomain.com:443 | openssl x509 -noout -text | grep DNS
```

###  8 . Google DNS via HTTPS

```
echo "targetdomain.com" | xargs -I domain proxychains curl -s "https://dns.google.com/resolve?name=domain&type=A" | jq .
```

###  9 .  CommonCrawl to find endpoints on a site

```
echo "targetdomain.com" | xargs -I domain curl -s "http://index.commoncrawl.org/CC-MAIN-2018-22-index?url=*.domain&output=json" | jq -r .url | sort -u
```

###  10 . Using WebArchive

```
curl -s "http://web.archive.org/cdx/search/cdx?url=*.hackerone.com/*&output=text&fl=original&collapse=urlkey" |sort| sed -e 's_https*://__' -e "s/\/.*//" -e 's/:.*//' -e 's/^www\.//' | uniq
``` 

###  11 . Using ThreatCrowd

```
curl https://www.threatcrowd.org/searchApi/v2/domain/report/?domain=hackerone.com |jq .subdomains |grep -o '\w.*hackerone.com'
```

###  12 . Using Hackertarget

```
curl https://api.hackertarget.com/hostsearch/?q=hackerone.com | grep -o '\w.*hackerone.com'
```

###  13 . Bruteforce Subdomains

```
while read sub; do if host "$sub.example.com" &> /dev/null; then echo "$sub.example.com"; fi; done < wordslist.txt
```

###  14 .  Assetfinder 

```
assetfinder http://hackerone.com > recon.txt; for d in $(<recon.txt); do $(cutycapt --url=$d --out=$d.jpg --max-wait=100000); done
```

## Note : 

```
These oneliners are collected from different sources , Credits to the respesctive authors
```

## Contribute : 

```
Open Pull requests
```

## __Want to support my work?__
If you think my work has added some value to your existing knowledge, then you can [Buy me a Coffee here](https://www.buymeacoffee.com/Asm0d3us) (and who doesn't loves a good cup of coffee?')


[![name](https://img.buymeacoffee.com/api/?url=aHR0cHM6Ly9jZG4uYnV5bWVhY29mZmVlLmNvbS91cGxvYWRzL3Byb2ZpbGVfcGljdHVyZXMvMjAyMS8wOS8wMGU4ZGJjODc0NzI0MmRjYTJmNGJkMmMzMzQ1ODUzZC5wbmdAMzAwd18wZS53ZWJw&creator=Asm0d3us&is_creating=creating%20educational%20cybersecurity%20related%20content.&design_code=1&design_color=%235F7FFF&slug=Asm0d3us)](https://www.buymeacoffee.com/Asm0d3us)
