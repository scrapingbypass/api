# ScrapingBypass API 
Using ScrapingBypass can help you easily bypass Cloudflare's verification.
This document provides detailed usage methods of HTTP API mode and Proxy mode, including interface address, request parameters, return processing, etc.
## Curl
### API
#### Request
> The API requests provided by this site are all communicated based on a secure encryption protocol. The following is the url address of the HTTP API:
```
https://api.cloudbypass.com
```

##### Request header (custom request parameters)
> Here is the full list of request headers for custom requests.

| **Parameter** | **Type** | **Default** | **Required** | **Description** |
|---------------|----------|-------------|--------------|----------------|
| x-cb-apikey   | string   | API key     | √            | your apikey |
| x-cb-host     | string   | -           | √            | The target domain name of the request, such as: opensea.io |
| x-cb-protocol | string   | "https"     |              | Request protocol, such as: http, https |
| x-cb-proxy    | string   | -           |              | 1. Custom proxy address, which can be IP or domain name. 2. Support http, https protocol, such as: http://proxy.com:8080 or http://username:password:proxy.com:8080. 3. The protocol header is optional, if not filled, it defaults to http. |
| x-cb-version  | string   | -           |              | When you need to use Cloud Piercer v2, the request header should be 2. |
| x-cb-part     | integer  | 0           |              | This request header is only valid in Cloud Piercer v2 and is used to distinguish different sessions. Users can have up to 1000 session partitions. |

##### Example
Visit `https://opensea.io/category/memberships`, the following is an example request:
```
# Use curl to request https://opensea.io/category/memberships
# curl -X GET "https://opensea.io/category/memberships"

# 使用 穿云API 请求示例
# Use Cloudbyapss API to request
curl -X GET "https://api.cloudbypass.com/category/memberships" ^
   -H "x-cb-apikey: YOUR_API_KEY" ^
   -H "x-cb-host: opensea.io" -k

```
#### Response
> If there is no error in the request process, the server will return all the response data requested by the proxy.  
When an error occurs, the server will put the error information into the response body, and you can judge it through our custom response parameters.

##### Response header (custom response parameters)
> Below is the full list of custom response headers.

| **Parameter** | **Mode**   | **Type** | **Description** |
|---------------|------------|----------|-----------------|
| x-cb-status   | -          | string   | Response status, returning ok means that the request has been processed successfully, and returning ok means that the request has failed to be processed.  The corresponding points will be deducted when returning ok. |
| x-cb-code     | Proxy only | string   | Error code, which identifies the type of error and can be used for troubleshooting.  When using API mode, the error code will be returned in the response body |

##### Response body
> If the response header `x-cb-status` is ok, the response body will be all the response data requested by the agent, if the response header x-cb-status is empty, the response body An error message will be returned.

##### Error message
The error message is a JSON object, the following is an example:
> id: request unique identifier, which can be used for troubleshooting.
> code: Error code, which is used to identify the type of error and can be used for troubleshooting.
> message: error message, used to describe the cause of the error.

```
{
  "id": "string",
  "code":"string",
  "message": "string"
}
```
##### Error code
> The following is a list of error codes, through which you can quickly locate the cause of the error.

| **Code**                     | **Mode**    | **Description**                                                                                                                                                    |
|------------------------------|-------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| PARAM_ERROR                  | -           | The parameter is wrong. It may be that the request parameter is incorrect or the required parameter is missing.                                                    |
| APIKEY_INVALID               | -           | The apikey is invalid, maybe your apikey does not exist or has been disabled.                                                                                      |
| AUTHORIZATION_REQUIRED       | Proxy only  | (Proxy mode) Authentication failed, maybe the apikey does not exist or has been disabled.                                                                          |
| HOSTNAME_IS_FORBIDDEN        | API only    | The requested host is forbidden. It may be that the interface address you requested is forbidden or your apikey does not have permission to access the interface.  |
| HOSTNAME_IS_NULL             | API only    | The requested host is empty, and the request header parameter x-cb-host cannot be empty.                                                                           |
| PROXY_ERROR                  | -           | Proxy error, either the proxy server was unable to connect or the proxy server returned an incorrect response.                                                     |
| PROXY_FORMAT_ERROR           | -           | The proxy format is wrong, only HTTP/HTTPS proxy protocol is supported.                                                                                            |
| PROXY_IS_FORBIDDEN           | -           | The proxy is forbidden. It may be that the proxy address you requested is forbidden or your apikey does not have permission to access the proxy.                   |
| CONNECTION_ERROR             | -           | Connection error, either the proxy server was unable to connect or the proxy server returned an incorrect response.                                                |
| DNS_ERROR                    | -           | DNS resolution error, it may be DNS resolution failure or DNS resolution timeout.                                                                                  |
| TLS_ERROR                    | -           | TLS client connection error, please check whether the requested address or proxy server address is correct.                                                        |
| UNSUPPORTED_CAPTCHA          | -           | The authentication type of the requested website address may not be supported.                                                                                     |
| CERT_ERROR                   | -           | The certificate is wrong. It may be that the certificate parsing failed or the certificate parsing timed out.                                                      |
| REQUEST_ERROR                | -           | The request was wrong, either the request timed out or the request was rejected.                                                                                   |
| EOF_ERROR                    | -           | EOF error, please check whether the requested address or proxy server address is correct.                                                                          |
| CLOUDFLARE_PREMIUM           | API v1 only | Protected by Cloudflare Premium, try Cloudbypass V2 requests.                                                                                                      |
| CLOUDFLARE_CHALLENGE_TIMEOUT | API v2 only | Cloudflare challenge timeout.                                                                                                                                      |
| FORBIDDEN                    | API only    | Access is restricted by the target website. For details, refer to the response message.                                                                            |
| INTERNAL_ERROR               | -           | Internal error, we will fix it as soon as possible after receiving the error message.                                                                              |

