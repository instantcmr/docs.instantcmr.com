
# Transaction API

The instantCMR Integration API allows you programmatic access to transactions uploaded by mobile users to the instantCMR backend. These transactions include CMR submissions, accident and goods damage reports and miscellaneous status reports. They contain the name and organization unit of the mobile user, the location of the mobile device at time of transaction, value of custom data fields entered by the mobile user and images, such as document scans or photos attached to the transaction.

When a transaction is uploaded by a mobile device it gets enqueued for delivery and processing on each integration endpoint set up to receive it. You can then use the integration endpoint to fetch unprocessed transactions, download imagery and mark transactions as processed.

Besides document submissions, the transaction API is also used to receive changes to the other components of the ecosystems such as trips, users and chat rooms.

## GET unprocessed updates
Initiates a HTTP long poll on integration endpoint identified by **iep**. When establishing the connection the client should employ an idle timeout of at least 40 seconds and ensure that no proxy between the client and the API uses a more eager timeout.

### Request
```http 
GET /v3/igr/dub/{copid}/{iep}/receive?expire={download-url-expiry}&recid={recid} HTTP/1.1  
Host: api.instantcmr.com  
x-icmr-auth-1: {authentication-token}
```

{% table %}
---
* **copid**
* Company identifier assigned by instantCMR
---
* **iep**
* Integration endpoint
---
* **download-url-expiry**
* The desired duration of the validity period until image data can be downloaded using the temporary image urls provided in the response. Provided in integral number of minutes (e.g. **1** means one minute). If omitted, the validity period of temporary urls is 15 minutes.
---
* **recid**
* Client-generated receive identifier.
---
* **authentication-token**
* See [Authentication](?p=/authentication#authentication).
{% /table %}

### Response

The API will keep the HTTP connection open until at least one entity update becomes available for processing on the endpoint or until the wait timeout elapses. It then responds immediately, sending all accrued transactions over in the response body.

The response body may contain up to 10 transactions. The client is expected to process each transaction within the preconfigured processing timeout (typically 3 minutes) and acknowledge successful processing of each transaction by sending back a [DELETE update](?p=/transaction-api#delete-update) request to the API.

The instantCMR API guarantees at-least-once delivery for each entity update. To ensure this, entity updates returned from [GET unprocessed updates](?p=/transaction-api#get-unprocessed-updates) will be eventually returned again unless the client marks those updates as processed by sending a [DELETE update](?p=/transaction-api#delete-update) request with the appropriate removal handle (**rhnd**) of the entity update.

The instantCMR API also guarantees in-order and exactly-once delivery for each entity update that is processed (and successfully marked as such) within the preconfigured processing timeout. To achieve this, entity updates returned to the client from [GET unprocessed updates](?p=/transaction-api#get-unprocessed-updates) will be marked as inflight by the API for the duration of the preconfigured processing timeout. While an entity update is marked as inflight, neither the same entity update, nor any other entity update corresponding to the same (trip, document, or user) entity will be returned from subsequent [GET unprocessed update](?p=/transaction-api#get-unprocessed-updates) requests. If the client does not mark the entity update as processed within the processing timeout, the update will go back to unprocessed state and become eligible for retrieval again.

If the client observes a network outage or temporary service outage (HTTP 429, HTTP 5XX) it should resend the same request after employing an exponential backoff. In this scenario the instantCMR API can only guarantee exactly-once delivery by keeping all entity updates that it previously returned to the original requestor as inflight and not returning them until the processing timeout elapses, effectively delaying those entity updates by several minutes. To avoid this, the client can decorate each [GET unprocessed updates](?p=/transaction-api#get-unprocessed-updates) with a randomly generated receive identifier (**recid**) and resend the same receive identifier when it retries the same [GET unprocessed updates](?p=/transaction-api#get-unprocessed-updates) request after a network error. The instantCMR API will return the same entity updates (even if they are inflight) to the client as long as it receives the same receive identifier as in the original request.


```http 
HTTP/1.1 200 Thank you  
Date: Fri, 24 Jan 2020 14:12:19 GMT  
Content-Type: application/json; charset=UTF-8  
Transfer-Encoding: chunked  

{  
    "rgdubm": [  
        {dubm1},  
        {dubm2},  
        …  

        {dubm10}  
     ]  

}  
```

### Response codes
{% table %}
---
* 200 OK
* Request was successful either because an entity update became available for processing or because the wait timeout has elapsed. You may repeat the request as soon as you are ready to receive further entity updates for processing. The response body contains the list of up to 10 unprocessed entity updates.
---
* 400 Bad Request
* The value of **download-url-expiry** parameter is invalid.
---
* 401 Unauthorized
* Missing or invalid **authentication-token**.
---
* 403 Forbidden
* Authenticated user does not have sufficient permission to access the integration endpoint.
---
* 404 Not Found
* Integration endpoint not found.
---
* 429 Too Many Requests
* Request has been throttled as user exceeded API quota.
{% /table %}

### Response body
{% table %}
---
* **rgdubm**
* list of [dubm](?p=/transaction-schemas#unprocessed-update-dubm)
* List of entity update records.
{% /table %}


## DELETE update

Removes an entity update (previously received as a response to a [GET unprocessed updates](?p=/transaction-api#get-unprocessed-updates) request) from the unprocessed entity update queue.

### Request
```http 
DELETE /v3/igr/dub/{copid}/{iep}/rhnd/{rhnd} HTTP/1.1  
Host: api.instantcmr.com  
x-icmr-auth-1: {authentication-token}  
```

{% table %}
---
* **copid**
* Company identifier assigned by instantCMR
---
* **iep**
* Integration endpoint
---
* **rhnd**
* The removal handle of the entity update to be removed from the unprocessed entity update queue.
---
* **authentication-token**
* See [Authentication](?p=/authentication#authentication).
{% /table %}


### Response
```http 
HTTP/1.1 200 Thank you  
Date: Fri, 24 Jan 2020 15:42:01 GMT  
Content-Type: text/html; charset=UTF-8  
Content-Length: 0  
```

### Response codes
{% table %}
---
* 200 OK
* Request was successful. The entity update has been removed from the unprocessed entity update queue.
---
* 400 Bad Request
* Invalid **rhnd**. Please note that the removal handle of the entity update may be different each time the entity update is retrieved by a [GET unprocessed updates](?p=/transaction-api#get-unprocessed-updates) request. Make sure you submit the latest **rhnd**.
---
* 401 Unauthorized
* Missing or invalid **authentication-token**.
---
* 403 Forbidden
* Authenticated user does not have sufficient permission to access the integration endpoint.
---
* 404 Not Found
* Integration endpoint not found.
---
* 429 Too Many Requests
* Request has been throttled as user exceeded API quota.
{% /table %}


## GET image

Downloads an image attached to a document.

Note, that images can receive updates as a result of reviewers manipulating images on the instantCMR Hub. Images can be reviewed (accepted or rejected), added to or removed from documents, or edited (cropped, rotated and adjusted). Such edits may or may not alter the binary representation of the attached image.

Document images are identified by the **Dubdosuimg.imgid** field, meaning that and document entity updates ([Dubdosu](?p=/transaction-schemas#document-dubdosu)) that contain updated data for the same attached image (such as a review, or edit) will contain a [Dubdosuimg](?p=/transaction-schemas#document-image-dubdosuimg) entity in its **Dubdosu.rgimg** field with the same **imgid** value.

If the binary content of an attached image has been changed, it will be signalled in the **Dubdosuimg.odoed.doedid** field. Hence the binary representation of attached images is guaranteed to stay identical if and only if both the document image identifier (**Dubdosuimg.imgid**) and the document edit identifier (**Dubdoed.doedid**) stays identical. These two identifiers combined identify the binary content of the attached image.

### Request
```http 
GET {image-url-path} HTTP/1.1  
Host: {image-url-host}  
x-icmr-auth-1: {authentication-token}  
```

{% table %}
---
* **image-url-path**
* The path part of the temporary image url specified in the **url** field of a document entity update returned by the API as a response to a [GET unprocessed updates](?p=/transaction-api#get-unprocessed-updates) request. See also [Dubdosuimg](?p=/transaction-schemas#document-image-dubdosuimg).
---
* **image-url-host**
* The host part of the temporary image url specified in the **url** field of a document entity update returned by the API as a response to a [GET unprocessed updates](?p=/transaction-api#get-unprocessed-updates) request. See also [Dubdosuimg](?p=/transaction-schemas#document-image-dubdosuimg).
---
* **authentication-token**
* See [Authentication](?p=/authentication#authentication).
{% /table %}


### Response
```http 
HTTP/1.1 200 Thank you  
Content-Length: 67676  
Content-Type: image/jpeg  
Etag: "16049243cba368ad9bea03033d6f2fb6"  
Last-Modified: Thu, 23 Nov 2017 21:14:23 GMT  

{image-bytes}
```

### Response codes
{% table %}
---
* 200 OK
* Request was successful. Response body contains the raw image bytes.
---
* 401 Unauthorized
* Missing or invalid **authentication-token**.
---
* 403 Forbidden
* The validity of the temporary image url has elapsed.
---
* 429 Too Many Requests
* Request has been throttled as user exceeded API quota.
{% /table %}
