# Awesome BugHunting Oneliners 
##### A curated list of bugunting one liners

## Subdomain Enumeration 

#### 1 .  dns.bufferover.run 
```
curl -s https://dns.bufferover.run/dns?q=.example.com |jq -r .FDNS_A[]|cut -d',' -f2|sort -u
```
####  2 . crt.sh 
```
curl -s https://dns.bufferover.run/dns?q=.hackerone.com |jq -r .FDNS_A[]|cut -d',' -f2|sort -u
```


```
curl 'https:​//crt​.sh/?q=%.example​.com&output=json' | jq '.name_value' | sed 's/\"//g' | sed 's/\*\.//g' | sort -u
```

#### 3 . certspotter

```
curl https://certspotter.com/api/v0/certs\?domain\=example.com | jq '.[].dns_names[]' | sed 's/\"//g' | sed 's/\*\.//g' | uniq
```

**With port scanning** 
```
curl https://certspotter.com/api/v0/certs\?domain\=example.com | jq '.[].dns_names[]' | sed 's/\"//g' | sed 's/\*\.//g' | uniq | dig +short -f - | uniq | nmap -T5 -Pn -sS -i - -p 80,443,21,22,8080,8081,8443 --open -n -oG -
```
### Sublist3r One Liner 
```
. <(cat domains | xargs -n1 -i{} python sublist3r.py -d {} -o {}.txt)
```


