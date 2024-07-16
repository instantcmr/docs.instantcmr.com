# Transaction API Schemas

## Unprocessed update (Dubm)

Represents an update to either a trip, dosu, user or chat record.

```json {% name="Dubm" %}
{
    "dubmid": "ZWY2MTY0MTEtYjdjMi00YjE1LThiZmUtODQ5ZDc0OThlMDAw",
    "rhnd": "QVFFQk…2OTNJPQ",
    "kdubm": "dosu",  
    "odubdosu": {
        "obefore": dubdosu,
        "oafter": dubdosu
    }  
}

{
    "dubmid": "ZWY2MTY0MTEtYjdjMi00YjE1LThiZmUtODQ5ZDc0OThlMDAw",
    "rhnd": "QVFFQk…2OTNJPQ",
    "kdubm": "trip",   
    "odubtrip": {
        "obefore": dubtrip,
        "oafter": dubtrip
    }  
}

{
    "dubmid": "ZWY2MTY0MTEtYjdjMi00YjE1LThiZmUtODQ5ZDc0OThlMDAw",
    "rhnd": "QVFFQk…2OTNJPQ",
    "kdubm": "user",   
    "odubuser": {
        "obefore": dubuser,
        "oafter": dubuser
    }  
}

{
    "dubmid": "ZWY2MTY0MTEtYjdjMi00YjE1LThiZmUtODQ5ZDc0OThlMDAw",
    "rhnd": "QVFFQk…2OTNJPQ",
    "kdubm": "chat",   
    "odubroom": {
        "obefore": dubroom,
        "oafter": dubroom
    }
} 
```

### Fields
{% table %}
---
* **kdubm**
* string
* Type of updated entity. Also determines which of the **odubdosu** and **odubtrip** fields contain the entity update. update type.
    {% list %}
    * **dosu**: this record represents a document entity update. The update is described in the **odubdosu** field.
    * **trip**: this record represents a trip entity update. The update is described in the **odubtrip** field.
    * **user**: this record represents a user entity update. The update is described in the **odubuser** field.
    * **chat**: this record repsensents a chatroom entity update. The update is described in the **odubroom** field.
    {% /list %}