### Proxy
#### Ask
> Just replace the proxy service address with the proxy address of CloudBypass. The following is the proxy service address:

```
http://proxy.cloudbypass.com:1087
```
##### Request parameters
> Below is the full list of custom request parameters.

| **Parameter** | **Type** | **Default** | **Required** | **Description** |
|---------------|----------|-------------|--------------|-----------------|
| proxy         | string   | -           |              | Custom proxy address, which can be IP or domain name. Support http, https protocols, such as: http:proxy.com:8080 or http:username:password:proxy.com:8080. The protocol header is optional, if not filled, it defaults to http. |

##### Example
Visit `https://opensea.io/category/memberships`, the following is an example request:
```
# Use curl to request https://opensea.io/category/memberships
# curl -X GET "https://opensea.io/category/memberships"


# 使用 穿云Proxy 请求示例
# Use Cloudbyapss Proxy to request
curl -X GET "https://opensea.io/category/memberships" -x "http://YOUR_API_KEY:@proxy.cloudbypass.com:1087" -k
```

#### Response
> If there is no error in the request process, the server will return all the response data requested by the proxy.
> When an error occurs, the server will put the error information into the custom response header parameter.

##### Response header (custom response parameters)
> Below is the full list of custom response headers.

| **Parameter** | **Mode**   | **Type** | **Description** |
|---------------|------------|----------|-----------------|
| x-cb-status   | -          | string   | 	Response status, returning ok means that the request has been processed successfully, and returning ok means that the request has failed to be processed. The corresponding points will be deducted when returning ok. |
| x-cb-code     | Proxy only | string   | Error code, which identifies the type of error and can be used for troubleshooting. When using API mode, the error code will be returned in the response body |

##### Response body
> If the response header `x-cb-status` is ok, the response body will be all the response data requested by the agent. When `x-cb-status` is empty, the error code will be The response header `x-cb-code` is returned, and the response body is an error message.

##### Error code
> The following is a list of error codes, through which you can quickly locate the cause of the error.

| **Code**                     | **Mode**    | **Description**                                                                                                                                                   |
|------------------------------|-------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| PARAM_ERROR                  | -           | The parameter is wrong. It may be that the request parameter is incorrect or the required parameter is missing.                                                    |
| APIKEY_INVALID               | -           | The apikey is invalid, maybe your apikey does not exist or has been disabled.                                                                                      |
| AUTHORIZATION_REQUIRED       | Proxy only  | (Proxy mode) Authentication failed, maybe the apikey does not exist or has been disabled.                                                                          |
| HOSTNAME_IS_FORBIDDEN        | API only    | The requested host is forbidden. It may be that the interface address you requested is forbidden or your apikey does not have permission to access the interface.  |
| HOSTNAME_IS_NULL             | API only    | The requested host is empty, and the request header parameter x-cb-host cannot be empty.                                                                           |
| PROXY_ERROR                  | -           | Proxy error, either the proxy server was unable to connect or the proxy server returned an incorrect response.                                                     |
| PROXY_FORMAT_ERROR           | -           | The proxy format is wrong, only HTTP/HTTPS proxy protocol is supported.                                                                                            |
| PROXY_IS_FORBIDDEN           | -           | The proxy is forbidden. It may be that the proxy address you requested is forbidden or your apikey does not have permission to access the proxy.                   |
| CONNECTION_ERROR             | -           | Connection error, either the proxy server was unable to connect or the proxy server returned an incorrect response.                                                |
| DNS_ERROR                    | -           | DNS resolution error, it may be DNS resolution failure or DNS resolution timeout.                                                                                  |
| TLS_ERROR                    | -           | TLS client connection error, please check whether the requested address or proxy server address is correct.                                                        |
| UNSUPPORTED_CAPTCHA          | -           | The authentication type of the requested website address may not be supported.                                                                                     |
| CERT_ERROR                   | -           | The certificate is wrong. It may be that the certificate parsing failed or the certificate parsing timed out.                                                      |
| REQUEST_ERROR                | -           | The request was wrong, either the request timed out or the request was rejected.                                                                                   |
| EOF_ERROR                    | -           | EOF error, please check whether the requested address or proxy server address is correct.                                                                          |
| CLOUDFLARE_PREMIUM           | API v1 only | Protected by Cloudflare Premium, try Cloudbypass V2 requests.                                                                                                      |
| CLOUDFLARE_CHALLENGE_TIMEOUT | API v2 only | Cloudflare challenge timeout.                                                                                                                                      |
| FORBIDDEN                    | API only    | Access is restricted by the target website. For details, refer to the response message.                                                                            |
| INTERNAL_ERROR               | -           | Internal error, we will fix it as soon as possible after receiving the error message.                                                                              |

## Python
### API
#### Request
> The API requests provided by this site are all communicated based on a secure encryption protocol. The following is the url address of the HTTP API:
```
https://api.cloudbypass.com
```

##### Request header (custom request parameters)
> Here is the full list of request headers for custom requests.

