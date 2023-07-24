# ScrapingBypass API 
Using CloudBypass can help you easily bypass Cloudflare's verification.
This document provides detailed usage methods of HTTP API mode and Proxy mode, including interface address, request parameters, return processing, etc.
## Curl
### API
#### Request
The API requests provided by this site are all communicated based on a secure encryption protocol. The following is the url address of the HTTP API:
```
https://api.cloudbypass.com
```

##### Request header (custom request parameters)
| Parameter | Type | Default | Required | Description |
| :----- | :----: | :----: | :----: | :----: | :-----|
| x-cb-apikey | string | APIkey | √ | your apikey |
| x-cb-host | string | - | √ | The target domain name of the request, such as: opensea.io |
| x-cb-protocol | string | "https" |  | Request protocol, such as: http, https |
| x-cb-proxy | string | - |   | Custom proxy address, which can be IP or domain name.
Support http, https protocol, such as: http://proxy.com:8080 or http://username:password:proxy.com:8080.
The protocol header is optional, if not filled, it defaults to http. |
| x-cb-version | string | - |   | When you need to use Cloud Piercer v2, the request header should be 2. |
| x-cb-part | string | - |   | This request header is only valid in Cloud Piercer v2 and is used to distinguish different sessions. Users can have up to 1000 session partitions. |

### Proxy

## Python
### API
### Proxy

## NodeJS
### API
### Proxy

## Java
### API
### Proxy

