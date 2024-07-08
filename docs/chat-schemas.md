# Chat API Schemas

## Chat room (Roomed)

Represents a chat room.

```json {% name="Roomed" %}
{
    "roomxtid": "team-4-briefing",
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
    "ostTitle": "Team 4 Daily Briefings",
    "dtuCreate": "2022-05-26T12:46:32.036Z",
    "rgboma": [
        {
            "userxtid": "494922944810349",
            "usern": "Bertram Friedrich",
            "usermeta": {
                "ostEmployeeId": "494922944810349",
                "ostVoicePhone": "+49-155-5558-878",
                "ostHaulerPlate": "FM682RK",
                "ostTrailerPlate": "OB462PY"
            },
            "colBg": "00ea1547",
            "owetagdtuSync": {
                "etag": "60e",
                "v": "2022-09-02T07:58:32.847Z"
            },
            "owetagdtuRead": {
                "etag": "60e",
                "v": "2022-09-02T12:56:14.185Z"
            },
            "fMuted": false
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
* Identifier of the chat room.
---
* **ouxtid**
* string
* Organization unit identifier.
---
* **rovered**
* [rovered](?p=/chat-schemas#chat-room-version-rovered)
* Chat room version.
---
* **ostTitle**
* string (optional)
* Conversation topic title as displayed to members of the chatroom. When omitted, a generic topic title, listing the participants of the conversation will be displayed.
---
* **dtuCreate**
* datetime (utc)
* Date/time when the chat room was created.
---
* **rgboma**
* list of [boma](?p=/chat-schemas#chat-room-member-boma)
* List of members in the chat room. Only members can see and post messages into the chat room. A chat room can have up to 100 members.
{% /table %}



## Chat room member (Boma)

Represents a member of a chat room.

```json {% name="Boma" %}
{
    "userxtid": "494922944810349",
    "usern": "Bertram Friedrich",
    "usermeta": {
        "ostEmployeeId": "494922944810349",
        "ostVoicePhone": "+49-155-5558-878",
        "ostHaulerPlate": "FM682RK",
        "ostTrailerPlate": "OB462PY"
    },
    "colBg": "00ea1547",
    "owetagdtuSync": {
        "etag": "60e",
        "v": "2022-09-02T07:58:32.847Z"
    },
    "owetagdtuRead": {
        "etag": "60e",
        "v": "2022-09-02T12:56:14.185Z"
    },
    "fMuted": false
}
```

### Fields
{% table %}
---
* **userxtid**
* string
* The client-generated identifier of the user entity representing this member of the chat room.
---
* **usern**
* string
* Name of the user.
---
* **usermeta**
* [usermeta](?p=/user-schemas#user-description-usermeta)
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

## Chat room version (Rovered)

Describes the version of a chat room. A chat room version consists of two sequence numbers represented as hexadecimal strings.

**wetagdturoom.etag** is bumped whenever any change is made to the chat room itself:

*   Arrival/departure of members or any change in member data.
*   Update to the conversation topic.
*   A new message being posted to the chat room.
*   A new delivery receipt or read receipt recorded for a member of the chat room.

**wetagdtupost.etag** is the sequence number of the most recent message posted in the chat room.
```json {% name="Rovered" %}
{  
    "wetagdturoom": {  
        "etag": "45a2f",  
        "v": "2021-11-16T19:33:34.368Z"  
    },  

    "wetagdtupost": {  
        "etag": "3bd1",  
        "v": "2021-11-16T19:33:34.368Z"  
    }  
}  
```

### Fields
{% table %}
---
* **wetagdturoom**
* [wetagdtu](?p=/chat-schemas#version-with-date-and-time-wetagdtu)
* Contains the version number of the chat room (aka **etagroom**) in its **etag** field, which also serves as the sequence number of the most recent chat room update. The **v** field contains the date and time of the last chat room update.
---
* **wetagdtupost**
* [wetagdtu](?p=/chat-schemas#version-with-date-and-time-wetagdtu)
* Contains the sequence number of the most recent message posted to the chat room (aka **etagpost**) in its **etag** field. The **v** field contains the date and time of the last message posted in the chat room.
{% /table %}


## Version with date and time (Wetagdtu)

This data structure is used to describe a version of an entity (etag) and the date and time (v) when the entity arrived at the particular version.

The instantCMR API uses this data structure to represent chat room versions (both **wetagdturoom** versioning any change to a chat room and **wetagdtupost** versioning messages sent to the chat room) in the [rovered](?p=/chat-schemas#chat-room-version-rovered) data structure.

In addition to that this data structure is also used to represent delivery or read receipts which contain the **etagpost** of the most recent message delivered to or read by a member of a chat room along with the date and time when the message was delivered/read.

```json {% name="Wetagdtu" %}
{  
    "etag": "60e",  
    "v": "2022-09-02T07:58:32.847Z"  
}  
```

### Fields
{% table %}
---
* **etag**
* string
* The chat room version (etagroom) or the message sequence number (etagpost).
---
* **v**
* datetime (utc)
* The date and time when the chat room or message delivery/read receipt was updated.
{% /table %}




## Chat room update (Roomeu)

Represents a chat room update.

```json {% name="Roomeu" %}
{
    "ouxtid": "BusinessUnit1",
    "rgboma": [
        {
            "userxtid": "jonas.paulsson",
            "fMuted": false
        },  
        …
    ],
    "ostTitle": "Team 4 Daily Briefings"
}
```

### Fields
{% table %}
---
* **ouxtid**
* string
* Organization unit identifier.
---
* **rgboma**
* list of [bomaeu](?p=/chat-schemas#chat-room-member-update-bomaeu)
* List of chat room members.
---
* **ostTitle**
* string (optional)
* Conversation topic title as displayed to members of the chatroom. When omitted, a generic topic title, listing the participants of the conversation will be displayed.
{% /table %}


## Chat room member update (Bomaeu)

Represents a chat room member update.

```json {% name="Bomaeu" %}
{  

    "userxtid": "jonas.paulsson",  
    "fMuted": false  
}  
```

### Fields
{% table %}
---
* **userxtid**
* list of string
* The client-generated identifier of the user entity representing this member of the chat room.
---
* **fMuted**
* boolean
* **True** if the member should be muted in the chat room. **False** otherwise.
{% /table %}




## New message entity (Posteu)
Represents a new message to be posted in a chat room.

```json {% name="Posteu" %}
{  
    "userxtid": "obi.wan.kenobi",  
    "stMessage": "Hello there!"  
}  
```

### Fields
{% table %}
---
* **userxtid**
* string
* The client-generated identifier of the user sending the message. May also be a user who is not a member of the chatroom.
---
* **stMessage**
* string
* Message content.
{% /table %}




## Message update result (Posted)
Represents a message posted in a chat room.
```json {% name="Posted" %}
{  
    "roomxtid": "team-4-briefing",  
    "etag": "9e",  
    "userxtid": "obi.van.kenobi",  
    "postxtid": "nd9z3ppn9z8dnp893np98",  
    "dtu": "2022-10-04T16:05:49.574Z",  
    "payp": {  
        "k": "msg",  
        "st": "Hello there!"  
    }  
}  
```

### Fields
{% table %}
---
* **roomxtid**
* string
* The client-generated identifier of the chat room the message was posted into.
---
* **etag**
* string
* The sequence number of the message (**etagpost**).
---
* **userxtid**
* string
* The client-generated identifier of the user sending the message.
---
* **postxtid**
* string
* The client-generated identifier of the message.
---
* **dtu**
* datetime (utc)
* The date and time when the message was stored by the Chat API.
---
* **payp**
* [payp](?p=/chat-schemas#message-payload-payp)
* Message payload.
{% /table %}

## Message payload (Payp)

Additional data related to the message. The shape of this entity is determined by the value of its **k** field.
```json {% name="Payp" %}
{
    "k": "msg",
    "st": "Hello there!"
}  

