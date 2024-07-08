
# Authentication

The API requires all requests to provide an authentication token in the **x-icmr-auth-1** request header.

First, compute the following values.

{% table %}

---
* **api_access_key**
* The API access key id (aka **kid**) provided to you.
---
* **timestamp**
* The current UTC time in **yyyyMMdd.HHmmss.SSS** format.
---
* **nonce**
* Random sequence of characters generated with each request.
{% /table %}

Build the **request_token** by concatenating the above strings, separated by single space " " characters.

```python
request_token = f"{api_access_key} {timestamp} {nonce} -" 
```

Then compute the following values.

{% table %}
---
* **http_method**
* The HTTP method of the request in full capital letters (e.g. GET, POST, PUT, DELETE)
---
* **request_path**
* The path of the request uri, including query string parameters
---
* **content_length**
* The value of the Content-Length header if present in the request or a single dash "-" character otherwise.
---
* **content_type**
* The value of the Content-Type header if present in the request or a single dash "-" character otherwise.
{% /table %}

Build the **request_metadata_token** by concatenating the above strings, separated by single space " " characters.

```python 
request_metadata_token = f"{http_method} {request_path} {content_length} {content_type}"
```

Then build the **unsigned_authentication_token** by concatenating the **request_token** and the **request_metadata_token**, separated by a single space " " character.

```python 
unsigned_authentication_token = f"{request_token} {request_metadata_token}"
```

To compute the **request_signature** take the UTF-8 encoded bytes of the **unsigned_authentication_token** and sign them using the HMAC-SHA265 algorithm using the UTF-8 encoded bytes of your **api_secret_key** (aka **shs**) as the signing key and then compute the Base64 representation of the resulting byte array.

```qqq
signature = hmac.new(
    api_secret_key.encode('utf-8'),
    unsigned_authentication_token.encode('utf-8'),
    hashlib.sha256
).digest()
request_signature = base64.b64encode(signature).decode('utf-8')
```

Finally compute the **authentication_token** by concatenating the **request_token** and the **request_signature**, again separated by a single space " " character.

```python
authentication_token = f"{request_token} {request_signature}"
```

Send the **authentication_token** in the **x-icmr-auth-1** header of the request.

```http
{http_method} {request_path} HTTP/1.1  
host: {host} 
x-icmr-auth-1: {authentication_token}
Content-Type: {content_type}  
Content-Length: {content_length}
```

## Server clock synchronization

The instantCMR API requires the **timestamp** to be within 15 minutes of the API server clock to prevent replay attacks. Should the difference be bigger the API will return a **401 Request time too skewed** response, including an **x-icmr-auth-1** header with the current server timestamp. The client should parse back the server timestamp, compare it to its own clock and adjust its timestamp accordingly in subsequent requests.

## Example

Lets assume that your **api-access-key** is **oh91tDqJySK8wur2V6ZNhg** and your **api-secret-key** is **HPlkr8Bwh0OESa7B8Lw4t5k\_yWg56ap7dsHEGUPaYU**. You want to send the request:

```qqq {% name="HTTP" %}
GET /v3/igr/dub/foo/bar/receive?expire=5&recid=00001 HTTP/1.1  
host: api.instantcmr.com  
```

The following example shows the steps to calculate the **x-icmr-auth-1** header.

```python
import hmac
import hashlib
import base64
import datetime
import uuid

# API credentials
api_access_key = 'oh91tDqJySK8wur2V6ZNhg'
api_secret_key = 'HPlkr8Bwh0OESa7B8Lw4t5k_yWg56ap7dsHEGUPaYU'

# Generate timestamp and nonce in the reqested format
timestamp = datetime.datetime.utcnow().strftime('%Y%m%d.%H%M%S.%f')[:-3]
nonce = str(uuid.uuid4())

# Request details
http_method = 'GET'
request_path = '/v3/igr/dub/foo/bar/receive?expire=5&recid=00001'
host = 'api.instantcmr.com'
content_type = '-'
content_length = '-'

# Create the x-icmr-auth-1 header
request_token = f"{api_access_key} {timestamp} {nonce} -"
request_metadata_token = f"{http_method} {request_path} {content_length} {content_type}"
unsigned_authentication_token = f"{request_token} {request_metadata_token}"
signature = hmac.new(
    api_secret_key.encode('utf-8'),
    unsigned_authentication_token.encode('utf-8'),
    hashlib.sha256
).digest()
request_signature = base64.b64encode(signature).decode('utf-8')

authentication_token = f"{request_token} {request_signature}"

print(authentication_token)
```

For the sake of demonstration, if we also pin down **timestamp** to **20171123.231834.311** and **nonce** to **d374ad26-6f8e-4d72-9004-4c713409bacd** the computed **authentication_token** should be:

```python
authentication_token = "oh91tDqJySK8wur2V6ZNhg 20171123.231834.311 d374ad26-6f8e-4d72-9004-4c713409bacd - cCalf3gwUOFaiLsTHWJSShGWem4cuyTFmFkquhzAbes="
```

Using these values, the final request including the **x-icmr-auth-1** header becomes:
```http
GET /v3/igr/dub/foo/bar/receive?expire=5&recid=00001 HTTP/1.1  
host: api.instantcmr.com  
x-icmr-auth-1: oh91tDqJySK8wur2V6ZNhg 20171123.231834.311 d374ad26-6f8e-4d72-9004-4c713409bacd - cCalf3gwUOFaiLsTHWJSShGWem4cuyTFmFkquhzAbes=
```