| **Parameter** | **Type** | **Default** | **Required** | **Description** |
|---------------|----------|-------------|--------------|----------------|
| x-cb-apikey   | string   | API key     | √            | your apikey |
| x-cb-host     | string   | -           | √            | The target domain name of the request, such as: opensea.io |
| x-cb-protocol | string   | "https"     |              | Request protocol, such as: http, https |
| x-cb-proxy    | string   | -           |              | 1. Custom proxy address, which can be IP or domain name. 2. Support http, https protocol, such as: http://proxy.com:8080 or http://username:password:proxy.com:8080. 3. The protocol header is optional, if not filled, it defaults to http. |
| x-cb-version  | string   | -           |              | When you need to use Cloud Piercer v2, the request header should be 2. |
| x-cb-part     | integer  | 0           |              | This request header is only valid in Cloud Piercer v2 and is used to distinguish different sessions. Users can have up to 1000 session partitions. |

##### Example
Visit `https://opensea.io/category/memberships`, the following is an example request:
```
// Use python to request https://opensea.io/category/memberships
import requests

"""
# 修改前的代码示例
# original code
url = "https://opensea.io/category/memberships"
response = requests.request("GET", url)
print(response.text)
print(response.status_code,response.reason)
# (403, 'Forbidden')
"""

# 使用 穿云API 请求示例
# Use Cloudbyapss API to request
url = "https://api.cloudbypass.com/category/memberships"

headers = {
    'x-cb-apikey': 'YOUR_API_KEY',
    'x-cb-host': 'opensea.io',
}

response = requests.request("GET", url, headers=headers)

print(response.text)

```
#### Response
> If there is no error in the request process, the server will return all the response data requested by the proxy.  
When an error occurs, the server will put the error information into the response body, and you can judge it through our custom response parameters.

##### Response header (custom response parameters)
> Below is the full list of custom response headers.

| **Parameter** | **Mode**   | **Type** | **Description** |
|---------------|------------|----------|-----------------|
| x-cb-status   | -          | string   | Response status, returning ok means that the request has been processed successfully, and returning ok means that the request has failed to be processed.  The corresponding points will be deducted when returning ok. |
| x-cb-code     | Proxy only | string   | Error code, which identifies the type of error and can be used for troubleshooting.  When using API mode, the error code will be returned in the response body |

##### Response body
> If the response header `x-cb-status` is ok, the response body will be all the response data requested by the agent, if the response header x-cb-status is empty, the response body An error message will be returned.

##### Error message
The error message is a JSON object, the following is an example:
> id: request unique identifier, which can be used for troubleshooting.
> code: Error code, which is used to identify the type of error and can be used for troubleshooting.
> message: error message, used to describe the cause of the error.

```
{
  "id": "string",
  "code":"string",
  "message": "string"
}
```
##### Error code
> The following is a list of error codes, through which you can quickly locate the cause of the error.

| **Code**                     | **Mode**    | **Description**                                                                                                                                                    |
|------------------------------|-------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| PARAM_ERROR                  | -           | The parameter is wrong. It may be that the request parameter is incorrect or the required parameter is missing.                                                    |
| APIKEY_INVALID               | -           | The apikey is invalid, maybe your apikey does not exist or has been disabled.                                                                                      |
| AUTHORIZATION_REQUIRED       | Proxy only  | (Proxy mode) Authentication failed, maybe the apikey does not exist or has been disabled.                                                                          |
| HOSTNAME_IS_FORBIDDEN        | API only    | The requested host is forbidden. It may be that the interface address you requested is forbidden or your apikey does not have permission to access the interface.  |
| HOSTNAME_IS_NULL             | API only    | The requested host is empty, and the request header parameter x-cb-host cannot be empty.                                                                           |
| PROXY_ERROR                  | -           | Proxy error, either the proxy server was unable to connect or the proxy server returned an incorrect response.                                                     |
| PROXY_FORMAT_ERROR           | -           | The proxy format is wrong, only HTTP/HTTPS proxy protocol is supported.                                                                                            |
| PROXY_IS_FORBIDDEN           | -           | The proxy is forbidden. It may be that the proxy address you requested is forbidden or your apikey does not have permission to access the proxy.                   |
| CONNECTION_ERROR             | -           | Connection error, either the proxy server was unable to connect or the proxy server returned an incorrect response.                                                |
| DNS_ERROR                    | -           | DNS resolution error, it may be DNS resolution failure or DNS resolution timeout.                                                                                  |
| TLS_ERROR                    | -           | TLS client connection error, please check whether the requested address or proxy server address is correct.                                                        |
| UNSUPPORTED_CAPTCHA          | -           | The authentication type of the requested website address may not be supported.                                                                                     |
| CERT_ERROR                   | -           | The certificate is wrong. It may be that the certificate parsing failed or the certificate parsing timed out.                                                      |
| REQUEST_ERROR                | -           | The request was wrong, either the request timed out or the request was rejected.                                                                                   |
| EOF_ERROR                    | -           | EOF error, please check whether the requested address or proxy server address is correct.                                                                          |
| CLOUDFLARE_PREMIUM           | API v1 only | Protected by Cloudflare Premium, try Cloudbypass V2 requests.                                                                                                      |
| CLOUDFLARE_CHALLENGE_TIMEOUT | API v2 only | Cloudflare challenge timeout.                                                                                                                                      |
| FORBIDDEN                    | API only    | Access is restricted by the target website. For details, refer to the response message.                                                                            |
| INTERNAL_ERROR               | -           | Internal error, we will fix it as soon as possible after receiving the error message.                                                                              |

### Proxy
#### Ask
> Just replace the proxy service address with the proxy address of CloudBypass. The following is the proxy service address:

```
http://proxy.cloudbypass.com:1087
```
##### Request parameters
> Below is the full list of custom request parameters.

