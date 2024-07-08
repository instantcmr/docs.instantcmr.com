

## Document storage configuration (Dboxc)

Configuration of a user’s document storage.

```json {% name="Dboxc" %}
{
    "oshrn": "Bertram",
    "rguserxtidFollow": [
        "49492294481526", 
        …
        "4949222382123"
    ]
}
```

### Fields
{% table %}
---
* **rguserxtidFollow**
* list of string
* List of user identifiers of users whose document storages this user is subscribed to.
---
* **oshrn**
* string (optional)
* Name of this user’s document storage as it appears to other users who subscribe to it.
{% /table %}



## Document storage contents (Dboxed)

Contents of a user’s document storage, including the list of documents and folders uploaded, the list of subscriptions to document storages of other user accounts (configured via the **[dboxc](?p=/document-storage-schemas#document-storage-configuration-dboxc)** entity using the User API) and a list of delivery receipts for each document successfully delivered to the given user, including those delivered from a shared document storage. Does not include documents and folders uploaded into document storages shared with this user.

```json {% name="Dboxed" %}
{
    "userxtid": "494922944810349",
    "rgit": [
        {
            "k": "f",
            "filid": "ixU1CeqNHTihdvM73CtNHQ",
            "pat": "/Bathsystem Spa/Entry Procedure.pdf",
            "dtuUpload": "2023-01-03T17:49:36.012Z",
            "cb": 2891965,
            "urlv": {
                "url": "https://document-url",
                "dtuValid": "2023-01-06T19:00:34.587Z"
            }
        },
        {
            "k": "d",
            "filid": "obHqpD16ccZq1jcUYaVIYA",
            "pat": "/Bathsystem Spa/",
            "dtuUpload": "2023-01-03T18:51:54.362Z"
        },  
        …
    ],
    "rgsyc": [
        {
            "filid": "ixU1CeqNHTihdvM73CtNHQ",
            "dtuDelivered": "2023-01-03T17:51:38.445Z"
        },  
        …
    ],
    "rgsub": [
        {
            "userxtid": "45222265420334"
        },  
        …
    ]
}
```

### Fields
{% table %}
---
* **userxtid**
* string
* The client-generated identifier of the user account owning this document storage.
---
* **rgit**
* list of [dboxit](?p=/document-storage-schemas#document-storage-item-dboxit)
* List of documents and folders located in the document storage.
---
* **rgsyc**
* list of [dboxsyc](?p=/document-storage-schemas#document-storage-item-delivery-receipt-dboxsyc)
* List of document storage item delivery receipts.
---
* **rgsub**
* list of [dboxsub](?p=/document-storage-schemas#document-storage-subscription-dboxsub)
* List of subscriptions to other user accounts’ document storages.
{% /table %}


## Document storage item (Dboxit)

A document or folder uploaded into a user’s document storage.

```json {% name="Dboxit" %}
{
    "k": "f",
    "filid": "ixU1CeqNHTihdvM73CtNHQ",
    "pat": "/Bathsystem Spa/Entry Procedure.pdf",
    "dtuUpload": "2023-01-03T17:49:36.012Z",
    "cb": 2891965,
    "urlv": {
        "url": "https://document-url",
        "dtuValid": "2023-01-06T19:00:34.587Z"
    }
}  

{
    "k": "d",
    "filid": "obHqpD16ccZq1jcUYaVIYA",
    "pat": "/Bathsystem Spa/",
    "dtuUpload": "2023-01-03T18:51:54.362Z"
}
```


### Fields
{% table %}
---
* **k**
* string
* Designates the type of the document storage item.
  {% list %} 
  * **f**: Document/file
  * **d**: Folder
  {% /list %}
---
* **filid**
* string
* The server-generated unique identifier for the document or folder
---
* **pat**
* string
* The full path to the document or folder. Folder paths always end with a trailing / character.
---
* **dtuUpload**
* datetime
* The date/time when the document was uploaded to or when the folder was created in the user’s document storage.
---
* **cb**
* number
* The size of the document in bytes. Only applicable for document/file items.
---
* **urlv**
* [urlv](?p=/document-storage-schemas#temporary-blob-url-urlv)
* Temporary blob url used to download the document. Only applicable for document/file items.
{% /table %}




## Document storage item delivery receipt (Dboxsyc)

A delivery receipt describing the **filid** of a document/file delivered to a user and the date/time of the delivery. Might also represent a delivery receipt of a document shared from another document storage.

```json {% name="Dboxsyc" %}
{
    "filid": "ixU1CeqNHTihdvM73CtNHQ",
    "dtuDelivered": "2023-01-03T17:51:38.445Z"
}
```

### Fields
{% table %}
---
* **filid**
* string
* The server-generated unique identifier of the document/file this delivery receipt corresponds to.
---
* **dtuDelivered**
* datetime
* The date and time when the document was delivered to the user’s mobile device.
{% /table %}



## Document storage subscription (Dboxsub)

A subscription to a shared document storage.

```json {% name="Dboxsub" %}
{
    "userxtid": "45222265420334"
}
```

### Fields
{% table %}
---
* **userxtid**
* string
* The client-generated identifier of the user account this document storage is subscribed to.
{% /table %}





## Document move target (Filta)
Specifies the target of a move operation of a document or folder in a user’s document storage.

```json {% name="Dboxsub" %}
{  
    "userxtid": "45222265420334",
    "pat": "/Bathsystem Spa/Entry Procedure.pdf"
}  
```

### Fields
{% table %}
---
* **userxtid**
* string
* The client-generated identifier of the target user account the document storage item should be moved to.
---
* **pat**
* string
* The target path where the document or folder should be moved to within the document storage of the target user.
When moving folders make sure to terminate the path with a **/** character. In contrast when moving documents make sure the path is not terminated with a **/** character. The path must start with a **/** character.
{% /table %}




## Temporary blob url (Urlv)

Represents a temporary url of a blob stored on the instantCMR backend.

```json {% name="Ruts" %}
{  
    "url": "https://attachment-url",  
    "dtuValid": "2021-11-17T19:37:57.573Z"  
}  
```

### Fields
{% table %}
---
* **url**
* string
* Temporary url. Can be used to download the blob data before the end of the validity period defined by **dtuValid**.
---
* **dtuValid**
* datetime (utc)
* Date and time (UTC) until the url provided in the **url** field can be used to download blob data. By default, download urls are valid for 15 minutes from the time the HTTP response has been generated by API. To extend or reduce the validity period, pass the desired duration (in minutes) in the **download-url-expiry** parameter.
{% /table %}