---
* **dubmid**
* string
* Entity update identifier. Can be used to prevent duplicate entity update processing.
---
* **rhnd**
* string
* Entity update removal handle. Should be used to mark this entity update as processed in a subsequent [DELETE update](?p=/transaction-api#delete-update) request.
---
* **odubdosu**
* object (optional)
* Contains the state of the document entity before and after the update. Only present for document entity updates, ie. when **kdub** has the value of **dosu**.
---
* **odubtrip**
* object (optional)
* Contains the state of the trip entity before and after the update. Only present for trip entity updates, ie. when **kdub** has the value of **trip**.
---
* **odubuser**
* object (optional)
* Contains the state of the user entity before and after the update. Only present for user entity updates, ie. when **kdub** has the value of **user**.
---
* **odubroom**
* object (optional)
* Contains the state of the chatroom entity before and after the update. Only present for chatroom entity updates, ie. when **kdub** has the value of **chat**.
---
* **obefore**
* [dubdosu](?p=/transaction-schemas#document-dubdosu) [dubtrip](?p=/transaction-schemas#trip-dubtrip) [dubuser](?p=/transaction-schemas#user-dubuser) [dubroom](?p=/transaction-schemas#chat-room-dubroom) (optional)
* The state of the document, trip, user or chatroom entity before the update. Missing if the entity update denotes an entity creation.
---
* **oafter**
* [dubdosu](?p=/transaction-schemas#document-dubdosu) [dubtrip](?p=/transaction-schemas#trip-dubtrip) [dubuser](?p=/transaction-schemas#user-dubuser)  
[dubroom](?p=/transaction-schemas#chat-room-dubroom) (optional)
* The state of the document, trip, user or chatroom entity after the update. Missing if the entity update denotes an entity removal.
{% /table %}


## Document (Dubdosu)

Represents a document submitted by a driver

```json {% name="Dubdosu" %}
{
    "dosuxtid": "494922944810349:kJxP8Xs4NtuDPr32rUZLIA",
    "userxtid": "494922944810349",
    "copid": "LogisticsGmbH",
    "ouid": "EUT",
    "kdocr": "dlvryn",
    "odocr": {
        "docrid": "iRio2dLXUTYxx4HqHuFO3g",
        "tripxtid": "KD29SNS58"
    },
    "dtuSubmit": "2020-02-28T10:27:46.077Z",
    "dtuUpload": "2020-02-28T10:27:53.064Z",
    "loc": "45.4526356+10.3780368",
    "dosumeta": {
        "ostOrderid": "1247ABV/3",
        "ostPlate": "OB462PY",
        "ostNotes": "Az átvevő megjegyzéseket írt a CMRre."
    },
    "rgimg": [
        {
            "imgid": "t-KD29SNS58-kJxP8Xs4NtuDP…zc253LsK2PezI-lMFg",
            "urlv": {
                "url": "https://image-url",
                "dtuValid": "2020-02-28T10:43:25.699Z"
            },
            "oredoim": {
                "userxtid": "642642278953221",
                "dtu": "2020-03-01T10:26:46.077Z",
                "kredosu": "accept"
            },
            "odoed": {
                "doedid": "d1a4afef-1006-4144-b252-8f20a8f090e6",
                "userxtid": "642642278953221",
                "dtu": "2020-03-01T10:26:46.077Z"
            }
        },  
        …
    ]
}
```

### Fields
{% table %}
---
* **dosuxtid**
* string
* Identifier of the document entity being updated.
---
* **userxtid**
* string
* User identifier of the driver submitting this document.
---
* **copid**
* string
* Company identifier assigned by instantCMR.
---
* **ouid**
* string
* Organization unit identifier.
---
* **kdocr**
* [kdocr](?p=/trip-schemas#document-types-kdocr)
* Type of document.
---
* **odocr**
* [docrref](?p=/transaction-schemas#document-request-reference-docrref) (optional)
* Reference to the trip and its document request that this document fulfills. Missing, if the document has not been submitted as a response to a document request.
---
* **dtuSubmit**
* datetime (utc)
* Date and time when this document has been submitted by the driver.
---
* **dtuUpload**
* datetime (utc)
* Date and time when this document has been accepted by the instantCMR backend.
---
* **loc**
* string
* Location of the device at the time the document was created. Either **lat+lon**, **nosignal** or **disabled**.  
A value of **nosignal** means no recent location was available to the device during the document creation.  
A value of **disabled** means the driver has deliberately disabled instantCMR App to collect location information (e.g. by rejecting or revoking permission on mobile device).
---
* **dosumeta**
* [dosumeta](?p=/transaction-schemas#document-description-dosumeta)
* Contains the value of fields attached by the driver to this document.
---
* **rgimg**
* list of [dubdosuimg](?p=/transaction-schemas#document-image-dubdosuimg)
* List of images attached to the document.
{% /table %}



### Document request reference (Docrref)

Represents a reference to a document request on a trip.

```json {% name="Dubdosu" %}
{  
    "docrid": "iRio2dLXUTYxx4HqHuFO3g",  
    "tripxtid": "KD29SNS58"  
}  
```

#### Fields
{% table %}

---
* **docrid**
* string
* Identifier of the document request.
---
* **tripxtid**
* string
* Client-generated identifier of the trip.
{% /table %}



### Document description (Dosumeta)

Text fields entered by the driver when submitting a [document](?p=/transaction-schemas#document-dubdosu).

```json {% name="Dosumeta" %}
{  
    "ostOrderid": "1247ABV/3",  
    "ostPlate": "OB462PY",  
    "ostNotes": "Az átvevő megjegyzéseket írt a CMRre."  
}  
```

#### Fields
{% table %}
---
* **ostOrderid**
* string (optional)
* Contains the value of the Order Id field. Omitted if the field was left empty by the driver. Also omitted for documents attached to a trip.
---
* **ostPlate**
* string (optional)
* Contains the value of the License Plate field. Omitted if the field was left empty by the driver. Also omitted for documents attached to a trip.
---
* **ostNotes**
* string (optional)
* Contains the value of the Notes field. Omitted if the field was left empty by the driver.
{% /table %}



### Document image (Dubdosuimg)

Represents a document page scanned by the driver

```json {% name="Dubdosuimg" %}
{
    "imgid": "t-KD29SNS58-kJxP8Xs4NtuDP…zc253LsK2PezI-lMFg",
    "urlv": {
        "url": "https://image-url",
        "dtuValid": "2020-02-28T10:43:25.699Z"
    },
    "oredoim": {
        "userxtid": "642642278953221",
        "dtu": "2020-03-01T10:26:46.077Z",
        "kredosu": "accept"
    },
    "odoed": {
        "doedid": "d1a4afef-1006-4144-b252-8f20a8f090e6",
        "userxtid": "642642278953221",
        "dtu": "2020-03-01T10:26:46.077Z"
    }
}
```

#### Fields
{% table %}
---
* **imgid**
* string
* Image identifier.
---
* **urlv**
* [urlv](?p=/document-storage-schemas#temporary-blob-url-urlv)
* Image url descriptor.
---
* **url**
* string
* Temporary image url. Can be used to download the image data before the end of the validity period defined by **dtuValid**.
---
* **dtuValid**
* datetime (utc)
* Date and time (UTC) until the url provided in the **url** field can be used to download image data. By default, download urls are valid for 15 minutes from the time the HTTP response has been generated by API. To extend or reduce the validity period, pass the desired duration in the **download-url-expiry** parameter.
---
* **oredoim**
* [dubredoim](?p=/transaction-schemas#document-image-review-dubredoim)  
(optional)
* Contains the result of document image review. Omitted if the image has not been reviewed yet.
---
* **odoed**
* [dubdoed](?p=/transaction-schemas#document-image-edit-dubdoed) (optional)
* Contains the last edit performed on this document image. Omitted if the image has not been edited, or if the last edit has been undone.
{% /table %}




### Document image review (Dubredoim)

Represents the result of a document image review performed on the instantCMR Hub.

```json {% name="Dubredoim" %}
{  
    "userxtid": "642642278953221",  
    "dtu": "2020-03-01T10:26:46.077Z",  
    "kredosu": "accept"  
}  
```

#### Fields
{% table %}
---
* **userxtid**
* string
* User identifier of the reviewer who reviewed the image.
---
* **dtu**
* datetime (utc)
* Date and time of the review.
---
* **kredosu**
* string
* Result of review. Either **accept** or **reject**.
{% /table %}



### Document image edit (Dubdoed)

Represents the result of an image edit performed on the instantCMR Hub.

```json {% name="Dubdoed" %}
{  
    "doedid": "d1a4afef-1006-4144-b252-8f20a8f090e6",  
    "userxtid": "642642278953221",  
    "dtu": "2020-03-01T10:26:46.077Z"  
}  
```

#### Fields
{% table %}
---
* **userxtid**
* string
* User identifier of the reviewer who edited the image.
---
* **dtu**
* datetime (utc)
* Date and time of the review.
---
* **doedid**
* string
* Identifier of the image edit.
{% /table %}



## Trip (Dubtrip)

Represents a trip and all documents submitted the trip.

```json {% name="Dubtrip" %}
{
    "tripxtid": "KD29SNS58",
    "etag": "2",
    "copid": "LogisticsGmbH",
    "ouid": "EUT",
    "ktroc": "o",
    "ostSortBy": "0000000",
    "dtuCreate": "2020-02-28T09:49:40.706Z",
    "tripmeta": {
        "ostTripCode": "200026571",
        "ostOrderId": "AB2398VBA4",
        "ostAddress": "IT 25011 Calcinato, VIA CAVOUR 149",
        "ostCustomer": "BATHSYSTEM SPA",
        "odtuCreate": "2020-02-27T10:20:23.839Z",
        "ostInstructions": "6 spaniferrel",
        "ostNotes": "hívj megérkezéskor",
        "ostHaulerPlate": "FM682RK",
        "ostTrailerPlate": "OB462PY"
    },
    "ouserxtidDriver": "494922944810349",
    "rgdocr": [
        {
            "docrid": "qOMjwWEGbysBn8axLPlJ2A",
            "dtuRequest": "2020-02-27T10:20:23.839Z",
            "kdocr": "cmr",
            "ostShortName": "CMR (Calcinato)",
            "ostDesc": "minden oldalt",
            "fRequired": true
        },  
        …
    ],
    "rgstan": [
        {
            "stanxtid": "geqcQAOxUbkeS-N8jEtJOQ",
            "kstan": "ld",
            "odtu": "2019-12-02T06:30:00.000Z",
            "ostAddress": "IT 25011 Calcinato, VIA CAVOUR 149",
            "ostNotes": "kapukod: A678*",
            "okmDistance": 825.0,
            "ostSortBy": "0000000",
            "cufis": {
                "rgcufi": [
                    {
                        "id": "parking_lot",
                        "n": "Parking Lot",
                        "v": "Lot No. 2",
                    },  
                …
                ]
            }
        },  
        …
    ],
    "rgdosu": [
        {
            "dosuxtid": "494922944810349:kJxP8Xs4NtuDPr32rUZLIA",
            "userxtid": "494922944810349",
            "copid": "LogisticsGmbH",
            "ouid": "EUT",
            "kdocr": "dlvryn",
            "odocr": {
                "docrid": "iRio2dLXUTYxx4HqHuFO3g",
                "tripxtid": "KD29SNS58"
            },
            "dtuSubmit": "2020-02-28T10:27:46.077Z",
            "dtuUpload": "2020-02-28T10:27:53.064Z",
            "loc": "45.4526356+10.3780368",
            "dosumeta": {
                "ostNotes": "Az átvevő megjegyzéseket írt a CMRre."
            },
            "rgimg": [
                {
                    "imgid": "t-KD29SNS58-kJxP8Xs4NtuDP…zc253LsK2PezI-lMFg",
                    "urlv": {
                        "url": "https://image-url-2",
                        "dtuValid": "2020-02-28T10:43:25.699Z"
                    },
                    "oredoim": {
                        "userxtid": "642642278953221",
                        "dtu": "2020-03-01T10:26:46.077Z",
                        "kredosu": "accept"
                    },
                    "odoed": {
                        "doedid": "d1a4afef-1006-4144-b252-8f20a8f090e6",
                        "userxtid": "642642278953221",
                        "dtu": "2020-03-01T10:26:46.077Z"
                    }
                },  
            …
            ]
        }  
        …
    ],
    "cufis": {
        "rgcufi": [
            {
                "id": "reference_number",
                "n": "Reference Number",
                "v": "202203-DE-19373",
            },  
            …
        ],
    },
    "ruts": {
        "rgrut": [
            {
                "rutid": "sKtoHr7OS6GdxBybOt1Lrw",
                "fpat": "station-2/route.bcr",
                "cb": 11741,
                "dtuUpload": "2021-11-16T19:33:34.368Z",
                "urlv": {
                    "url": "https://attachment-url",
                    "dtuValid": "2021-11-17T19:37:57.573Z"
                }
            }  
            …
        ]
    }
}
```

### Fields
{% table %}
---
* **tripxtid**
* string
* Client-generated identifier of the trip entity being updated.
---
* **etag**
* string
* Entity version tag.
---
* **copid**
* string
* Company identifier.
---
* **ouid**
* string
* Organization unit identifier.
---
* **ktroc**
* [ktroc](?p=/trip-schemas#trip-open-close-status-ktroc)
* Trip open/close status.
---
* **ostSortBy**
* string (optional)
* Defines the sort order of trips as shown to the driver.

  Trips will be shown in lexicographic order sorted by their first field that is provided from the following list: **ostSortBy**, **tripmeta.odtuArrival**, start of **tripmeta.odtuiUnloading**, **tripmeta.odtuCreate**, **dtuCreate**
---
* **dtuCreate**
* datetime (utc)
* Date and time when this trip was created.
---
* **odtuMfc**
* datetime (utc) (optional)
* Date and time when this trip was closed by driver. (only present if **ktroc** is **mfc**).
---
* **tripmeta**
* [tripmeta](?p=/trip-schemas#trip-description-tripmeta)
* Fields describing the trip to the driver.
---
* **ouserxtidDriver**
* string (optional)
* Client-generated user id of the driver this trip is assigned to. (only present if the trip is assigned to a driver).
---
* **rgdocr**
* list of [docr](?p=/trip-schemas#document-request-docr)
* List of document requests.
---
* **rgdosu**
* list of [dubdosu](?p=/transaction-schemas#document-dubdosu)
* List of documents submitted to this trip.
---
* **rgstan**
* list of [stan](?p=/trip-schemas#station-stan)
* List of stations.
---
* **cufis**
* [cufis](?p=/trip-schemas#custom-fields-cufis)
* Custom fields of the trip or station.
---
* **ruts**
* [dubruts](?p=/transaction-schemas#trip-attachment-storage-dubruts)
* Trip attachments.
{% /table %}



### Trip attachment storage (Dubruts)

Represents the documents and routes attached to the trip or to a station of the trip.

```json {% name="Dubruts" %}
{
    "rgrut": [
        {
            "rutid": "sKtoHr7OS6GdxBybOt1Lrw",
            "fpat": "station-2/route.bcr",
            "cb": 11741,
            "dtuUpload": "2021-11-16T19:33:34.368Z",
            "urlv": {
                "url": "https://attachment-url",
                "dtuValid": "2021-11-17T19:37:57.573Z"
            }
        }  
        …
    ]
}

```

#### Fields
{% table %}
---
* **rgrut**
* list of [dubrut](?p=/transaction-schemas#trip-attachment-dubrut)
* List of attachments.
{% /table %}


### Trip attachment (Dubrut)
Represents a document or route attached to the trip or to a station of a trip.

#### Fields
{% table %}
---
* **rutid**
* string  
* Server-generated attachment id.
---
* **fpat**
* string
* Name and path of the attachment.

  Documents attached to the trip should be stored in the root folder (e.g. **/document.pdf**) whereas documents attached to a station should be stored in a folder named after the **stanxtid** of the station (e.g. **/station-2/route.bcr**, where **station-2** is the **stanxtid** of a station on this trip) Name of the document will be displayed to the driver.
---
* **cb**
* number
* Size of the document in bytes.
---
* **dtuUpload**
* datetime (utc)
* Date and time when the document was uploaded to the trip.
---
* **urlv**
* [urlv](?p=/document-storage-schemas#temporary-blob-url-urlv)
* Temporary blob url used to download the document.
{% /table %}


## User (Dubuser)

Represents an intantCMR User.

```json {% name="Dubuser" %}
{
    "userxtid": "494922944810349",
    "copid": "LogisticsGmbH",
    "ouid": "BusinessUnit1",
    "usern": "Bertram Friedrich",
    "usermeta": {
        "ostEmployeeId": "494922944810349",
        "ostVoicePhone": "+49-155-5558-878",
        "ostHaulerPlate": "FM682RK",
        "ostTrailerPlate": "OB462PY"
    },
    "dboxc": {
        "oshrn": "Bertram",
        "rguserxtidFollow": [
            "49492294481526", … ,
            "4949222382123"
        ]
    },
    "rgkrole": [
        "driver",
        "disp",
        "rev",
        "dia",
        "iep",
        "chedit",
        "chadmin"
    ],
    "rgulic": [
        {
            "kid": "oh91tDqJySK8wur2V6ZNhg",
            "ostDeviceModel": "SAMSUNG J330F",
            "ostDeviceImei": "302828644031295",
            "ostPin": "1111",
            "ostPhone": "+49-152-5552-942",
            "ostImsi": "082926209143692255",
            "ostSubscription": "Vodafone Red",
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
* Client-generated identifier of the user entity being updated.
---
* **copid**
* string
* Company identifier.
---
* **ouid**
* string
* Organization unit identifier.
---
* **usern**
* string
* Name of the user.
---
* **rgulic**
* list of [dubulic](?p=/transaction-schemas#license-assignment-entity-dubulic)
* List of licenses assigned to a user.
---
* **usermeta**
* [dubusermeta](?p=/transaction-schemas#user-description-dubusermeta)
* User information.
---
* **dbox**
* [dubdbox](?p=/transaction-schemas#document-storage-configuration-dubdbox)
* Configuration of the user’s document storage.
---
* **rgkrole**
* list of krole
* List of roles assigned to the user.
  {% list %}
  * **driver**: user is a driver. Drivers can be assigned mobile devices and trips and can submit documents.
  * **disp**: user is a dispatcher. Dispatchers can manage other users’ document storages and inboxes on the instantCMR Hub.
  * **rev**: user is a reviewer. Reviewers can manage other users’ trips on the instantCMR Hub.
  * **dia**: user is a device inventory administrator. Device inventory administrators can manage mobile devices and assign these devices to drivers on the instantCMR Hub.
  * **chedit**: user is a chat editor. Chat editors can create chat groups and manage (add/remove/mute members of) chat groups they are a member of.
  * **chadmin**: user is a chat administrator. Chat administrators can create chat groups and manage (add/remove/mute members of) any chat group of their organization unit.
  * **iep**: user has API access to instantCMR.
  {% /list %}
---
* **rgtripxtid**
* list of string
* List of trip identifiers (**tripxtid**) of trips assigned to this user. Empty list, for users that do not have the **driver** role.
{% /table %}


### License assignment entity (Dubulic)

Represents a license assigned to a user.

```json {% name="Dubuser" %}
{  
    "kid": "oh91tDqJySK8wur2V6ZNhg",  
    "ostDeviceModel": "SAMSUNG J330F",  
    "ostDeviceImei": "302828644031295",  
    "ostPin": "1111",  
    "ostPhone": "+49-152-5552-942",  
    "ostImsi": "082926209143692255",  
    "ostSubscription": "Vodafone Red",  
}  
```

#### Fields
{% table %}
---
* **kid**
* string
* Identifier of the license assigned to the user.
---
* **ostDeviceModel**
* string (optional)
* Model name of the mobile device. (only present if this license denotes a mobile device)
---
* **ostDeviceImei**
* string (optional)
* Imei number of the mobile device. (only present if this license denotes a mobile device)
---
* **ostPin**
* string (optional)
* Pin code of the mobile device. (only present if this license denotes a mobile device)
---
* **ostPhone**
* string (optional)
* Phone number of the sim card in the mobile device. (only present if this license denotes a mobile device)
---
* **ostImsi**
* string (optional)
* Imsi number of the sim card in the mobile device. (only present if this license denotes a mobile device)
---
* **ostSubscription**
* string (optional)
* Subscription package name and/or details of the sim card in the mobile device. (only present if this license denotes a mobile device)
{% /table %}



### User description (Dubusermeta)

Fields describing the user.

```json {% name="Dubusermeta" %}
{  
    "ostEmployeeId": "494922944810349",  
    "ostVoicePhone": "+49-155-5558-878",  
    "ostHaulerPlate": "FM682RK",  
    "ostTrailerPlate": "OB462PY"  
 }  
```

#### Fields
{% table %}
---
* **ostEmployeeId**
* string (optional)
* Employee identifier of the user.
---
* **ostVoicePhone**
* string (optional)
* Voice phone number of the user.
---
* **ostHaulerPlate**
* string (optional)
* License plate of the user’s hauler.
---
* **ostTrailerPlate**
* string (optional)
* License plate of the user’s trailer.
{% /table %}




### Document storage configuration (Dubdbox)

Document storage configuration of a user.

```json {% name="Dubdbox" %}
{  
     "oshrn": "Bertram",  
     "rguserxtidFollow": ["49492294481526", … , "4949222382123"]  
 }  
```

#### Fields
{% table %}
---
* **rguserxtidFollow**
* list
* List of user identifiers of users whose document storages this user is subscribed to.
---
* **oshrn**
* string (optional)
* Name of this user’s document storage as it appears to other users who subscribe to it.
{% /table %}




## Chat room (Dubroom)

Represents the state of a chat room update before or after a chat room update as a part of a [Dubm](?p=/transaction-schemas#unprocessed-update-dubm) entity.

For brevity **Dubroom** records do not contain the entire state of the chat room (as opposed to e.g. **Dubdosu**, **Dubtrip** or **Dubuser** entities) at a given time only the updated entities.

The **rgpost** field only contains new posts added as a result of a chat room state change.

The **rgboma** field only contains chat room member entities that have been affected by the chat room state change: either members added to the chat room, members removed from the chat room or members of the chat room that have been updated. Members unaffected by the chat room update are omitted from this list.

These updates are represented in a similar fashion as in the **Dubm** entity:

*   When an entity (e.g. **Dubboma**) is updated, the state of the entity before the update will be listed as a part of the **Dubroom** entity stored in **Dubm.odubroom.obefore** field (e.g. in the **Dubm.odubroom.obefore.rgboma** field). Similarly the state of the entity after the update will be listed as a part of the **Dubroom** entity stored in the **Dubm.odubroom.oafter** field (e.g. in the **Dubm.odubroom.oafter.rgboma** field). The two versions of the updated entities can be paired against each other using the natural identifier of the entity (e.g. **Dubboma.userxtid**).
*   When an entity is deleted, the state of the entity before the update will be listed as part of the **Dubroom** entity stored in **Dubm.odubroom.obefore** field and will be omitted from the **Dubroom** entity stored in the **Dubm.odubroom.oafter** field.
*   When an entity is inserted, the state of the entity after the update will be listed as part of the **Dubroom** entity stored in **Dubm.odubroom.oafter** field and will be omitted from the **Dubroom** entity stored in **Dubm.odubroom.obefore** field.
*   When an entity is unaffected by a chatroom state change, it will be omitted from both **Dubroom** entities stored in **Dubm.odubroom.obefore** and **Dubm.odubroom.oafter** fields.

In addition to the **rgboma** field, **rgbomath** field contains a list of all chat room members whose posts are listed in the **rgpost** field - members who posted new messages in this chat room update. The **rgbomath** field however only contains those members who are not already listed in the **rgboma** field.
```json {% name="Dubroom" %}
{
    "roomxtid": "room-1",
    "ouxtid": "BusinessUnit1",
    "rovered": {
        "wetagdturoom": {
            "etag": "45a2f",
            "v": "2021-11-16T19:33:34.368Z"
        },
        "wetagdtupost": {
            "etag": "3bd1",
            "v": "2021-11-16T19:33:34.368Z"
        }
    },
    "dtuCreate": "2021-11-16T19:33:34.368Z",
    "ostTitle": "Daily briefings",
    "rgboma": [
        {
            "userxtid": "john.peterssen",
            "usern": "John Peterssen",
            "usermeta": {
                "ostEmployeeId": "494922944810349",
                "ostVoicePhone": "+49-155-5558-878",
                "ostHaulerPlate": "FM682RK",
                "ostTrailerPlate": "OB462PY"
            },
            "colBg": "00d0d0d0",
            "owetagdtuSync": {
                "etag": "3bd0",
                "v": "2021-11-16T19:33:34.368Z"
            },
            "owetagdtuRead": {
                "etag": "3bd0",
                "v": "2021-11-16T19:33:34.368Z"
            },
            "fMuted": false
        },  

        …
    ],
    "rgbomath": [
        {
            "userxtid": "herman.mueller",
            "usern": "Herman Mueller",
            "colBg": "001a0000"
        },  

        …
    ],
    "rgpost": [
        {
            "postxtid": "oh91tDqJySK8wur2V6ZNhg",
            "etagpost": "3bd1",
            "userxtid": "herman.mueller",
            "usermeta": {
                "ostEmployeeId": "494922944810349",
                "ostVoicePhone": "+49-155-5558-878",
                "ostHaulerPlate": "FM682RK",
                "ostTrailerPlate": "OB462PY"
            },
            "dtu": "2021-11-16T19:33:34.368Z",
            "payp": {
                "k": "msg",
                "st": "Hello there my friends!"
            }
        },  

        …
    ]
}
```

### Fields
{% table %}
---
* **roomxtid**
* string
* The client-generated id of the chat room.
---
* **ouxtid**
* string
* Organization unit identifier.
---
* **rovered**
* [rovered](?p=/chat-schemas#chat-room-version-rovered)
* Chat room version.
---
* **dtuCreate**
* datetime (utc)
* Date/time when the chat room was created.
---
* **ostTitle**
* string (optional)
* Chat room title.
---
* **rgboma**
* list of [dubboma](?p=/transaction-schemas#chat-room-member-dubboma)
* List of chat room member entities updated in this update. When the Dubroom record represents a chat room state before an update, this list contains all updated and deleted members of the chat room. When the Dubroom record represents a chat room state after an update, this list contains all updated and inserted members of the chat rooms.
---
* **rgbomath**
* list of [dubbomath](?p=/transaction-schemas#chat-room-message-author-dubbomath)
* List of chat room members who authored at least one post in this chat room update.
---
* **rgpost**
* list of [dubpost](?p=/transaction-schemas#chat-message-dubpost)
* List of new messages sent to the chat room in this update, when the Dubroom record represents a chat room state after an update. Empty list otherwise.
{% /table %}


### Chat room member (Dubboma)

A member of a chat room.

```json {% name="Dubboma" %}
{  
    "userxtid": "john.peterssen",  
    "usern": "John Peterssen",  
    "usermeta": {  
        "ostEmployeeId": "494922944810349",  
        "ostVoicePhone": "+49-155-5558-878",  
        "ostHaulerPlate": "FM682RK",  
        "ostTrailerPlate": "OB462PY"  
    },  
    "colBg": "00d0d0d0",  
    "owetagdtuSync": {  
        "etag": "3bd0",  
        "v": "2021-11-16T19:33:34.368Z"  
    },  
    "owetagdtuRead": {  
        "etag": "3bd0",  
        "v": "2021-11-16T19:33:34.368Z"  
    },  
    "fMuted": false  
 }  
```

#### Fields
{% table %}
---
* **userxtid**
* string
* The client-generated identifier of the user entity representing this member of the chat room.
---
* **usern**
* string
* The name of this member.
---
* **usermeta**
* [usermeta](?p=/transaction-schemas#user-description-dubusermeta)
* User information.
---
* **colBg**
* [color](?p=/custom-menu-schemas#color)
* The color used to represent this member on the instantCMR Hub and on the mobile device.
---
* **owetagdtuSync**
* [wetagdtu](?p=/chat-schemas#version-with-date-and-time-wetagdtu) (optional)
* Receipt of the most recent message delivered to this member. Omitted when no messages have been delivered to this user yet.
---
* **owetagdtuRead**
* [wetagdtu](?p=/chat-schemas#version-with-date-and-time-wetagdtu) (optional)
* Receipt of the most recent message read by this member. Omitted when the user has not read any messages yet.
---
* **fMuted**
* boolean
* **True** if the user is a muted member of the conversation. **False** otherwise.
{% /table %}



### Chat room message author (Dubbomath)

A (former or present) member of the chat room who authored a message.

```json {% name="Dubbomath" %}
{
    "userxtid": "john.peterssen",
    "usern": "John Peterssen",
    "usermeta": {
        "ostEmployeeId": "494922944810349",
        "ostVoicePhone": "+49-155-5558-878",
        "ostHaulerPlate": "FM682RK",
        "ostTrailerPlate": "OB462PY"
    },
    "colBg": "00d0d0d0"
}
```

#### Fields
{% table %}
---
* **userxtid**
* string
* The client-generated identifier of the user entity representing this member of the chat room.
---
* **usern**
* string
* The name of this member.
---
* **usermeta**
* [usermeta](?p=/transaction-schemas#user-description-dubusermeta)
* User information.
---
* **colBg**
* [color](?p=/custom-menu-schemas#color)
* The color used to represent this member on the instantCMR Hub and on the mobile device.
{% /table %}




### Chat message (Dubpost)

A message posted in a chat room. Messages are uniquely identified by the client-generated identifier of the chat room (**roomxtid**) and the message sequence number (**etagpost**) of the message within the chat room. The sequence number of the messages in a chat room also determines the message ordering.

In addition to the **roomxtid** and **etagpost** pair, individual messages are required to have a unique **postxtid** generated by the client. This is necessary so that the Chat API can differentiate between two messages posted by the same author with identical contents.
```json {% name="Dubpost" %}
{
    "postxtid": "oh91tDqJySK8wur2V6ZNhg",
    "etagpost": "3bd1",
    "userxtid": "herman.mueller",
    "dtu": "2021-11-16T19:33:34.368Z",
    "payp": {
        "k": "msg",
        "st": "Hello there my friends!"
    }
}
```

#### Fields
{% table %}
---
* **postxtid**
* string
* The client-generated identifier of the message. Must be unique for each message in the same chat room.
---
* **etagpost**
* string
* Unique sequence number of the message. Determines message ordering within messages posted into the same chat room.
---
* **userxtid**
* string
* The client-generated identifier of the [User](?p=/transaction-schemas#user-dubuser) who posted this message.
---
* **dtu**
* datetime (utc)
* Date/time when message was posted.
---
* **payp**
* [dubpayp](?p=/transaction-schemas#message-payload-dubpayp)
* Message payload.
{% /table %}



### Message payload (Dubpayp)

Additional data related to the message. The shape of this entity is determined by the value of its **k** field.
```json {% name="Dubpayp" %}
{  

    "k": "msg",  
    "st": "Hello there my friends!"  
}  

{
    "k": "update",
    "roup": {
        "oroce": {},
        "otise": {
            "ostTitle": "Team 4 daily briefings"
        },
        "ousad": {
            "rguserxtid": [
                "obi.wan.kenobi", …
            ],
        },
        "ouski": {
            "rguserxtid": [
                "obi.wan.kenobi", …
            ],
        },
        "ousmu": {
            "rguserxtid": [
                "obi.wan.kenobi", …
            ],
        },
        "ousum": {
            "rguserxtid": [
                "obi.wan.kenobi", …
            ],
        },
    }
}
```

#### Fields
{% table %}

---
* **k**
* string
* Determines the type of message payload.
  {% list %}
  * **msg**: the payload contains a chat message posted.
  * **update**: the payload contains an update to the chat title.
  {% /list %}
---
* **st**
* string
* The message.
---
* **roup**
* [roup](?p=/transaction-schemas#chat-room-update-dubroup)
* A chat room update entity describing the changes a chat editor or chat administrator made to a chat room.
{% /table %}




### Chat room update (Dubroup)

Describes changes made to a chat room, such as changing the conversation title, adding, removing, muting or unmuting members.

```json {% name="Dubroup" %}
{
    "oroce": {},
    "otise": {
        "ostTitle": "Team 4 daily briefings"
    },
    "ousad": {
        "rguserxtid": [
            "obi.wan.kenobi", …
        ],
    },
    "ouski": {
        "rguserxtid": [
            "obi.wan.kenobi", …
        ],
    },
    "ousmu": {
        "rguserxtid": [
            "obi.wan.kenobi", …
        ],
    },
    "ousum": {
        "rguserxtid": [
            "obi.wan.kenobi", …
        ],
    },
}
```

#### Fields
{% table %}

---
* **oroce**
* object (optional)
* Denotes that the chat room was created by the author of the message. Omitted, if the chat room has not been created by this update.
---
* **otise**
* tise (optional)
* Denotes that the conversation title of the chat room has been updated. Omitted, if no conversation title update has been performed by this update.
---
* **ostTitle**
* string (optional)
* The conversation title of the chat room. If omitted, the payload represents the removal of the chat room title.
---
* **ousad**
* usad (optional)
* Denotes the new members added to the chat room by this update. Omitted, if no members have been added by this update.
---
* **ouski**
* uski (optional)
* Denotes the members removed from the chat room by this update. Omitted, if no members have been removed by this update.
---
* **ousmu**
* usmu (optional)
* Denotes the members muted in the chat room by this update. Omitted, if no members have been muted by this update.
---
* **ousum**
* usum (optional)
* Denotes the members unmuted in the chat room by this update. Omitted, if no members have been unmuted by this update.
---
* **rguserxtid**
* list of userxtid
* The list of client-generated user identifiers of members affected by this update.
{% /table %}