| **Parameter** | **Type** | **Default** | **Required** | **Description** |
|---------------|----------|-------------|--------------|-----------------|
| proxy         | string   | -           |              | Custom proxy address, which can be IP or domain name. Support http, https protocols, such as: http:proxy.com:8080 or http:username:password:proxy.com:8080. The protocol header is optional, if not filled, it defaults to http. |

##### Example
Visit `https://opensea.io/category/memberships`, the following is an example request:
```
# Use javascript to request https://opensea.io/category/memberships
import requests

"""
# 修改前的代码示例
# original code
url = "https://opensea.io/category/memberships"
response = requests.request("GET", url)
print(response.text)
print(response.status_code,response.reason)
# (403, 'Forbidden')
"""


# 使用 穿云Proxy 请求示例
# Use Cloudbyapss Proxy to request
url = "https://opensea.io/category/memberships"
proxies = {
    "http": "http://YOUR_API_KEY:@proxy.cloudbypass.com:1087",
    "https": "http://YOUR_API_KEY:@proxy.cloudbypass.com:1087"
}

# Use a custom proxy
# proxies = {
#     "http": "http://YOUR_API_KEY:proxy=http:CUSTOM_PROXY:8080@proxy.cloudbypass.com:1087",
#     "https": "http://YOUR_API_KEY:proxy=http:CUSTOM_PROXY:8080@proxy.cloudbypass.com:1087"
# }

response = requests.get(url, proxies=proxies)

print(response.text)
```

#### Response
> If there is no error in the request process, the server will return all the response data requested by the proxy.
> When an error occurs, the server will put the error information into the custom response header parameter.

##### Response header (custom response parameters)
> Below is the full list of custom response headers.

| **Parameter** | **Mode**   | **Type** | **Description** |
|---------------|------------|----------|-----------------|
| x-cb-status   | -          | string   | 	Response status, returning ok means that the request has been processed successfully, and returning ok means that the request has failed to be processed. The corresponding points will be deducted when returning ok. |
| x-cb-code     | Proxy only | string   | Error code, which identifies the type of error and can be used for troubleshooting. When using API mode, the error code will be returned in the response body |

##### Response body
> If the response header `x-cb-status` is ok, the response body will be all the response data requested by the agent. When `x-cb-status` is empty, the error code will be The response header `x-cb-code` is returned, and the response body is an error message.

##### Error code
> The following is a list of error codes, through which you can quickly locate the cause of the error.

| **Code**                     | **Mode**    | **Description**                                                                                                                                                   |
|------------------------------|-------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| PARAM_ERROR                  | -           | The parameter is wrong. It may be that the request parameter is incorrect or the required parameter is missing.                                                    |
| APIKEY_INVALID               | -           | The apikey is invalid, maybe your apikey does not exist or has been disabled.                                                                                      |
| AUTHORIZATION_REQUIRED       | Proxy only  | (Proxy mode) Authentication failed, maybe the apikey does not exist or has been disabled.                                                                          |
| HOSTNAME_IS_FORBIDDEN        | API only    | The requested host is forbidden. It may be that the interface address you requested is forbidden or your apikey does not have permission to access the interface.  |
| HOSTNAME_IS_NULL             | API only    | The requested host is empty, and the request header parameter x-cb-host cannot be empty.                                                                           |
| PROXY_ERROR                  | -           | Proxy error, either the proxy server was unable to connect or the proxy server returned an incorrect response.                                                     |
| PROXY_FORMAT_ERROR           | -           | The proxy format is wrong, only HTTP/HTTPS proxy protocol is supported.                                                                                            |
| PROXY_IS_FORBIDDEN           | -           | The proxy is forbidden. It may be that the proxy address you requested is forbidden or your apikey does not have permission to access the proxy.                   |
| CONNECTION_ERROR             | -           | Connection error, either the proxy server was unable to connect or the proxy server returned an incorrect response.                                                |
| DNS_ERROR                    | -           | DNS resolution error, it may be DNS resolution failure or DNS resolution timeout.                                                                                  |
| TLS_ERROR                    | -           | TLS client connection error, please check whether the requested address or proxy server address is correct.                                                        |
| UNSUPPORTED_CAPTCHA          | -           | The authentication type of the requested website address may not be supported.                                                                                     |
| CERT_ERROR                   | -           | The certificate is wrong. It may be that the certificate parsing failed or the certificate parsing timed out.                                                      |
| REQUEST_ERROR                | -           | The request was wrong, either the request timed out or the request was rejected.                                                                                   |
| EOF_ERROR                    | -           | EOF error, please check whether the requested address or proxy server address is correct.                                                                          |
| CLOUDFLARE_PREMIUM           | API v1 only | Protected by Cloudflare Premium, try Cloudbypass V2 requests.                                                                                                      |
| CLOUDFLARE_CHALLENGE_TIMEOUT | API v2 only | Cloudflare challenge timeout.                                                                                                                                      |
| FORBIDDEN                    | API only    | Access is restricted by the target website. For details, refer to the response message.                                                                            |
| INTERNAL_ERROR               | -           | Internal error, we will fix it as soon as possible after receiving the error message.                                                                              |

## NodeJS
### API
#### Request
> The API requests provided by this site are all communicated based on a secure encryption protocol. The following is the url address of the HTTP API:
```
https://api.cloudbypass.com
```

##### Request header (custom request parameters)
> Here is the full list of request headers for custom requests.

