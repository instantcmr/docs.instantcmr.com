# Trip API Schemas

## Trip response (Triped)

Represents a trip returned by the instantCMR API.


```json {% name="Triped" %}
{  

    "ouid": "EUT",  
    "ktroc": "o",  
    "ostSortBy": "0000000",  
    "dtuCreate": "2020-02-28T09:49:40.706Z",  
    "tripmeta": {  
        "ostTripCode": "200026571",  
        "ostCustomer": "BATHSYSTEM SPA",  
        "odtuCreate": "2020-02-27T10:20:23.839Z",  
        "ostInstructions": "use 6 ratchet straps",  
        "ostNotes": "call upon arrival",  
        "ostHaulerPlate": "FM682RK",  
        "ostTrailerPlate": "OB462PY"  
    },  
    "ouserxtidDriver": "494922944810349",  
    "rgdocr": [  
        {  
            "docrid": "iGq-fxXujlJ3VSCKtr5Mtw",  
            "dtuRequest": "2020-02-27T10:20:23.840Z",  
            "kdocr": "acc",  
            "fRequired": false  
        },  
        …  
    ],  
    "rgstan": [  
        {  
            "stanxtid": "geqcQAOxUbkeS-N8jEtJOQ",  
            "kstan": "ld",  
            "odtu": "2019-12-02T06:30:00.000Z",  
            "ostAddress": "IT 25011 Calcinato, VIA CAVOUR 149",  
            "ostNotes": "gate code: A678*",  
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
                "fpat": "document.pdf",  
                "cb": 11741,  
                "dtuUpload": "2021-11-16T19:33:34.368Z",  
                "urlv": {  
                    "url": "https://image-url-1"",  
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
* **ouid**
* string
* Organization unit id
---
* **ktroc**
* [ktroc](?p=/trip-schemas#trip-open-close-status-ktroc)
* Trip open/close status.
---
* **ostSortBy**
* string (optional)
* Defines the sort order of trips as shown to the driver. Trips will be shown in lexicographic order sorted by their first field that is provided from the following list: **ostSortBy**, **tripmeta.odtuCreate**, **dtuCreate**
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
* **rgstan**
* list of [stan](?p=/trip-schemas#station-stan)
* List of stations.
---
* **ruts**
* [ruts](?p=/trip-schemas#trip-attachment-storage-ruts)
* Trip attachments.
---
* **cufis**
* [cufis](?p=/trip-schemas#custom-fields-cufis)
* Custom fields of the trip or station.
{% /table %}


## Trip update (Tripeu)

Represents a trip update sent to the instantCMR API.

```json {% name="Tripeu" %}
{
    "ouid": "EUT",
    "ktroc": "o",
    "ostSortBy": "0000000",
    "tripmeta": {
        "ostTripCode": "200026571",
        "ostOrderId": "AB2398VBA4",
        "ostAddress": "IT 25011 Calcinato, VIA CAVOUR 149",
        "ostCustomer": "BATHSYSTEM SPA",
        "odtuCreate": "2020-02-27T10:20:23.839Z",
        "ostInstructions": "use 6 ratchet straps",
        "ostNotes": "call upon arrival",
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
            "ostDesc": "scan all pages",
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
            "ostNotes": "gate code: A678*",
            "okmDistance": 825,
            "ostSortBy": "0000000",
            "cufis": {
                "rgcufi": [
                    {
                        "id": "parking_lot",
                        "n": "Parking Lot",
                        "v": "Lot No. 2"
                    },  
                    …
                ]
            }
        },  
        …
    ],
    "cufis": {
        "rgcufi": [
            {
                "id": "reference_number",
                "n": "Reference Number",
                "v": "202203-DE-19373"
            },  
            …
        ]
    }
}
```

### Fields
{% table %}
---
* **ouid**
* string
* Organization unit id
---
* **ktroc**
* [ktroc](?p=/trip-schemas#trip-open-close-status-ktroc)
* Trip open/close status.
---
* **ostSortBy**
* string (optional)
* Defines the sort order of trips as shown to the driver. Trips will be shown in lexicographic order sorted by their first field that is provided from the following list: **ostSortBy**, **tripmeta.odtuCreate**, **dtuCreate**
---
* **dtuCreate**
* datetime (utc)
* Date and time when this trip was created.
---
* **tripmeta**
* [tripmeta](?p=/trip-schemas#trip-description-tripmeta)
* Fields describing the trip to the driver.
---
* **ouserxtidDriver**
* string (optional)
* Client-generated user id of the driver this trip is assigned to. (omit if the trip should be unassigned from drivers).
---
* **rgdocr**
* list of [docr](?p=/trip-schemas#document-request-docr)
* List of document requests.
---
* **rgstan**
* list of [stan](?p=/trip-schemas#station-stan)
* List of stations.
---
* **cufis**
* [cufis](?p=/trip-schemas#custom-fields-cufis)
* Custom fields of the trip or station.
{% /table %}



## Trip open/close status (Ktroc)

Represents the status of the trip determining whether the trip is visible to the driver and/or whether documents can be still submitted to the trip.

### Values
{% table %}
---
* **o**
* Open. Trip is shown for the driver and available for documents to be submitted
---
* **mfc**
* Closed (by driver). Trip is shown for the driver (as closed) and no documents can be submitted. All required document requests have a document submitted for them.
---
* **c**
* Closed. Trip has been closed by the dispatcher. Trip is not shown for the driver anymore.
{% /table %}



## Trip description (Tripmeta)

Fields describing the trip to the driver.

```json {% name="Tripmeta" %}
{  
    "ostTripCode": "200026571",  
    "ostCustomer": "BATHSYSTEM SPA",  
    "odtuCreate": "2020-02-27T10:20:23.839Z",  
    "ostInstructions": "use 6 ratchet straps",  
    "ostNotes": "call upon arrival",  
    "ostHaulerPlate": "FM682RK",  
    "ostTrailerPlate": "OB462PY"  
}  
```

### Fields
{% table %}
---
* **ostTripCode**
* string  
(optional)
* Trip code.
---
* **ostOrderId**
* string  
(optional)
* Order id.
---
* **ostAddress**
* string  
(optional)
* Address relevant to the trip (displayed to the driver in trip headings).
---
* **ostCustomer**
* string (optional)
* Name of customer.
---
* **odtuCreate**
* datetime (utc) (optional)
* Date and time when this trip was created (as displayed to the driver)
---
* **ostInstructions**
* string (optional)
* Instructions.
---
* **ostNotes**
* string (optional)
* Additional notes attached to the trip.
---
* **ostHaulerPlate**
* string (optional)
* License plate of the hauler.
---
* **ostTrailerPlate**
* string (optional)
* License plate of the trailer.
{% /table %}




## Document request (Docr)

Represents a document requested from the driver.

```json {% name="Docr" %}
{  
    "docrid": "iGq-fxXujlJ3VSCKtr5Mtw",  
    "dtuRequest": "2020-02-27T10:20:23.840Z",  
    "kdocr": "acc",  
    "fRequired": false  
}  
```

### Fields
{% table %}
---
* **docrid**
* string
* Client-generated document request id.
---
* **dtuRequest**
* datetime (utc)
* Date and time when this document request was created. Also determines the order in which document requests are displayed to the driver.
---
* **kdocr**
* [kdocr](?p=/trip-schemas#document-types-kdocr)
* Type of document requested.
---
* **ostShortName**
* string  
(optional)
* A short name of this document request.
---
* **ostDesc**
* string  
(optional)
* A (possibly multiline) message or note attached to this document request.
---
* **fRequired**
* boolean
* Specifies if the document request is mandatory, ie. if a document must be submitted for this request before the trip can be closed by the driver.
---
* **ofDeleted**
* boolean (optional)
* Only present (with a value of **true**), if the document request has been removed from this trip.
{% /table %}



## Document types (Kdocr)

The following table defines the document types that can be requested from or submitted by the drivers.

{% table %}
* Kdocr
* Cropped?
* Description (english)
* Description (hungarian)
---
* **cmr**
* yes
* CMR
* CMR
---
* **dlvryn**
* yes
* Delivery note
* Szállítólevél
---
* **palletn**
* yes
* Pallet receipt
* Raklapjegyzék
---
* **custd**
* yes
* Customs document
* Vámokmány
---
* **acc**
* no
* Vehicle accident report
* Gépjárműkár-bejelentés
---
* **misc**
* yes
* Miscellaneous document
* Egyéb dokumentum
---
* **wbt**
* yes
* Weighbridge ticket
* Mérlegjegy
---
* **miscph**
* no
* Miscellaneous report
* Egyéb esemény
---
* **thesc**
* yes
* Thermoscript
* Thermoscript
---
* **sanid**
* yes
* Sanitarian documents
* Egészségügyi papír
---
* **gdam**
* no
* Goods damage report
* Árukár-bejelentés
---
* **wayb**
* yes
* Waybill
* Menetlevél
---
* **wmad**
* yes
* Waste material accompanying document
* Hulladékkísérő okmány
---
* **dad**
* yes
* Damage declaration
* Kártérítési nyilatkozat
---
* **bol**
* yes
* Bill of lading
* Rakodási jegyzék
---
* **rep**
* yes
* Residence permit
* Tartózkodási igazolás
{% /table %}

In the above table
{% table %}

---
* Kdocr
* The value of the kdocr field in document request or document update entities.
---
* Cropped?
* Indicates if the images attached to the document will be automatically cropped and perspective-corrected by the instantCMR App.
---
* Description (english)
* The title of the document request/document as shown to the driver on mobile devices with english locale. Also used for unsupported locales.
---
* Description (hungarian)
* The title of the document request/document as shown to the driver on mobile devices with hungarian locale.
{% /table %}




## Station (Stan)

Represents a station of the trip's itinerary.

```json {% name="Stan" %}
{
    "stanxtid": "geqcQAOxUbkeS-N8jEtJOQ",
    "kstan": "ld",
    "odtu": "2019-12-02T06:30:00.000Z",
    "ostAddress": "IT 25011 Calcinato, VIA CAVOUR 149",
    "ostNotes": "gate code: A678*",
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
}
```

### Fields
{% table %}
---
* **stanxtid**
* string (optional)
* Client-generated station id.
---
* **kstan**
* kstan
* Station type.
---
* **odtu**
* datetime (utc)
* Date and time relevant to the station.
---
* **ostAddress**
* string (optional)
* Address of the station.
---
* **ostNotes**
* string (optional)
* Additional notes attached to the station.
---
* **okmDistance**
* number (optional)
* Distance travelled in kilometers.
---
* **ostSortBy**
* string (optional)
* Defines the sort order of stations as shown to the driver. Stations will be shown in lexicographic order sorted by their first field that is provided from the following list: **ostSortBy**, **odtu**.
---
* **cufis**
* [cufis](?p=/trip-schemas#custom-fields-cufis)
* Custom fields of the station.
---
* **ofDeleted**
* boolean (optional)
* Only present (with a value of **true**), if the station has been removed from this trip.
{% /table %}






## Station types (Kstan)

The following table defines the types of trip stations that can be sent to drivers.

{% table %}
---
* Kstan
* Description (english)
* Description (hungarian)
---
* **ld**
* Loading
* Felrakás
---
* **ul**
* Unloading
* Lerakás
---
* **h**
* Hook up/down
* Átakasztás
---
* **ds**
* Driver swap
* Gépjárművezető csere
---
* **owp**
* Mandatory waypoint
* Kötelező érintési pont
---
* **cuc**
* Customs clearance
* Vámkezelés
---
* **c**
* Crossing
* Átkelés
---
* **tun**
* Tunnel
* Alagút
---
* **rc**
* Railway crossing
* Vasúti átkelés
---
* **ref**
* Refuelling
* Tankolás
---
* **p**
* Parking
* Parkolás
---
* **fer**
* Ferry
* Komp
---
* **wei**
* Weighing
* Mérlegelés
---
* **bc**
* Border crossing
* Határátkelés
---
* **res**
* Resting
* Pihenő
---
* **acs**
* Arrival to customer site
* Érkezés vevői telepre
---
* **dcs**
* Departure from customer site
* Indulás vevői telepről
{% /table %}

In the above table
{% table %}
---
* Kstan
* The value of the kstan field in station or station update entities.
---
* Description (english)
* The title of the station as shown to the driver on mobile devices with english locale. Also used for unsupported locales.
---
* Description (hungarian)
* The title of the station as shown to the driver on mobile devices with hungarian locale.
{% /table %}



## Custom fields (Cufis)

Represents a list of custom fields attached to a trip or a station of a trip. Custom fields are displayed to the driver in the instantCMR mobile app and may contain supplementary information about the trip or the station they are attached to.

Represents the documents and routes attached to the trip or to a station of the trip.

```json {% name="Cufis" %}
{
    "rgcufi": [
        {
            "id": "reference_number",
            "n": "Reference Number",
            "v": "202203-DE-19373"
        },  
        …
    ]
}
```

### Fields
{% table %}
---
* **rgcufi**
* list of [cufi](?p=/trip-schemas#custom-field-cufi)
* List of custom fields.
{% /table %}


## Custom field (Cufi)

Represents a single custom field attached to a trip or a station of a trip. The **id** field serves a dual purpose: it is used to determine the display order of custom fields by sorting the fields in the alphabetical order of their **id**s. Custom fields also maintain their read/unread status on the mobile device. This unread status gets updated whenever either the value of the label of a custom field changes. To detect updates of an existing custom field the **id** of custom fields is used.

Custom fields are displayed as a label and a value, both provided in the Cufi entity.

```json {% name="Cufi" %}
{
    "id": "reference_number",
    "n": "Reference Number",
    "v": "202203-DE-19373"
}  
```

### Fields
{% table %}
---
* **id**
* string
* User-provided identifier of the custom field.
---
* **n**
* string
* Label of the custom field.
---
* **v**
* string
* Value of the custom field.
{% /table %}



## Trip attachment storage (Ruts)

Represents the documents and routes attached to the trip or to a station of the trip.

```json {% name="Ruts" %}
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

### Fields
{% table %}
---
* **rgrut**
* list of [rut](?p=/trip-schemas#trip-attachment-rut)
* List of attachments.
{% /table %}



## Trip attachment (Rut)

Represents a document or route attached to the trip or to a station of a trip.

### Fields
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