{
    "k": "update",
    "roup": {
        "oroce": {},
        "otise": {
            "ostTitle": "Team 4 daily briefings"
        },
        "ousad": {
            "rguserxtid": ["obi.wan.kenobi"]
        },
        "ouski": {
            "rguserxtid": ["obi.wan.kenobi"]
        },
        "ousmu": {
            "rguserxtid": ["obi.wan.kenobi"]
        },
        "ousum": {
            "rguserxtid": ["obi.wan.kenobi"]
        }
    }
}
```

### Fields
{% table %}
---
* **k**
* string
* Determines the type of message payload:
  {% list %}
    * **msg**: the payload contains a chat message posted.
    * **update**: the payload contains a chat room update.
  {% /list %}
---
* **st**
* string
* The message.
---
* **roup**
* [roup](?p=/chat-schemas#chat-room-update-roup)
* A chat room update entity describing the changes a chat editor or chat administrator made to a chat room.
{% /table %}





## Chat room update (Roup)

Describes changes made to a chat room, such as changing the conversation title, adding, removing, muting or unmuting members.

```json {% name="Roup" %}
{
    "oroce": {},
    "otise": {
        "ostTitle": "Team 4 daily briefings"
    },
    "ousad": {
        "rguserxtid": ["obi.wan.kenobi"]
    },
    "ouski": {
        "rguserxtid": ["obi.wan.kenobi"]
    },
    "ousmu": {
        "rguserxtid": ["obi.wan.kenobi"]
    },
    "ousum": {
        "rguserxtid": ["obi.wan.kenobi"]
    }
}

```
### Fields
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


