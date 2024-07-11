# Trip API

The Trip API lets integration clients manage trip information sent to drivers' mobile devices, including trip metadata, document requests, station and task list, external documents, such as information about site entry, exit, loading or unloading procedures.

Trip entities are requested by [GET trip](?p=/trip-api#get-trip), created or updated by sending [PUT trip](?p=/trip-api#put-trip) requests. 

Each trip is allocated an attachment storage where documents and routes attached to the trip can be uploaded to. Attachments can contain any document or file that can be viewed on the mobile devices using an external app installed on the device and associated with the mime type or file extension of the document. Examples include PDF documents (**.pdf** files), navigable routers (**.bcr** files), images (**.jpg**/**.png** files), etc.

InstantCMR distinguishes between documents containing navigable routes and general documents. Routes are shown to the driver on the mobile device under the Routes section with a button next to the route that starts the external navigation app. Presently routes are required to have the **.bcr** extension in their file name. Other attachments are considered as general documents and are shown under the Incoming documents section in the instantCMR app. Drivers can click the document to open them with an external app registered for the mime type of the document.

Attachments are managed by the [GET rut](?p=/trip-api#get-rut), [PUT rut](?p=/trip-api#put-rut) and [DELETE rut](?p=/trip-api#delete-rut) endpoints.


### Quotas and limits
The instantCMR backend enforces the following limits when storing trips in its database:

*   **Size of a trip record cannot exceed 80 KiB of data**. The size of the trip record is calculated by serializing the trip (including all of its metadata, deleted and active document requests, deleted and active stations and all documents submitted to the trip by mobile devices, excluding image data submitted with the documents and excluding content of attached documents) as json and then taking its UTF-8 representation.
*   **Size of tripxtid, docrid, stanxtid, userxtid and copid fields cannot exceed 64 bytes of data**. The size of these fields is calculated by taking their UTF-8 representation.
*   **No single driver is allowed to be assigned more than 100 active trips**. A trip is considered to be active if its **ktroc** field has a value of either **o** or **mfc**.
*   **Trips can have at most 20 attachments** in total (including all documents attached to individual stations within the trip)

Any [PUT trip](?p=/trip-api#put-trip) request in violation of the above limits will be rejected with a **400 BadRequest** response. It is strongly advised to stay well below these limits. Once these limits are exceeded the instantCMR backend will refuse accepting document submissions by mobile devices as well as new document reviews being added on the instantCMR Hub.


## GET trip

Retrieves a trip entity identified by **tripxtid**.

### Request
```http 
GET /v3/igr/trip/{copid}/{tripxtid}[?expire={download-url-expiry}][&{deleted}] HTTP/1.1  
Host: api.instantcmr.com  
x-icmr-auth-1: {authentication-token}  
```

{% table %}
---
* **copid**
* Company identifier assigned by instantCMR
---
* **tripxtid**
* Client-generated trip identifier
---
* **download-url-expiry**
* The desired duration of the validity period until attached document data can be downloaded using the [temporary blob urls](?p=/document-storage-schemas#temporary-blob-url-urlv) provided in the response. Provided in an integral number of minutes (e.g. **1** means one minute). If omitted, the validity period of temporary urls is 15 minutes.
---
* **deleted**
* Deleted document requests are by default not returned from a [GET trip](?p=/trip-api#get-trip) request, unless you specify the **?deleted** query parameter. If you do, all document requests of the trip will be returned (including the deleted ones) with the deleted document requests marked with the presence of the **ofDeleted** fields.

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

{triped}
```

### Response codes
{% table %}
---
* 200 OK
* Request was successful. Response body contains trip entity.
---
* 401 Unauthorized
* Missing or invalid **authentication-token**.
---
* 403 Forbidden
* Authenticated user does not have sufficient permission to access trips of company designated by **copid**.
---
* 404 Not Found
* Trip with id of **tripxtid** not found.
---
* 429 Too Many Requests
* Request has been throttled as user exceeded API quota.
{% /table %}

### Response headers
{% table %}
---
* **ETag**
* string
* Entity version tag. Must be supplied in subsequent [PUT trip](?p=/trip-api#put-trip) requests in the **If-Match** header.
{% /table %}

### Response body
{% table %}
---
* **triped**
* [triped](?p=/trip-schemas#trip-response-triped)
* Trip response entity.
{% /table %}




## PUT trip

Stores a trip entity, optionally replacing any previous entity stored under **tripxtid**.

Trip entities follow the guidelines of [entity versioning](?p=/entity-versioning). Send an **If-None-Match: \*** header when creating a new trip entity to ensure that no previously existing trip gets overwritten. To update a trip entity, send the request with an **If-Match** header specifying the last known **ETag** of the trip entity. 

#### Updating document requests (**rgdocr**)

When updating a document request associated with a trip, make sure that it is sent with the _same document request id_ in its **docrid** field. Note that once a document request has been submitted, updating its document type in its **kdocr** field is not supported. Doing so will yield a **400 Bad request** response. This also applies to deleted document requests.

Please be aware that document requests might also be added into a trip by reviewers using the instantCMR Hub. To ensure that you do not accidentally delete these document requests, make sure that you merge your document request changes with the contents of **rgdocr** field of the trip entity. I.e. if you want to add a new document request you can perform the addition on the version of the trip entity held in your local cache (the last known state of the trip) and send the modified trip entity in a [PUT trip](?p=/trip-api#put-trip) request. If however you receive a **412 PreconditionFailed** and then retrieve an updated version of the trip entity, you would need to perform the document request addition again on the updated trip entity before sending a new [PUT trip](?p=/trip-api#put-trip) request, otherwise you risk removing document requests added by reviewers from the trip entity.

There is an edge case around deleting document requests (in the field **rgdocr**). Although document requests omitted in a trip update will be removed from the UI on the mobile app, they will be kept around to allow any documents created by drivers (in response to these requests) to be submitted. The [GET trip](?p=/trip-api#get-trip) endpoint provides a way to retrieve these dangling submissions.

#### Updating trip status (**ktroc**)

The trip status (**ktroc**) can also be updated by external actors, which might be a change you are unaware of. The driver might change the trip status from open (**o**) to closed-by-driver (**mfc**), or the reviewer might change the trip status from closed-by-driver (**mfc**) to open (**o**) whenever s/he adds a new required document request to a trip already closed by the driver. Unless you want to explicitly override the trip status with your value, you should simply resend the trip status returned to you from the last [GET trip](?p=/trip-api#get-trip) request.


Trips that have at least one required and non-deleted document request that has not yet been fulfilled (i.e. no document has been submitted to fulfill that request) cannot be closed by the driver (**mfc**). This rule is also enforced by the instantCMR API, meaning that if for any [PUT trip](?p=/trip-api#put-trip) request that would violate this rule, the API will override any **ktroc** value of **mfc** to **o**.

This can occur if

*   You set **ktroc** to **mfc** for a trip that has required document requests that are yet unfulfilled.
*   You add a required document request to a trip that already has a **ktroc** value of **mfc**.

In both cases the [PUT trip](?p=/trip-api#put-trip) request will succeed with a **200 OK** response, however the value of **ktroc** will be set to **o** (reopening an already closed trip if necessary). The response body returned by the instantCMR API will also reflect this change.

Note, that this rule only applies to trips with a status of closed-by-driver (**mfc**). Trips can be closed by dispatcher (**c**) even if not all required document requests have been fulfilled.

### Request
```http 
PUT /v3/igr/trip/{copid}/{tripxtid}[?expire={download-url-expiry}] HTTP/1.1  
Host: api.instantcmr.com  
x-icmr-auth-1: {authentication-token}  
Content-Type: application/json  
[If-Match: {ETag}]  
[If-None-Match: "*"]  
Content-Length: 1779  

{tripeu}
```

{% table %}
---
* **copid**
* Company identifier assigned by instantCMR
---
* **tripxtid**
* Client-generated trip identifier
---
* **ETag**
* Entity version tag. Provide previous trip entity version tag when updating an already existing trip entity. When creating a new entity pass **“\*”** in the **If-None-Match** header.
---
* **download-url-expiry**
* The desired duration of the validity period until attached document data can be downloaded using the [temporary blob urls](?p=/document-storage-schemas#temporary-blob-url-urlv) provided in the response. Provided in an integral number of minutes (e.g. **1** means one minute). If omitted, the validity period of temporary urls is 15 minutes.
---
* **authentication-token**
* See [Authentication](?p=/authentication#authentication).
---
* **tripeu**
* [Trip update entity](?p=/trip-schemas#trip-update-tripeu).
{% /table %}

### Response
```http 
HTTP/1.1 200 OK  
Date: Thu, 23 Jan 2020 16:29:47 GMT  
Content-Type: application/json; charset=UTF-8  
Etag: {ETag} 
Content-Length: 1386  
Server: Jetty(9.2.1.v20140609)  

{triped}
```

### Response codes
{% table %}
---
* 200 OK
* Request was successful. Response contains updated trip entity in the response body and new trip entity version tag in the **ETag** response header.
---
* 400 BadRequest
* Malformed request or API quota violation. See also [Quotas and Limits](?p=/trip-api#quotas-and-limits).
---
* 401 Unauthorized
* Missing or invalid **authentication-token**.
---
* 403 Forbidden
* Authenticated user does not have sufficient permission to access trips of company designated by **copid**.
---
* 404 Not Found
* Trip with id of **tripxtid** not found. Only returned when updating an existing entity (i.e. **If-Match** header is present).
---
* 412 PreconditionFailed
* **ETag** specified in **If-Match** or **If-None-Match** headers did not match the current trip entity version. Retrieve the current trip entity using [GET trip](?p=/trip-api#get-trip), reapply the changes, and retry the [PUT trip](?p=/trip-api#put-trip) request with updated **ETag**.
---
* 429 Too Many Requests
* Request has been throttled as user exceeded API quota.
{% /table %}

### Response headers
{% table %}
---
* **ETag**
* string
* Entity version tag. Must be supplied in subsequent [PUT trip](?p=/trip-api#put-trip) requests in the **If-Match** header.
{% /table %}
### Response body
{% table %}
---
* **triped**
* [triped](?p=/trip-schemas#trip-response-triped)
* Trip response entity.
{% /table %}



## PUT rut
Attach a document or route to a trip and optionally to a station within the trip. Attachments are identified by their path and file name within the trip attachment storage (**file-path**). Attaching a document to a trip with the same name as a previous document attached to the same trip will overwrite the previous document.

Documents can be either attached to the entire trip or to individual stations of the trip. Documents attached to the trip will be displayed to the drivers on the Details tab of the trip whereas documents attached to a station will be displayed on the Stations tab, next to the individual station they are attached to. To attach a document to the entire trip, upload the document to the root folder of the trip attachment storage (e.g. **/route.bcr** or **/document.pdf**). To attach a document to a station , upload the document to a folder named after the **stanxtid** of the station (e.g. **/station-1/route.bcr** or **/station-1/document.pdf**).

### Request
```http 
PUT /v3/igr/trip/{copid}/{tripxtid}/rut/{file-path}[?expire={download-url-expiry}] HTTP/1.1  
Host: api.instantcmr.com  
x-icmr-auth-1: {authentication-token}  
Content-Type: application/octet-stream  
Accept: */*  
Content-Length: 11741  

{document-bytes}
```

{% table %}
---
* **copid**
* Company identifier assigned by instantCMR
---
* **tripxtid**
* Client-generated trip identifier
---
* **file-path**
* Name and path of the document file.
---
* **download-url-expiry**
* The desired duration of the validity period until attached document data can be downloaded using the [temporary blob urls](?p=/document-storage-schemas#temporary-blob-url-urlv) provided in the response. Provided in an integral number of minutes (e.g. **1** means one minute). If omitted, the validity period of temporary urls is 15 minutes.
---
* **authentication-token**
* See [Authentication](?p=/authentication#authentication).
---
* **document-bytes**
* Contents of the document.
{% /table %}

### Response
```http 
HTTP/1.1 200 OK  
Date: Thu, 23 Jan 2020 16:29:47 GMT  
Content-Type: application/json; charset=UTF-8  
Etag: {ETag} 
Content-Length: 1386  
Server: Jetty(9.2.1.v20140609)  

{triped}
```

### Response codes
{% table %}
---
* 200 OK
* Request was successful. Response contains updated trip entity in the response body and new trip entity version tag in the **ETag** response header.
---
* 400 BadRequest
* Malformed request or API quota violation. See also [Quotas and Limits](?p=/trip-api#quotas-and-limits).
---
* 401 Unauthorized
* Missing or invalid **authentication-token**.
---
* 403 Forbidden
* Authenticated user does not have sufficient permission to access trips of company designated by **copid**.
---
* 404 Not Found
* Trip with id of **tripxtid** not found.
---
* 429 Too Many Requests
* Request has been throttled as user exceeded API quota.
{% /table %}

### Response headers
{% table %}
---
* **ETag**
* string
* Entity version tag of the updated trip.
{% /table %}
### Response body
{% table %}
---
* **triped**
* [triped](?p=/trip-schemas#trip-response-triped)
* Trip response entity of the updated trip.
{% /table %}



## DELETE rut
Removes a document from the trip attachment storage.

### Request
```http 
DELETE /v3/igr/trip/{copid}/{tripxtid}/rut/{file-path}[?expire={download-url-expiry}] HTTP/1.1  
Host: api.instantcmr.com  
x-icmr-auth-1: {authentication-token}  
Content-Type: application/octet-stream  
Accept: */*  
```

{% table %}
---
* **copid**
* Company identifier assigned by instantCMR
---
* **tripxtid**
* Client-generated trip identifier
---
* **file-path**
* Name and path of the document file.
---
* **download-url-expiry**
* The desired duration of the validity period until attached document data can be downloaded using the [temporary blob urls](?p=/document-storage-schemas#temporary-blob-url-urlv) provided in the response. Provided in an integral number of minutes (e.g. **1** means one minute). If omitted, the validity period of temporary urls is 15 minutes.
---
* **authentication-token**
* See [Authentication](?p=/authentication#authentication).
---
* **document-bytes**
* Contents of the document.
{% /table %}

### Response
```http 
HTTP/1.1 200 OK  
Date: Thu, 23 Jan 2020 16:29:47 GMT  
Content-Type: application/json; charset=UTF-8  
Etag: {ETag} 
Content-Length: 1386  
Server: Jetty(9.2.1.v20140609)  

{triped}
```

### Response codes
{% table %}
---
* 200 OK
* Request was successful. Response contains updated trip entity in the response body and new trip entity version tag in the **ETag** response header.
---
* 400 BadRequest
* Malformed request or API quota violation. See also [Quotas and Limits](?p=/trip-api#quotas-and-limits).
---
* 401 Unauthorized
* Missing or invalid **authentication-token**.
---
* 403 Forbidden
* Authenticated user does not have sufficient permission to access trips of company designated by **copid**.
---
* 404 Not Found
* Trip with id of **tripxtid** not found.
---
* 429 Too Many Requests
* Request has been throttled as user exceeded API quota.
{% /table %}

### Response headers
{% table %}
---
* **ETag**
* string
* Entity version tag of the updated trip.
{% /table %}

### Response body
{% table %}
---
* **triped**
* [triped](?p=/trip-schemas#trip-response-triped)
* Trip response entity of the updated trip.
{% /table %}



## GET rut
Downloads an attached document from the trip attachment storage.

### Request
```http 
GET {document-url-path} HTTP/1.1  
Host: {document-url-host}  
x-icmr-auth-1: {authentication-token}  
```

{% table %}
---
* **document-url-path**
* The path part of the temporary document url specified in the **url** field of an [urlv](?p=/document-storage-schemas#temporary-blob-url-urlv) entity.
---
* **document-url-host**
* The host part of the temporary document url specified in the **url** field of an [urlv](?p=/document-storage-schemas#temporary-blob-url-urlv) entity.
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

{document-bytes}  
```

### Response codes
{% table %}
---
* 200 OK
* Request was successful. Response body contains the raw document bytes.
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
