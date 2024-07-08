# Custom Menu API

The Custom Menu API allows developers to create and manage a personalized interface for drivers, enhancing their experience with tailored functionalities. This API is structured around two main components: the Custom Menu (**[Scren](?p=/custom-menu-schemas#custom-menu-scren)**) and Widgets (**[Mbbut](?p=/custom-menu-schemas#widget-mbut)**). The Custom Menu serves as the container for various widgets, each performing specific actions or displaying pertinent information. By using this API, developers can design a menu layout that best fits the needs of drivers, integrating essential features directly into their workflow.

By leveraging this API, developers can craft a user-friendly and functional interface that streamlines a driver's daily operations, improving both efficiency and usability.


## GET scren

Downloads the custom menu of a user if present.

### Request
```http 
GET /v3/igr/user/{copid}/{userxtid}/scren HTTP/1.1  
Host: api.instantcmr.com  
x-icmr-auth-1: {authentication-token}  
```

{% table %}
---
* **copid**
* Company identifier assigned by instantCMR
---
* **userxtid**
* Client-generated user identifier
---
* **authentication-token**
* See [Authentication](?p=/authentication#authentication).
{% /table %}

### Response
```http 
HTTP/1.1 200 OK  
Date: Thu, 23 Jan 2020 16:29:47 GMT  
Content-Type: application/json; charset=UTF-8  
ETag: {ETag}
Content-Length: 1386  
Server: Jetty(9.2.1.v20140609)  

{scren}
```

### Response codes

{% table %}
---
* 200 OK
* Request was successful. Response contains the custom menu entity in the response body and its entity version tag in the **ETag** response header.
---
* 401 Unauthorized
* Missing or invalid **authentication-token**.
---
* 403 Forbidden
* Authenticated user does not have sufficient permission to access users of company designated by **copid**.
---
* 404 Not Found
* User with id of **userxtid** not found or the user does not have a custom menu.
---
* 429 Too Many Requests
* Request has been throttled as user exceeded API quota.
{% /table %}

### Response headers

{% table %}
---
* **ETag**
* string
* Entity version tag. Must be supplied in subsequent [PUT scren](?p=/custom-menu-api#put-scren) requests in the **If-Match** header.
{% /table %}

### Response body
{% table %}
---
* **scren**
* [scren](?p=/custom-menu-schemas#custom-menu-scren)
* Custom menu response entity.
{% /table %}



## PUT scren

Stores a custom menu of the user specified by **userxtid**, optionally updating the previous custom menu of the user if present.

### Request
```http 
PUT /v3/igr/user/{copid}/{userxtid}/scren HTTP/1.1  
Host: api.instantcmr.com  
[If-Match: {ETag}]  
[If-None-Match: *]  
x-icmr-auth-1: {authentication-token}  

{scren}
```

{% table %}
---
* **copid**
* Company identifier assigned by instantCMR
---
* **userxtid**
* Client-generated user identifier
---
* **ETag**
* Entity version tag. Provide previous custom menu entity version tag when updating an already existing custom menu entity. When creating a new entity pass **‘\*’** in the **If-None-Match** header.
---
* **authentication-token**
* See [Authentication](?p=/authentication#authentication).
{% /table %}

### Response
```http 
HTTP/1.1 200 OK  
Date: Thu, 23 Jan 2020 16:29:47 GMT  
Content-Type: application/json; charset=UTF-8  
ETag: {ETag}
Content-Length: 1386  
Server: Jetty(9.2.1.v20140609)  

{scren}
```

### Response codes
{% table %}
---
* 200 OK
* Request was successful. Response contains the updated custom menu entity in the response body and its updated entity version tag in the **ETag** response header.
---
* 401 Unauthorized
* Missing or invalid **authentication-token**.
---
* 403 Forbidden
* Authenticated user does not have sufficient permission to access users of company designated by **copid**.
---
* 404 Not Found
* User with id of **userxtid** not found or the user does not have a custom menu and an **If-Match** header was sent.
---
* 412 PreconditionFailed
* **ETag** specified in **If-Match** or **If-None-Match** headers did not match the current custom menu entity version. Retrieve the current custom menu entity using [GET scren](?p=/custom-menu-api#get-scren), reapply the changes, and retry the [PUT scren](?p=/custom-menu-api#put-scren) request with updated **ETag**.
---
* 429 Too Many Requests
* Request has been throttled as user exceeded API quota.
{% /table %}

### Response headers
{% table %}
---
* **ETag**
* string
* Entity version tag. Must be supplied in subsequent [PUT scren](?p=/custom-menu-api#put-scren) requests in the **If-Match** header.
{% /table %}

### Response body
{% table %}
---
* **scren**
* [scren](?p=/custom-menu-schemas#custom-menu-scren)
* Custom menu response entity.
{% /table %}



## DELETE scren

Removes the custom menu of the user specified by **userxtid**.

### Request
```http 
DELETE /v3/igr/user/{copid}/{userxtid}/scren HTTP/1.1  
Host: api.instantcmr.com  
[If-Match: {ETag}]  
[If-None-Match: *]  
x-icmr-auth-1: {authentication-token}  
```

{% table %}
---
* **copid**
* Company identifier assigned by instantCMR
---
* **userxtid**
* Client-generated user identifier
---
* **ETag**
* Entity version tag. Provide previous custom menu entity version tag when updating an already existing custom menu entity.
---
* **authentication-token**
* See [Authentication](?p=/authentication#authentication).
{% /table %}

### Response
```http 
HTTP/1.1 200 OK  
Date: Thu, 23 Jan 2020 16:29:47 GMT  
Server: Jetty(9.2.1.v20140609)  
```

### Response codes
{% table %}
---
* 200 OK
* Request was successful. Response contains the updated custom menu entity in the response body and its updated entity version tag in the **ETag** response header.
---
* 401 Unauthorized
* Missing or invalid **authentication-token**.
---
* 403 Forbidden
* Authenticated user does not have sufficient permission to access users of company designated by **copid**.
---
* 404 Not Found
* User with id of **userxtid** not found or the user does not have a custom menu and an **If-Match** header was sent.
---
* 412 PreconditionFailed
* **ETag** specified in **If-Match** or **If-None-Match** headers did not match the current custom menu entity version. Retrieve the current custom menu entity using [GET scren](?p=/custom-menu-api#get-scren) and retry the [DELETE scren](?p=/custom-menu-api#delete-scren) request with updated **ETag**.
---
* 429 Too Many Requests
* Request has been throttled as user exceeded API quota.
{% /table %}
