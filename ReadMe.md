

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


### JS Files From SubJs

* use Linkfinder to analys all js urls and save output with subdomain name (e.g: sub.teste.com, save as sub.html)
  
```
cat $output-subjs-file | while read -r url; do python3 /root/LinkFinder/linkfinder.py -i "$url" -o $(cat $output-subjs-file | grep $url | sed "s/https:\/\///g" | grep -Eo "^[^.]*").html; done
```

* use JsLuice to extract urls from js file
  
```
cat $output-subjs-file | while read -r url; do jsluice urls -R "$url" <(curl -sk "$url") | anew -q luice.$(cat $output-subjs-file | grep $url | sed "s/https:\/\///g" | grep -Eo "^[^.]*").txt; done
```


## Urls from Js Files

* use JQ to extract only urls from jsluice file output
  
```
ls jsluice/ | while read -r file; do cat jsluice/$file | jq -r '.url' | anew -q jq/jq.$(echo $file | sed "s/luice.//g"); done
```
