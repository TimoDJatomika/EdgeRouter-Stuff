# squidguard proxy
You can use your Edge Router as a proxy server to block certain categories e.g. ads or malware.

## prerequisite
SSH into your Edge Router and download the available catogories. Depending on your device this could take a few minutes.

```
update webproxy blacklists
```

## example config 
```
set service webproxy cache-size 0
set service webproxy default-port 3128
set service webproxy listen-address 172.22.3.1
set service webproxy mem-cache-size 5
set service webproxy url-filtering squidguard block-category ads
set service webproxy url-filtering squidguard block-category porn
set service webproxy url-filtering squidguard default-action allow
set service webproxy url-filtering squidguard redirect-url 'https://brainoftimo.com/not-for-you'
```
