# User API Schemas

## User response (Usered)

Represents a user entity returned by the instantCMR API

```json {% name="Usered" %}
{
    "copid": "LogisticsGmbH",
    "ouxtid": "BusinessUnit1",
    "userxtid": "494922944810349",
    "usern": "Bertram Friedrich",
    "ocontact": {
        "ousern": "Bertram Friedrich",
        "email": "bertram.friedrich@logisticsgmbh.de",
    },
    "oaccn": "betram.friedrich",
    "locale": "de-DE",
    "tz": "Europe/Berlin",
    "ofDeleted": true,
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
    "roles": {
        "odriver": {
            "rgcontactCmr": [
                {
                    "ousern": "Harald Weber",
                    "email": "harald.weber@logisticsgmbh.de",
                },  
                …
            ],
            "rgcontactAcc": […
            ],
            "rgcontactGdam": […
            ],
            "rgcontactMisc": […
            ]
        },
        "odisp": {},
        "orev": {},
        "odia": {},
        "ochedit": {},
        "ochadmin": {},
        "oiep": {}
    },
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
* **copid**
* string
* Company identifier assigned by instantCMR.
---
* **ouxtid**
* string
* Organization unit identifier.
---
* **userxtid**
* string
* Client-generated user identifier.
---
* **usern**
* string
* Name of user.
---
* **ocontact**
* [contact](?p=/user-schemas#email-contact-contact) (optional)
* Email address of user. instantCMR uses this contact information to send license invitation and password renewal emails to this user. 
  
  If omitted, user will not receive invitation or password reset emails.
---
* **oaccn**
* string (optional)
* Account name of user.
Used to form the login name of the user on the instantCMR Hub. Users can login to the instantCMR Hub with the login name **oaccn@copid** (e.g. **betram.friedrich@LogisticsGmbH**). The account name may only consist of letters, numbers and the **‘.’** (dot) and **‘-’** (dash) characters. 
  
  If omitted, an account name will be generated from the value of the **usern** field for each user whose roles provide access to the instantCMR Hub (**disp**, **rev**, **dia, chedit, chadmin**). The generated account name will contain all valid characters from **usern**, with letters converted to lowercase, whitespaces replaced with **‘.’** (dot) character. E.g. the user name **“Betram Friedrich-Strauss+69”** will be converted to an account name of **“bertram.friedrich-strauss69”**. The account name (both if present or generated from user name) must be unique for each user of the company denoted by **copid**.
---
* **locale**
* string
* ISO 639 alpha-2 or alpha-3 language code identifying the language of email notifications sent to this user.
---
* **tz**
* string
* The IANA time zone database name of the time zone used to format date and time info in email correspondence with this user.
---
* **ofDeleted**
* boolean (optional)
* Only present (with a value of **true**), if the user has been deactivated. Deactivated users are unable to use instantCMR but their historic data is kept around.
---
* **usermeta**
* [usermeta](?p=/user-schemas#user-description-usermeta)
* User information.
---
* **dboxc**
* [dboxc](?p=/document-storage-schemas#document-storage-configuration-dboxc)
* Configuration of the user’s document storage.
---
* **roles**
* [roles](?p=/user-schemas#user-roles-roles)
* Roles assigned to the user.
---
* **rgulic**
* list of [ulic](?p=/user-schemas#user-license-assignment-ulic)
* List of licenses assigned to the user.
{% /table %}




## User update (Usereu)

Represents a user update sent to the instantCMR API.

```json {% name="Usereu" %}
{
    "ouxtid": "BusinessUnit1",
    "usern": "Bertram Friedrich",
    "ocontact": {
        "ousern": "Bertram Friedrich",
        "email": "bertram.friedrich@logisticsgmbh.de",
    },
    "oaccn": "betram.friedrich",
    "locale": "de-DE",
    "tz": "Europe/Berlin",
    "ofDeleted": true,
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
    "roles": {
        "odriver": {
            "rgcontactCmr": [
                {
                    "ousern": "Harald Weber",
                    "email": "harald.weber@logisticsgmbh.de",
                },  
                …
            ],
            "rgcontactAcc": […],
            "rgcontactGdam": […],
            "rgcontactMisc": […]
        },
        "odisp": {},
        "orev": {},
        "odia": {},
        "ochedit": {},
        "ochadmin": {},
        "oiep": {}
    }
}
```

### Fields
{% table %}
---
* **ouxtid**
* string
* Organization unit identifier.
---
* **userxtid**
* string
* Client-generated user identifier.
---
* **usern**
* string
* Name of user.
---
* **ocontact**
* [contact](?p=/user-schemas#email-contact-contact) (optional)
* Email address of user. instantCMR uses this contact information to send license invitation and password renewal emails to this user. If omitted, user will not receive invitation or password reset emails.
---
* **oaccn**
* string (optional)
* Account name of user. Used to form the login name of the user on the instantCMR Hub. Users can login to the instantCMR Hub with the login name **oaccn@copid** (e.g. **betram.friedrich@LogisticsGmbH**). The account name may only consist of letters, numbers and the **‘.’** (dot) and **‘-’** (dash) characters.

  If omitted, an account name will be generated from the value of the **usern** field for each user whose roles provide access to the instantCMR Hub (**disp**, **rev**, **dia, chedit, chadmin**). The generated account name will contain all valid characters from **usern**, with letters converted to lowercase, whitespaces replaced with **‘.’** (dot) character. E.g. the user name **“Betram Friedrich-Strauss+69”** will be converted to an account name of **“bertram.friedrich-strauss69”**. The account name (both if present or generated from user name) must be unique for each user of the company denoted by **copid**.
---
* **locale**
* string
* ISO 639 alpha-2 or alpha-3 language code identifying the language of email notifications sent to this user.
---
* **tz**
* string
* The IANA time zone database name of the time zone used to format date and time info in email correspondence with this user.
---
* **ofDeleted**
* boolean (optional)
* Only present (with a value of **true**), if the user has been deactivated. Deactivated users are unable to use instantCMR but their historic data is kept around.
---
* **usermeta**
* [usermeta](?p=/user-schemas#user-description-usermeta)
* User information.
---
* **dboxc**
* [dboxc](?p=/document-storage-schemas#document-storage-configuration-dboxc)
* Configuration of the user’s document storage.
---
* **roles**
* [roles](?p=/user-schemas#user-roles-roles)
* Roles assigned to the user.
{% /table %}



## Email contact (Contact)

Represents an email address.

```json {% name="Contact" %}
{
    "ousern": "Harald Weber",
    "email": "harald.weber@logisticsgmbh.de"
}
```

### Fields
{% table %}
---
* **ousern**
* string (optional)
* Display name of contact.
---
* **email**
* string
* Email address of contact. Please observe [Anti-Spam requirements for email contacts](?p=/anti-spam#anti-spam-requirements-for-email-contacts).
{% /table %}




## User description (Usermeta)

Fields describing the user.

```json {% name="Usermeta" %}
{
    "ostEmployeeId": "494922944810349",
    "ostVoicePhone": "+49-155-5558-878",
    "ostHaulerPlate": "FM682RK",
    "ostTrailerPlate": "OB462PY"
}
```

### Fields
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






## User roles (Roles)

Roles assigned to a user.

```json {% name="Roles" %}
{
    "odriver": {
        "rgcontactCmr": [
            {
                "ousern": "Harald Weber",
                "email": "harald.weber@logisticsgmbh.de",
            },  
            …
        ],
        "rgcontactAcc": […],
        "rgcontactGdam": […],
        "rgcontactMisc": […]
    },
    "odisp": {},
    "orev": {},
    "odia": {},
    "ochedit": {},
    "ochadmin": {},
    "oiep": {}
}
```

### Fields

{% table %}
---
* **odriver**
* [driverrole](?p=/user-schemas#driver-role-driverrole) (optional)
* Present if the user is a driver, omitted otherwise. Drivers can be assigned mobile devices and trips and can submit documents.
---
* **odisp**
* object (optional)
* Present if the user is a dispatcher, omitted otherwise. When present, the value is an empty object. Dispatchers can manage other users’ document storages and inboxes on the instantCMR Hub.
---
* **orev**
* object (optional)
* Present if the user is a reviewer, omitted otherwise. When present, the value is an empty object. Reviewers can manage other users’ trips on the instantCMR Hub.
---
* **odia**
* object (optional)
* Present if the user is a device inventory administrator, omitted otherwise. When present, the value is an empty object. Device inventory administrators can manage mobile devices and assign these devices to drivers on the instantCMR Hub.
---
* **ochedit**
* object (optional)
* Present if the user is a chat editor, omitted otherwise. When present, the value is an empty object. Chat editors can create and manage chat groups in their organization unit that they are also a member of.
---
* **chadmin**
* object (optional)
* Present if the user is a chat administrator, omitted otherwise. When present, the value is an empty object. Chat administrators can create, manage, read and post messages into any chat group within their organization unit.
---
* **oiep**
* object (optional)
* Present if the user has API access to instantCMR, omitted otherwise. When present, the value is an empty object.
{% /table %}




## Driver role (Driverrole)

Represents a driver role.

```json {% name="Driverrole" %}
{
    "rgcontactCmr": [
        {
            "ousern": "Harald Weber",
            "email": "harald.weber@logisticsgmbh.de",
        },  
        …
    ],
    "rgcontactAcc": […],
    "rgcontactGdam": […],
    "rgcontactMisc": […]
}
```

### Fields
{% table %}
---
* **rgcontactCmr**
* list of [contact](?p=/user-schemas#email-contact-contact)
* List of email contacts to receive email notification whenever this driver submits documents of [type](?p=/trip-schemas#document-types-kdocr) **cmr**, **dlvryn**, **palletn**, **custd**, **misc**, **wbt**, **thesc**, **sanid**, **wayb**, **wmad**, **dad**, **bol**, **rep**.
---
* **rgcontactAcc**
* list of [contact](?p=/user-schemas#email-contact-contact)
* List of email contacts to receive email notification whenever this driver submits documents of [type](?p=/trip-schemas#document-types-kdocr) **acc**.
---
* **rgcontactGdam**
* list of [contact](?p=/user-schemas#email-contact-contact)
* List of email contacts to receive email notification whenever this driver submits documents of [type](?p=/trip-schemas#document-types-kdocr) **gdam**.
---
* **rgcontactMisc**
* list of [contact](?p=/user-schemas#email-contact-contact)
* List of email contacts to receive email notification whenever this driver submits documents of [type](?p=/trip-schemas#document-types-kdocr) **miscph**.
{% /table %}



## User license assignment (Ulic)

Represents a license assigned to a user.

```json {% name="Ulic" %}
{  
    "kid": "oh91tDqJySK8wur2V6ZNhg",  
    "ostDeviceModel": "SAMSUNG J330F",  
    "ostDeviceImei": "302828644031295",  
    "ostPin": "1111",  
    "ostPhone": "+49-152-5552-942",  
    "ostImsi": "082926209143692255",  
    "ostSubscription": "Vodafone Red" 
}  
```

### Fields
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