| **Parameter** | **Type** | **Default** | **Required** | **Description** |
|---------------|----------|-------------|--------------|----------------|
| x-cb-apikey   | string   | API key     | √            | your apikey |
| x-cb-host     | string   | -           | √            | The target domain name of the request, such as: opensea.io |
| x-cb-protocol | string   | "https"     |              | Request protocol, such as: http, https |
| x-cb-proxy    | string   | -           |              | 1. Custom proxy address, which can be IP or domain name. 2. Support http, https protocol, such as: http://proxy.com:8080 or http://username:password:proxy.com:8080. 3. The protocol header is optional, if not filled, it defaults to http. |
| x-cb-version  | string   | -           |              | When you need to use Cloud Piercer v2, the request header should be 2. |
| x-cb-part     | integer  | 0           |              | This request header is only valid in Cloud Piercer v2 and is used to distinguish different sessions. Users can have up to 1000 session partitions. |

##### Example
Visit `https://opensea.io/category/memberships`, the following is an example request:
```
// Use javascript to request https://opensea.io/category/memberships
const axios = require('axios');


/*
// 修改前的代码示例
// original code
const url = "https://opensea.io/category/memberships";
axios.get(url, {})
  .then(response => console.log(response.data))
  .catch(error => console.error(error));
*/


// 使用 穿云API 请求示例
// Use Cloudbyapss API to request
const url = "https://api.cloudbypass.com/path/to/target?a=4";
const headers = {
  'x-cb-apikey': 'YOUR_API_KEY',
  'x-cb-host': 'www.example.com',
};

axios.get(url, {}, {headers: headers})
  .then(response => console.log(response.data))
  .catch(error => console.error(error));
```
#### Response
> If there is no error in the request process, the server will return all the response data requested by the proxy.  
When an error occurs, the server will put the error information into the response body, and you can judge it through our custom response parameters.

##### Response header (custom response parameters)
> Below is the full list of custom response headers.

| **Parameter** | **Mode**   | **Type** | **Description** |
|---------------|------------|----------|-----------------|
| x-cb-status   | -          | string   | Response status, returning ok means that the request has been processed successfully, and returning ok means that the request has failed to be processed.  The corresponding points will be deducted when returning ok. |
| x-cb-code     | Proxy only | string   | Error code, which identifies the type of error and can be used for troubleshooting.  When using API mode, the error code will be returned in the response body |

##### Response body
> If the response header `x-cb-status` is ok, the response body will be all the response data requested by the agent, if the response header x-cb-status is empty, the response body An error message will be returned.

##### Error message
The error message is a JSON object, the following is an example:
> id: request unique identifier, which can be used for troubleshooting.
> code: Error code, which is used to identify the type of error and can be used for troubleshooting.
> message: error message, used to describe the cause of the error.

```
{
  "id": "string",
  "code":"string",
  "message": "string"
}
```
##### Error code
> The following is a list of error codes, through which you can quickly locate the cause of the error.

| **Code**                     | **Mode**    | **Description**                                                                                                                                                    |
|------------------------------|-------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| PARAM_ERROR                  | -           | The parameter is wrong. It may be that the request parameter is incorrect or the required parameter is missing.                                                    |
| APIKEY_INVALID               | -           | The apikey is invalid, maybe your apikey does not exist or has been disabled.                                                                                      |
| AUTHORIZATION_REQUIRED       | Proxy only  | (Proxy mode) Authentication failed, maybe the apikey does not exist or has been disabled.                                                                          |
| HOSTNAME_IS_FORBIDDEN        | API only    | The requested host is forbidden. It may be that the interface address you requested is forbidden or your apikey does not have permission to access the interface.  |
| HOSTNAME_IS_NULL             | API only    | The requested host is empty, and the request header parameter x-cb-host cannot be empty.                                                                           |
| PROXY_ERROR                  | -           | Proxy error, either the proxy server was unable to connect or the proxy server returned an incorrect response.                                                     |
| PROXY_FORMAT_ERROR           | -           | The proxy format is wrong, only HTTP/HTTPS proxy protocol is supported.                                                                                            |
| PROXY_IS_FORBIDDEN           | -           | The proxy is forbidden. It may be that the proxy address you requested is forbidden or your apikey does not have permission to access the proxy.                   |
| CONNECTION_ERROR             | -           | Connection error, either the proxy server was unable to connect or the proxy server returned an incorrect response.                                                |
| DNS_ERROR                    | -           | DNS resolution error, it may be DNS resolution failure or DNS resolution timeout.                                                                                  |
| TLS_ERROR                    | -           | TLS client connection error, please check whether the requested address or proxy server address is correct.                                                        |
| UNSUPPORTED_CAPTCHA          | -           | The authentication type of the requested website address may not be supported.                                                                                     |
| CERT_ERROR                   | -           | The certificate is wrong. It may be that the certificate parsing failed or the certificate parsing timed out.                                                      |
| REQUEST_ERROR                | -           | The request was wrong, either the request timed out or the request was rejected.                                                                                   |
| EOF_ERROR                    | -           | EOF error, please check whether the requested address or proxy server address is correct.                                                                          |
| CLOUDFLARE_PREMIUM           | API v1 only | Protected by Cloudflare Premium, try Cloudbypass V2 requests.                                                                                                      |
| CLOUDFLARE_CHALLENGE_TIMEOUT | API v2 only | Cloudflare challenge timeout.                                                                                                                                      |
| FORBIDDEN                    | API only    | Access is restricted by the target website. For details, refer to the response message.                                                                            |
| INTERNAL_ERROR               | -           | Internal error, we will fix it as soon as possible after receiving the error message.                                                                              |

### Proxy
#### Ask
> Just replace the proxy service address with the proxy address of CloudBypass. The following is the proxy service address:

```
http://proxy.cloudbypass.com:1087
```
##### Request parameters
> Below is the full list of custom request parameters.

| **Parameter** | **Type** | **Default** | **Required** | **Description** |
|---------------|----------|-------------|--------------|-----------------|
| proxy         | string   | -           |              | Custom proxy address, which can be IP or domain name. Support http, https protocols, such as: http:proxy.com:8080 or http:username:password:proxy.com:8080. The protocol header is optional, if not filled, it defaults to http. |

##### Example
Visit `https://opensea.io/category/memberships`, the following is an example request:
```
# Use javascript to request https://opensea.io/category/memberships
const axios = require('axios');

/*
// 修改前的代码示例
// original code
const url = "https://opensea.io/category/memberships";
axios.get(url, {})
  .then(response => console.log(response.data))
  .catch(error => console.error(error));
*/

// 使用 穿云Proxy 请求示例
// Use Cloudbyapss Proxy to request
const url = "https://opensea.io/category/memberships";
const config = {
    proxy: {
        host: 'proxy.cloudbypass.com',
        port: 1087,
        auth: {
            username: 'YOUR_API_KEY',
            password: ''
            // Use a custom proxy
            // password: 'proxy=http:CUSTOM_PROXY:8080'
        }
    }
};

axios.get(url, config)
  .then(response => console.log(response.data))
  .catch(error => console.error(error));
```

#### Response
> If there is no error in the request process, the server will return all the response data requested by the proxy.
> When an error occurs, the server will put the error information into the custom response header parameter.

##### Response header (custom response parameters)
> Below is the full list of custom response headers.

| **Parameter** | **Mode**   | **Type** | **Description** |
|---------------|------------|----------|-----------------|
| x-cb-status   | -          | string   | 	Response status, returning ok means that the request has been processed successfully, and returning ok means that the request has failed to be processed. The corresponding points will be deducted when returning ok. |
| x-cb-code     | Proxy only | string   | Error code, which identifies the type of error and can be used for troubleshooting. When using API mode, the error code will be returned in the response body |

##### Response body
> If the response header `x-cb-status` is ok, the response body will be all the response data requested by the agent. When `x-cb-status` is empty, the error code will be The response header `x-cb-code` is returned, and the response body is an error message.

##### Error code
> The following is a list of error codes, through which you can quickly locate the cause of the error.

| **Code**                     | **Mode**    | **Description**                                                                                                                                                   |
|------------------------------|-------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| PARAM_ERROR                  | -           | The parameter is wrong. It may be that the request parameter is incorrect or the required parameter is missing.                                                    |
| APIKEY_INVALID               | -           | The apikey is invalid, maybe your apikey does not exist or has been disabled.                                                                                      |
| AUTHORIZATION_REQUIRED       | Proxy only  | (Proxy mode) Authentication failed, maybe the apikey does not exist or has been disabled.                                                                          |
| HOSTNAME_IS_FORBIDDEN        | API only    | The requested host is forbidden. It may be that the interface address you requested is forbidden or your apikey does not have permission to access the interface.  |
| HOSTNAME_IS_NULL             | API only    | The requested host is empty, and the request header parameter x-cb-host cannot be empty.                                                                           |
| PROXY_ERROR                  | -           | Proxy error, either the proxy server was unable to connect or the proxy server returned an incorrect response.                                                     |
| PROXY_FORMAT_ERROR           | -           | The proxy format is wrong, only HTTP/HTTPS proxy protocol is supported.                                                                                            |
| PROXY_IS_FORBIDDEN           | -           | The proxy is forbidden. It may be that the proxy address you requested is forbidden or your apikey does not have permission to access the proxy.                   |
| CONNECTION_ERROR             | -           | Connection error, either the proxy server was unable to connect or the proxy server returned an incorrect response.                                                |
| DNS_ERROR                    | -           | DNS resolution error, it may be DNS resolution failure or DNS resolution timeout.                                                                                  |
| TLS_ERROR                    | -           | TLS client connection error, please check whether the requested address or proxy server address is correct.                                                        |
| UNSUPPORTED_CAPTCHA          | -           | The authentication type of the requested website address may not be supported.                                                                                     |
| CERT_ERROR                   | -           | The certificate is wrong. It may be that the certificate parsing failed or the certificate parsing timed out.                                                      |
| REQUEST_ERROR                | -           | The request was wrong, either the request timed out or the request was rejected.                                                                                   |
| EOF_ERROR                    | -           | EOF error, please check whether the requested address or proxy server address is correct.                                                                          |
| CLOUDFLARE_PREMIUM           | API v1 only | Protected by Cloudflare Premium, try Cloudbypass V2 requests.                                                                                                      |
| CLOUDFLARE_CHALLENGE_TIMEOUT | API v2 only | Cloudflare challenge timeout.                                                                                                                                      |
| FORBIDDEN                    | API only    | Access is restricted by the target website. For details, refer to the response message.                                                                            |
| INTERNAL_ERROR               | -           | Internal error, we will fix it as soon as possible after receiving the error message. 

## Java
### API
#### Request
> The API requests provided by this site are all communicated based on a secure encryption protocol. The following is the url address of the HTTP API:
```
https://api.cloudbypass.com
```

##### Request header (custom request parameters)
> Here is the full list of request headers for custom requests.

