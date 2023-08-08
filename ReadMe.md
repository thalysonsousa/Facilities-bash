

### Initial Configs


## Initial Files

* save subdomains
```
$ subfinder -d $DOMAIN -pc $CONFIG_SUBFINDER | httpx | anew -q $output-file
```
* save urls with js files
```
cat $output-file | subjs | anew -q $output-subjs-file
```


### LinkFinder Analys
* use Linkfinder to analys all js urls and save output with subdomain name (e.g: sub.teste.com, save as sub.html)
  
```
cat $output-subjs-file | while read -r url; do python3 /root/LinkFinder/linkfinder.py -i "$url" -o $(cat $output-subjs-file | grep $url | sed "s/https:\/\///g" | grep -Eo "^[^.]*").html; done
```

