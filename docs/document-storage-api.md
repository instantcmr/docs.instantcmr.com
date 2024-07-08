# Document Storage API

The Document Storage API lets integration clients send external documents and other files to the mobile devices of drivers.

Each user account in the instantCMR application is automatically assigned a document storage that can be used to send documents to the user’s mobile device. Documents and other files can be sent to users by uploading them into the user’s document storage. Documents can be further organized into folders.

Documents uploaded to the document storage of a user can be shared to other users by creating subscriptions to a document storage via the [PUT user request](?p=/user-api#put-user). Uploading a document or creating a folder in a shared document storage will send the given document/folder to each users’ mobile device who is subscribed to it.

Documents and folders are identified by **filid**, a server generated unique identifier assigned to the item at creation. Documents and folders are also identified by their path (**pat**) within the document storage they are created in. Uploading a different document under the same path will replace the previous copy of the document with the new one, assigning a new **filid** to the document in the process.

When uploading a document, all folders on the document path will be automatically created. Similarly, when deleting a folder, all folders and documents stored directly or indirectly within the folder will be automatically deleted. Deleting a folder is a non-atomic operation. If documents are uploaded to a folder at the same time the folder is being deleted there are no guarantees about the exact outcome. It is equally possible that newly uploaded documents will get deleted along with the folder or that the folder being deleted will get recreated while uploading a new document.

When a document or folder is delivered to a user’s mobile device a delivery receipt is created in the user’s document storage, specifying the date of delivery and the **filid** identifying the item delivered.


## Quotas and limits

A single user’s document storage may store at most 512 documents or folders in total, not counting documents and folders received via a subscription from another user’s document storage.

Documents uploaded to the document storage may not exceed 5MB in size.

The path of any file or folder stored in the document storage may not exceed 2048 characters, including the leading (and in case of folders, trailing) forward slash (**/**) character.

The metadata describing a single user’s document storage contents may not exceed 128KB in size when counted with the following rules

```javascript
metadata_size = 
  58 /* bytes */
  + userid_size  
  + /* sum of file_descriptor_size for each document or folder in the document storage (excluding shared items) */
  + /* sum of delivery_receipt_size for each document or folder in the document storage (including shared items) */

file_descriptor_size = 
  103 /* bytes */
  + userxtid_size  
  + /* size of file name represented in UTF-8 encoding */

delivery_receipt_size = 49 /* bytes */

userid_size = 1 /* byte */ + userxtid_size + copid_size

userxtid_size = /* size of userxtid represented in UTF-8 encoding */

copid_size = /* size of copid represented in UTF-8 encoding */
```



## GET document storage contents

Enumerates the contents of a document storage assigned to a user identified by **userxtid** .

### Request
```http 
GET /v3/igr/dbox/{copid}/{userxtid}/fil[?expire={download-url-expiry}] HTTP/1.1  
Host: api.instantcmr.com  
x-icmr-auth-1: {authentication-token}  
```

{% table %}
---
* **copid**
* Company identifier assigned by instantCMR.
---
* **userxtid**
* Client-generated identifier of the user whose document storage should be enumerated.
---
* **download-url-expiry**
* The desired duration of the validity period until document data can be downloaded using the [temporary blob urls](?p=/document-storage-schemas#temporary-blob-url-urlv) provided in the response. Provided in an integral number of minutes (e.g. **1** means one minute). 

  If omitted, the validity period of temporary urls is 15 minutes.
---
* **authentication-token**
* See [Authentication](?p=/authentication#authentication).
{% /table %}

### Response
```http 
HTTP/1.1 200 OK  
Date: Thu, 23 Jan 2020 16:29:47 GMT  
Content-Type: application/json; charset=UTF-8  
Content-Length: 1386  
Server: Jetty(9.2.1.v20140609)  

{dboxed}
```

### Response codes
{% table %}
---
* 200 OK
* Request was successful. The response contains the document storage content entity in the response body.
---
* 400 BadRequest
* Malformed request.
---
* 401 Unauthorized
* Missing or invalid **authentication-token**.
---
* 403 Forbidden
* The authenticated user does not have sufficient permission to access document storage of users of the company designated by **copid**.
---
* 404 Not Found
* The user specified by the **userxtid** does not exist.
---
* 429 Too Many Requests
* Request has been throttled as the user exceeded API quota or request validation is temporarily impossible. Retry the request with an exponential backoff.
{% /table %}

### Response body
{% table %}
---
* **dboxed**
* [dboxed](?p=/document-storage-schemas#document-storage-contents-dboxed)
* Document storage content entity describing the documents, folders, delivery receipts and subscriptions of the user’s document storage.
{% /table %}



## PUT document

Upload a document to a user’s document storage

### Request

Documents are identified by their path and file name within the document storage (**file-path**). Document and folder names within the path may not start or end with a whitespace character and must be concatenated with forward slash (**/**) characters. The **file-path** must not end with a forward slash (**/**) character.

As a result of this request, all folders on the specified **file-path** will be automatically created if they do not exist.

Uploading a document with the same **file-path** as an already existing document will replace the previous document with the uploaded one.

Documents uploaded to a user’s document storage will be delivered to the user’s mobile device as soon as network conditions permit. Drivers can click the document to open it with an external app registered for the mime type of the document.

When the document has been successfully delivered to the user, a [document storage item delivery receipt](?p=/document-storage-schemas#document-storage-item-delivery-receipt-dboxsyc) is created in the user’s document storage.

```http 
PUT /v3/igr/dbox/{copid}/{userxtid}/fil/{file-path} HTTP/1.1  
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
* **userxtid**
* Client-generated user identifier specifying the owner of the document storage area this document should be uploaded to.
---
* **file-path**
* Name and path of the document file.
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
Server: Jetty(9.2.1.v20140609)  
```

### Response codes
{% table %}
---
* 200 OK
* Request was successful.
---
* 400 BadRequest
* Malformed request or API quota violation. See also [Quotas and Limits](?p=/document-storage-api#quotas-and-limits).
---
* 401 Unauthorized
* Missing or invalid **authentication-token**.
---
* 403 Forbidden
* The authenticated user does not have sufficient permission to access document storage of the company designated by **copid**.
---
* 404 Not Found
* User account with id of **userxtid** not found.
---
* 429 Too Many Requests
* Request has been throttled as user exceeded API quota.
{% /table %}



## PUT folder

Create a folder in a user’s document storage area.

### Request

Folders are identified by their path and name within the document storage (**folder-path**). Folder names within the path may not start or end with a whitespace character and must be concatenated with forward slash (**/**) characters. The **folder-path** must end with a forward slash (**/**) character.

As a result of this request, all folders on the specified **folder-path** will be automatically created if they do not exist.

Creating a folder with the same **folder-path** as an already existing folder will result in a no-operation and return a **200 OK** response..

Folders created in a user’s document storage will be delivered to the user’s mobile device as soon as network conditions permit. When the folder has been successfully delivered to the user, a [document storage item delivery receipt](?p=/document-storage-schemas#document-storage-item-delivery-receipt-dboxsyc) is created in the user’s document storage.

```http 
PUT /v3/igr/dbox/{copid}/{userxtid}/fil/{folder-path}/ HTTP/1.1  
Host: api.instantcmr.com  
x-icmr-auth-1: {authentication-token}  
Accept: */*  
```

{% table %}
---
* **copid**
* Company identifier assigned by instantCMR
---
* **userxtid**
* Client-generated user identifier specifying the owner of the document storage area where this folder should be created in.
---
* **folder-path**
* Name and path of the folder.
---
* **authentication-token**
* See [Authentication](?p=/authentication#authentication).
{% /table %}

### Response
```http 
HTTP/1.1 200 OK  
Date: Thu, 23 Jan 2020 16:29:47 GMT  
Content-Type: application/json; charset=UTF-8  
Server: Jetty(9.2.1.v20140609)  
```

### Response codes
{% table %}
---
* 200 OK
* Request was successful.
---
* 400 BadRequest
* Malformed request or API quota violation. See also [Quotas and Limits](?p=/document-storage-api#quotas-and-limits).
---
* 401 Unauthorized
* Missing or invalid **authentication-token**.
---
* 403 Forbidden
* The authenticated user does not have sufficient permission to access document storage of the company designated by **copid**.
---
* 404 Not Found
* User account with id of **userxtid** not found.
---
* 429 Too Many Requests
* Request has been throttled as user exceeded API quota.
{% /table %}


## DELETE document or folder

Removes a document or folder from a user’s document storage.

### Request

Removes a document or folder specified by **folder-or-file-path** from the document storage of the user specified by **userxtid**.

When deleting a folder, **folder-or-file-path** must end with a trailing forward slash (**/**) character. All documents and folders contained within the target folder will be automatically removed from the document storage.

When deleting a document, **folder-or-file-path** must not end with a trailing forward slash (**/**) character.

As a result of this request the document or folder (including its entire contents) will be removed from the user’s mobile device. When this succeeds all [document storage item delivery receipts](?p=/document-storage-schemas#document-storage-item-delivery-receipt-dboxsyc) corresponding to documents and folders removed from the user’s mobile device will be also deleted from the user’s document storage. Please note however that until this happens, the document storage might contain [document storage item delivery receipts](?p=/document-storage-schemas#document-storage-item-delivery-receipt-dboxsyc) referring to documents and/or folders that are not anymore stored in the document storage.

```http 
DELETE /v3/igr/dbox/{copid}/{userxtid}/fil/{folder-or-file-path} HTTP/1.1  
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
* **userxtid**
* Client-generated user identifier specifying the owner of the document storage area where this document or folder should be deleted from.
---
* **folder-or-file-path**
* Name and path of the document or folder to be deleted. Document paths must, while file paths must not end with a forward slash (**/**) character.
---
* **authentication-token**
* See [Authentication](?p=/authentication#authentication).
{% /table %}

### Response
```http 
HTTP/1.1 200 OK  
Date: Thu, 23 Jan 2020 16:29:47 GMT  
Content-Type: application/json; charset=UTF-8  
Server: Jetty(9.2.1.v20140609)  
```

### Response codes
{% table %}
---
* 200 OK
* Request was successful.
---
* 400 BadRequest
* Malformed request or API quota violation. See also [Quotas and Limits](?p=/document-storage-api#quotas-and-limits).
---
* 401 Unauthorized
* Missing or invalid **authentication-token**.
---
* 403 Forbidden
* The authenticated user does not have sufficient permission to access document storage of the company designated by **copid**.
---
* 404 Not Found
* User account with id of **userxtid** not found or the document/folder specified by **folder-or-file-path** does not exist.
---
* 429 Too Many Requests
* Request has been throttled as user exceeded API quota.
{% /table %}

## MOVE document
Moves or renames a document or folder within a user’s document storage.

### Request
Moves or renames a document or folder specified by **source-folder-or-file-path** within the document storage of the user specified by **source-userxtid**.

The new location of the document or folder should be specified in the request body using a [Document move target](?p=/document-storage-schemas#document-move-target-filta) entity, specifying the **target-userxtid** and **target-folder-or-file-path**. The current version of the Document Storage API only supports moving documents or folders within a single document storage, hence **source-userxtid** and **target-userxtid** must match.

To rename a document or folder, specify the same path using a different name in **source-folder-or-file-path** and **target-folder-or-file-path**.

When moving a folder, both **source-folder-or-file-path** and **target-folder-or-file-path** must end with a trailing forward slash (**/**) character. All documents and folders contained within the target folder will be automatically moved along with the source folder within the document storage.

When moving a document, both **source-folder-or-file-path** and **target-folder-or-file-path** must not end with a trailing forward slash (**/**) character.

As a result of this request the source document or folder (including its entire contents) will be moved on the user’s mobile device without the device re-downloading the document or folder contents.

```http 
PUT /v3/igr/dbox/{copid}/{source-userxtid}/mv/{source-folder-or-file-path} HTTP/1.1  
Host: api.instantcmr.com  
x-icmr-auth-1: {authentication-token}  
Content-Type: application/octet-stream  
Accept: */*  

{filta}
```

{% table %}
---
* **copid**
* Company identifier assigned by instantCMR
---
* **source-userxtid**
* Client-generated user identifier specifying the owner of the document storage area where this document or folder should be deleted from.
---
* **folder-or-file-path**
* Name and path of the document or folder to be deleted. Folder paths must, while document paths must not end with a forward slash (**/**) character.
---
* **authentication-token**
* See [Authentication](?p=/authentication#authentication).
---
* **filta**
* [Document move target](?p=/document-storage-schemas#document-move-target-filta).
{% /table %}

### Response
```http 
HTTP/1.1 200 OK  
Date: Thu, 23 Jan 2020 16:29:47 GMT  
Content-Type: application/json; charset=UTF-8  
Server: Jetty(9.2.1.v20140609)  
```

### Response codes
{% table %}
---
* 200 OK
* Request was successful.
---
* 400 BadRequest
* Malformed request or API quota violation. See also [Quotas and Limits](?p=/document-storage-api#quotas-and-limits).
---
* 401 Unauthorized
* Missing or invalid **authentication-token**.
---
* 403 Forbidden
* The authenticated user does not have sufficient permission to access document storage of the company designated by **copid**.
---
* 404 Not Found
* Either the user account with id of **source-userxtid** or **target-userxtid** was not found or the document/folder specified by **source-folder-or-file-path** does not exist.
---
* 429 Too Many Requests
* Request has been throttled as user exceeded API quota.
{% /table %}




## GET document

Downloads a document from a user’s document storage.

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
