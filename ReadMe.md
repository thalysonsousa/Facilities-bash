

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
cat subjs.txt | while read -r url; do python3 /root/LinkFinder/linkfinder.py -i "$url" -o $(cat subjs.txt | grep $url | sed "s/https:\/\///g" | grep -Eo "^[^.]*").html; done
```

* use JsLuice to extract urls from js file
  
```
cat subjs.txt | while read -r url; do jsluice urls -R "$url" <(curl -sk "$url") | anew -q luice.$(cat subjs.txt | grep $url | sed "s/https:\/\///g" | grep -Eo "^[^.]*").txt; done
```


## Urls from Js Files

* use JQ to extract only urls from jsluice file output
  
```
ls jsluice/ | while read -r file; do cat jsluice/$file | jq -r '.url' | anew -q jq/jq.$(echo $file | sed "s/luice.//g"); done
```


## Extract Path from urls


*
```
cat urls.txt | grep -Eo "((http[s]?:\/\/).*\/)" | urldedupe | anew -q paths.txt; cat urls.txt | grep -Eo "((http[s]?:\/\/).[^/]+.)" | urldedupe | anew -q paths.txt; cat urls.txt | grep -Eo "((http[s]?:\/\/).[^/]+.[^/]+)" | urldedupe | anew -q paths.txt; cat urls.txt | grep -Eo "((http[s]?:\/\/).[^/]+.[^/]+.[^/]+)" | urldedupe | anew -q paths.txt

```

## Search in Path from files

```
cat paths.txt | httpx -mc 200 --path "/struts/utils.js" -ct | grep text/javascript
```

```
cat paths.txt | httpx -mc 200 --path "/struts/webconsole.js" -ct | grep text/javascript
```

```
cat paths.txt | httpx -mc 200 --path "/app_dev.php/_profiler/open?file=app/config/parameters.ym
l" -ms database_user
```