| **Parameter** | **Type** | **Default** | **Required** | **Description** |
|---------------|----------|-------------|--------------|----------------|
| x-cb-apikey   | string   | API key     | √            | your apikey |
| x-cb-host     | string   | -           | √            | The target domain name of the request, such as: opensea.io |
| x-cb-protocol | string   | "https"     |              | Request protocol, such as: http, https |
| x-cb-proxy    | string   | -           |              | 1. Custom proxy address, which can be IP or domain name. 2. Support http, https protocol, such as: http://proxy.com:8080 or http://username:password:proxy.com:8080. 3. The protocol header is optional, if not filled, it defaults to http. |
| x-cb-version  | string   | -           |              | When you need to use Cloud Piercer v2, the request header should be 2. |
| x-cb-part     | integer  | 0           |              | This request header is only valid in Cloud Piercer v2 and is used to distinguish different sessions. Users can have up to 1000 session partitions. |

##### Example
Visit `https://opensea.io/category/memberships`, the following is an example request:
```
// Use java to request https://opensea.io/category/memberships
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;


public class Main {
    public static void main(String[] args) throws Exception {
        /*
            // 修改前的代码示例
            // original code
            String url = "https://opensea.io/category/memberships";
            HttpClient client = HttpClient.newHttpClient();
            HttpRequest request = HttpRequest.newBuilder()
                            .uri(URI.create(url))
                            .GET(HttpRequest.BodyPublishers.noBody())
                            .build();

            HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());
            System.out.println(response.body());
        */

        // 使用 穿云API 请求示例
        // Use Cloudbyapss API to request
        String url = "https://api.cloudbypass.com/category/memberships";
        HttpClient client = HttpClient.newHttpClient();
        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create(url))
                .header("x-cb-apikey", "YOUR_API_KEY")
                .header("x-cb-host", "opensea.io")
                .GET(HttpRequest.BodyPublishers.noBody())
                .build();

        HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());
        System.out.println(response.body());
    }
}

```
#### Response
> If there is no error in the request process, the server will return all the response data requested by the proxy.  
When an error occurs, the server will put the error information into the response body, and you can judge it through our custom response parameters.

##### Response header (custom response parameters)
> Below is the full list of custom response headers.

| **Parameter** | **Mode**   | **Type** | **Description** |
|---------------|------------|----------|-----------------|
| x-cb-status   | -          | string   | Response status, returning ok means that the request has been processed successfully, and returning ok means that the request has failed to be processed.  The corresponding points will be deducted when returning ok. |
| x-cb-code     | Proxy only | string   | Error code, which identifies the type of error and can be used for troubleshooting.  When using API mode, the error code will be returned in the response body |

##### Response body
> If the response header `x-cb-status` is ok, the response body will be all the response data requested by the agent, if the response header x-cb-status is empty, the response body An error message will be returned.

##### Error message
The error message is a JSON object, the following is an example:
> id: request unique identifier, which can be used for troubleshooting.
> code: Error code, which is used to identify the type of error and can be used for troubleshooting.
> message: error message, used to describe the cause of the error.

```
{
  "id": "string",
  "code":"string",
  "message": "string"
}
```
##### Error code
> The following is a list of error codes, through which you can quickly locate the cause of the error.

| **Code**                     | **Mode**    | **Description**                                                                                                                                                    |
|------------------------------|-------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| PARAM_ERROR                  | -           | The parameter is wrong. It may be that the request parameter is incorrect or the required parameter is missing.                                                    |
| APIKEY_INVALID               | -           | The apikey is invalid, maybe your apikey does not exist or has been disabled.                                                                                      |
| AUTHORIZATION_REQUIRED       | Proxy only  | (Proxy mode) Authentication failed, maybe the apikey does not exist or has been disabled.                                                                          |
| HOSTNAME_IS_FORBIDDEN        | API only    | The requested host is forbidden. It may be that the interface address you requested is forbidden or your apikey does not have permission to access the interface.  |
| HOSTNAME_IS_NULL             | API only    | The requested host is empty, and the request header parameter x-cb-host cannot be empty.                                                                           |
| PROXY_ERROR                  | -           | Proxy error, either the proxy server was unable to connect or the proxy server returned an incorrect response.                                                     |
| PROXY_FORMAT_ERROR           | -           | The proxy format is wrong, only HTTP/HTTPS proxy protocol is supported.                                                                                            |
| PROXY_IS_FORBIDDEN           | -           | The proxy is forbidden. It may be that the proxy address you requested is forbidden or your apikey does not have permission to access the proxy.                   |
| CONNECTION_ERROR             | -           | Connection error, either the proxy server was unable to connect or the proxy server returned an incorrect response.                                                |
| DNS_ERROR                    | -           | DNS resolution error, it may be DNS resolution failure or DNS resolution timeout.                                                                                  |
| TLS_ERROR                    | -           | TLS client connection error, please check whether the requested address or proxy server address is correct.                                                        |
| UNSUPPORTED_CAPTCHA          | -           | The authentication type of the requested website address may not be supported.                                                                                     |
| CERT_ERROR                   | -           | The certificate is wrong. It may be that the certificate parsing failed or the certificate parsing timed out.                                                      |
| REQUEST_ERROR                | -           | The request was wrong, either the request timed out or the request was rejected.                                                                                   |
| EOF_ERROR                    | -           | EOF error, please check whether the requested address or proxy server address is correct.                                                                          |
| CLOUDFLARE_PREMIUM           | API v1 only | Protected by Cloudflare Premium, try Cloudbypass V2 requests.                                                                                                      |
| CLOUDFLARE_CHALLENGE_TIMEOUT | API v2 only | Cloudflare challenge timeout.                                                                                                                                      |
| FORBIDDEN                    | API only    | Access is restricted by the target website. For details, refer to the response message.                                                                            |
| INTERNAL_ERROR               | -           | Internal error, we will fix it as soon as possible after receiving the error message.                                                                              |

### Proxy
#### Ask
> Just replace the proxy service address with the proxy address of CloudBypass. The following is the proxy service address:

```
http://proxy.cloudbypass.com:1087
```
##### Request parameters
> Below is the full list of custom request parameters.

| **Parameter** | **Type** | **Default** | **Required** | **Description** |
|---------------|----------|-------------|--------------|-----------------|
| proxy         | string   | -           |              | Custom proxy address, which can be IP or domain name. Support http, https protocols, such as: http:proxy.com:8080 or http:username:password:proxy.com:8080. The protocol header is optional, if not filled, it defaults to http. |

##### Example
Visit `https://opensea.io/category/memberships`, the following is an example request:
```
# Use java to request https://opensea.io/category/memberships
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;

public class Main {
    public static void main(String[] args) throws Exception {
        /*
            // 修改前的代码示例
            // original code
            String url = "https://opensea.io/category/memberships";
            HttpClient client = HttpClient.newHttpClient();
            HttpRequest request = HttpRequest.newBuilder()
                            .uri(URI.create(url))
                            .GET(HttpRequest.BodyPublishers.noBody())
                            .build();

            HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());
            System.out.println(response.body());
        */


        // 使用 穿云Proxy 请求示例
        // Use Cloudbyapss Proxy to request
        String url = "https://opensea.io/category/memberships";
        HttpClient client = HttpClient.newBuilder()
                .proxy(HttpClient
                        .ProxySelector
                        // Use a custom proxy
                        //.of(URI.create("http://YOUR_API_KEY:proxy=http:CUSTOM_PROXY:8080@proxy.cloudbypass.com:1087")))
                        .of(URI.create("http://YOUR_API_KEY:@proxy.cloudbypass.com:1087")))
                .build();
        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create(url))
                .GET()
                .build();
        HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());
        System.out.println(response.body());
    }
}

```

#### Response
> If there is no error in the request process, the server will return all the response data requested by the proxy.
> When an error occurs, the server will put the error information into the custom response header parameter.

##### Response header (custom response parameters)
> Below is the full list of custom response headers.

| **Parameter** | **Mode**   | **Type** | **Description** |
|---------------|------------|----------|-----------------|
| x-cb-status   | -          | string   | 	Response status, returning ok means that the request has been processed successfully, and returning ok means that the request has failed to be processed. The corresponding points will be deducted when returning ok. |
| x-cb-code     | Proxy only | string   | Error code, which identifies the type of error and can be used for troubleshooting. When using API mode, the error code will be returned in the response body |

##### Response body
> If the response header `x-cb-status` is ok, the response body will be all the response data requested by the agent. When `x-cb-status` is empty, the error code will be The response header `x-cb-code` is returned, and the response body is an error message.

##### Error code
> The following is a list of error codes, through which you can quickly locate the cause of the error.

| **Code**                     | **Mode**    | **Description**                                                                                                                                                   |
|------------------------------|-------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| PARAM_ERROR                  | -           | The parameter is wrong. It may be that the request parameter is incorrect or the required parameter is missing.                                                    |
| APIKEY_INVALID               | -           | The apikey is invalid, maybe your apikey does not exist or has been disabled.                                                                                      |
| AUTHORIZATION_REQUIRED       | Proxy only  | (Proxy mode) Authentication failed, maybe the apikey does not exist or has been disabled.                                                                          |
| HOSTNAME_IS_FORBIDDEN        | API only    | The requested host is forbidden. It may be that the interface address you requested is forbidden or your apikey does not have permission to access the interface.  |
| HOSTNAME_IS_NULL             | API only    | The requested host is empty, and the request header parameter x-cb-host cannot be empty.                                                                           |
| PROXY_ERROR                  | -           | Proxy error, either the proxy server was unable to connect or the proxy server returned an incorrect response.                                                     |
| PROXY_FORMAT_ERROR           | -           | The proxy format is wrong, only HTTP/HTTPS proxy protocol is supported.                                                                                            |
| PROXY_IS_FORBIDDEN           | -           | The proxy is forbidden. It may be that the proxy address you requested is forbidden or your apikey does not have permission to access the proxy.                   |
| CONNECTION_ERROR             | -           | Connection error, either the proxy server was unable to connect or the proxy server returned an incorrect response.                                                |
| DNS_ERROR                    | -           | DNS resolution error, it may be DNS resolution failure or DNS resolution timeout.                                                                                  |
| TLS_ERROR                    | -           | TLS client connection error, please check whether the requested address or proxy server address is correct.                                                        |
| UNSUPPORTED_CAPTCHA          | -           | The authentication type of the requested website address may not be supported.                                                                                     |
| CERT_ERROR                   | -           | The certificate is wrong. It may be that the certificate parsing failed or the certificate parsing timed out.                                                      |
| REQUEST_ERROR                | -           | The request was wrong, either the request timed out or the request was rejected.                                                                                   |
| EOF_ERROR                    | -           | EOF error, please check whether the requested address or proxy server address is correct.                                                                          |
| CLOUDFLARE_PREMIUM           | API v1 only | Protected by Cloudflare Premium, try Cloudbypass V2 requests.                                                                                                      |
| CLOUDFLARE_CHALLENGE_TIMEOUT | API v2 only | Cloudflare challenge timeout.                                                                                                                                      |
| FORBIDDEN                    | API only    | Access is restricted by the target website. For details, refer to the response message.                                                                            |
| INTERNAL_ERROR               | -           | Internal error, we will fix it as soon as possible after receiving the error message. 
