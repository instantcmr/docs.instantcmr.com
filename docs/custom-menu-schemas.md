
## Custom menu 

The Custom Menu (**[Scren](?p=/custom-menu-schemas#custom-menu-scren)**) is a collection of widgets (**[Mbut](?p=/custom-menu-schemas#widget-mbut)**) that are displayed on a driver's interface. It is defined by a list of widget definitions, allowing for flexible customization of the driver's menu. Each widget added to this menu can perform different actions, such as synchronizing data, navigating to lists, or launching external applications, thereby enhancing the driver's ability to manage tasks efficiently.

Widgets (**Mbut**) are individual elements that can be configured to appear on the Custom Menu. Each widget is defined by parameters like action, position, size, and style settings. The versatility of widgets allows developers to create dynamic menus tailored to the driver's specific needs.

The Widget Action (**[Buta](?p=/custom-menu-schemas#widget-action-buta)**) parameter specifies the type and behavior of the widget. This parameter defines what the widget will display and how it will interact with the driver. For example, the **[Sync](?p=/custom-menu-schemas#synchronization-indicator-sync)** action triggers data synchronization, while **[triplist](?p=/custom-menu-schemas#navigate-to-list-of-trips-triplist)** and **[roomlist](?p=/custom-menu-schemas#navigate-to-list-of-chat-rooms-roomlist)** navigate to lists of trips and chat rooms respectively. Additional actions like the Driver’s File Storage (**[Dbox](?p=/custom-menu-schemas#navigate-to-driver’s-file-storage-dbox)**), Document Scanner (**[Docr](?p=/custom-menu-schemas#scan-document-docr)**), and External App Launcher (**[Launchactivity](?p=/custom-menu-schemas#launch-external-app-launchactivity)**) further extend the capabilities of widgets. These components provide predefined actions and behaviors, ensuring that common tasks can be integrated seamlessly into the custom menu.


## Custom menu (Scren)

Custom menu of a driver.

```json {% name="Scren" %}
{  
    "rgmbut": [  
        mbut-1,  
        …,  
        mbut-N  
    ]  
}  
```

### Fields
{% table %}
---
* **rgmbut**
* list of [mbut](?p=/custom-menu-schemas#widget-mbut)
* List of widget definitions to appear in the drivers menu.
{% /table %}



## Widget (Mbut)

A widget that can appear on a custom menu.

```json {% name="Mbut" %}
{  

    "buta": buta,  
    "ptd": ptd,  
    "szd": szd,  
    "busty": busty,  
     "rgnotd": [notd1, …, notdN]  
}  
```

### Fields
{% table %}
---
* **buta**
* [buta](?p=/custom-menu-schemas#widget-action-buta)
* Entity describing the widgets action and displayed data.
---
* **ptd**
* [ptd](?p=/custom-menu-schemas#widget-position-ptd)
* Widget position.
---
* **szd**
* [szd](?p=/custom-menu-schemas#widget-size-szd)
* Widget size.
---
* **busty**
* [busty](?p=/custom-menu-schemas#widget-style-busty)  
(optional)
* Widget appearance.
---
* **rgnotd**
* list of [notd](?p=/custom-menu-schemas#custom-notification-notd)
* List of custom notifications to be shown to the user when the screen is downloaded.
{% /table %}



## Widget action (Buta)

Describes the action performed and/or the data displayed by the widget.

```json {% name="Buta" %}
{  
    "k": kbuta,  
    other-parameters  
 }  
```

### Fields
{% table %}
---
* **k**
* string
* Widget type. Either **[sync](?p=/custom-menu-schemas#synchronization-indicator-sync)**, **[triplist](?p=/custom-menu-schemas#navigate-to-list-of-trips-triplist)**, **[roomlist](?p=/custom-menu-schemas#navigate-to-list-of-chat-rooms-roomlist)**, **[dbox](?p=/custom-menu-schemas#navigate-to-driver’s-file-storage-dbox)**, **[docr](?p=/custom-menu-schemas#scan-document-docr)** or **[launchactivity](?p=/custom-menu-schemas#launch-external-app-launchactivity)**.
---
* **other-parameters**
* 
* Other parameters describing the behavior of the widget. Depends on the widget type.
{% /table %}





## Synchronization indicator (Sync)

Shows a button that triggers synchronization with the backend when pressed. Also displays the state of any ongoing synchronization and warns about any staleness of the data..

```json {% name="Sync" %}
{  
    "k": "sync"  
}  
```


## Navigate to list of trips (Triplist)

Shows a button that navigates to the list of trips assigned to the driver. Also displays the number of updated or new trips yet unread by the driver.

```json {% name="Triplist" %}
{  
    "k": "triplist"  
}  
```


## Navigate to list of chat rooms (Roomlist)

Shows a button that navigates to the list of chat rooms the driver is a member of. Also displays the number of new messages in all chat rooms yet unread by the driver.

```json {% name="Roomlist" %}
{  
    "k": "roomlist"  
}  
```


## Navigate to driver’s file storage (Dbox)

Shows a button that navigates to the list of documents uploaded to the driver’s file storage. Also displays the number of updated or new documents yet unread by the driver.

```json {% name="Dbox" %}
{  
    "k": "dbox"  
}      
```


## Scan document (Docr)

Shows a button that allows the driver to scan and submit a new document. The document type can be configured via the kdocr field.

```json {% name="Docr" %}
{  
    "k": "docr",  
    "kdocr": "gdam"  
}  
```

### Fields
{% table %}
---
* **k**
* string
* Must be **docr**.
---
* **kdocr**
* [kdocr](?p=/trip-schemas#document-types-kdocr)
* The type of document to be scanned.
{% /table %}



## Launch external app (Launchactivity)

Shows a button that allows the driver to launch an external application on the device.

```json {% name="Launchactivity" %}
{  
    "k": "launchactivity",  
    "inspe": {  
        "oaction": "android.intent.action.MAIN",  
        "orgcat": ["android.intent.category.APP_CALCULATOR"]  
     }  
}  
```

### Fields
{% table %}
---
* **k**
* string
* Must be **launchactivity**.
---
* **inspe**
* [inspe](?p=/custom-menu-schemas#android-intent-inspe)
* The [Android Intent](https://developer.android.com/reference/android/content/Intent) to be fired when the button is pressed.
{% /table %}




## Android intent (Inspe)

Describes the [Android Intent](https://developer.android.com/reference/android/content/Intent) that is to be fired when the user presses a [Launch External App](?p=/custom-menu-schemas#launch-external-app-launchactivity) button on the menu.

```json {% name="Inspe" %}
{  
    "oaction": intent-action,  
    "orgcat": [cat-1, …, cat-N],  
    "ocompn": component-name,  
    "odata": data-uri,  
    "omime": mime-type,  
    "ompextra": intent-extras  
}  
```

### Fields
{% table %}
---
* **oaction**
* string (optional)
* The intent action.
See also [android: Intent.setAction](https://developer.android.com/reference/android/content/Intent#setAction\(java.lang.String\)).
---
* **orgcat**
* list of strings (optional)
* The list of intent categories the launched component must handle in order to be resolved for this intent.

See also [android: Intent.setComponent](https://developer.android.com/reference/android/content/Intent#setComponent\(android.content.ComponentName\)).

---
* **ocompn**
* compn (optional)
* The explicit name of the target component to be launched by this intent. Intents that specify an explicit target component are called explicit intents and are not subject to implicit resolution.

  See also [android: Intent.setComponent](https://developer.android.com/reference/android/content/Intent#setComponent\(android.content.ComponentName\)).

---
* **odata**
* string (optional)
* The uri of the data to be passed by this intent.

  See also [android: Intent.setData](https://developer.android.com/reference/android/content/Intent#setData\(android.net.Uri\)).

---
* **omime**
* string (optional)
* The mime-type of the data provided by the data uri specified in **odata**. When omitted, the mime-type will be automatically inferred from **odata**. See also [android: Intent.setDataAndType](https://developer.android.com/reference/android/content/Intent#setDataAndType\(android.net.Uri,%20java.lang.String\)).

---
* **ompextra**
* [mpextra](?p=/custom-menu-schemas#intent-extras-mpextra) (optional)
* Extra parameters to be passed to the target component launched by this intent.

  See also [android: Intent.putExtras](https://developer.android.com/reference/android/content/Intent#putExtras\(android.os.Bundle\)).
{% /table %}



## Component name (Compn)

Specifies the exact target component to be executed by an intent. See also [android: ComponentName](https://developer.android.com/reference/android/content/ComponentName).

```json {% name="Compn" %}
{  
    "packagen": "com.example.sampletargetapp",  
    "classn": "com.example.sampletargetapp.TargetActivity"  
 }  
```
### Fields
{% table %}
---
* **packagen**
* string
* The package name of the component.
See also [android: ComponentName.getPackageName](https://developer.android.com/reference/android/content/ComponentName#getPackageName\(\)).

---
* **classn**
* string
* The class name of the component.

  See also [android: ComponentName.getClassName](https://developer.android.com/reference/android/content/ComponentName#getClassName\(\))

{% /table %}



## Intent extras (Mpextra)

Specifies a list of named parameters to be passed to the target component.

```json {% name="Mpextra" %}
{  
    "parameter-name-1": extra-value-1,  
    …,  
    "parameter-name-N": extra-value-N,  
 }  
```

### Fields
{% table %}
---
* **parameter-name**
* string
* The name of the parameter to pass to the target component.
---
* **extra-value**
* [Ev](?p=/custom-menu-schemas#extra-value-ev)
* The type and value of the parameter.
{% /table %}





## Extra value (Ev)

Specifies a single parameter value along with its type. Can be one of the following.

### Evbyte

A parameter of type **byte**.
```json {% name="Evbyte" %}
{  
    "k": "byte",  
    "v": 255  
}  
```

#### Fields
{% table %}
---
* **k**
* Kev
* Must be **byte**.
---
* **v**
* byte
* The value of the parameter to pass to the target
{% /table %}
### Evshort

A parameter of type **short**.
```json {% name="Evshort" %}
{  
    "k": "short",  
    "v": 128  
 }  
```

#### Fields
{% table %}
---
* **k**
* Kev
* Must be **short**.
---
* **v**
* short
* The value of the parameter to pass to the target
{% /table %}
### Evint

A parameter of type **int**.
```json {% name="Evint" %}
{  
    "k": "int",  
    "v": 2147483647  
}  
```

#### Fields
{% table %}

---
* **k**
* Kev
* Must be **int**.
---
* **v**
* int
* The value of the parameter to pass to the target
{% /table %}
### Evlong

A parameter of type **long**.
```json {% name="Evlong" %}
{  
    "k": "long",  
    "v": 9223372036854775807  
 }  
```


#### Fields
{% table %}
---
* **k**
* Kev
* Must be **long**.
---
* **v**
* long
* The value of the parameter to pass to the target
{% /table %}
### Evfloat

A parameter of type **float**.
```json {% name="Evfloat" %}
{  
    "k": "float",  
    "v": 3.14  
}  
```

#### Fields
{% table %}
---
* **k**
* Kev
* Must be **float**.
---
* **v**
* float
* The value of the parameter to pass to the target
{% /table %}

### Evdouble
A parameter of type **double**.
```json {% name="Evdouble" %}
{  
    "k": "double",  
    "v": 3.14   
}  
```

#### Fields
{% table %}
---
* **k**
* Kev
* Must be **double**.
---
* **v**
* double
* The value of the parameter to pass to the target
{% /table %}

### Evboolean
A parameter of type **boolean**.
```json {% name="Evboolean" %}
{  
    "k": "boolean",  
    "v": true  
}  
```

#### Fields
{% table %}
---
* **k**
* Kev
* Must be **boolean**.
---
* **v**
* boolean
* The value of the parameter to pass to the target
{% /table %}


### Evstring
A parameter of type **string**.
```json {% name="Evstring" %}
{  
    "k": "string",  
    "v": "Hello World!"  
}  
```

#### Fields
{% table %}
---
* **k**
* Kev
* Must be **string**.
---
* **v**
* string
* The value of the parameter to pass to the target
{% /table %}


### Evchar
A parameter of type **char**.
```json {% name="Evchar" %}
{  

    "k": "char",  
    "v": "H"  
}  
```

#### Fields
{% table %}
---
* **k**
* Kev
* Must be **char**.
---
* **v**
* char
* The value of the parameter to pass to the target
{% /table %}


### Evbytearray
A parameter of type **array of bytes**.
```json {% name="Evbytearray" %}
{  
    "k": "bytearray",  
    "v": [1, 2, …, 255]  
}  
```

#### Fields
{% table %}
---
* **k**
* Kev
* Must be **bytearray**.
---
* **v**
* list of byte
* The value of the parameter to pass to the target
{% /table %}


### Evshortarray
A parameter of type **array of shorts**.
```json {% name="Evshortarray" %}
{  

    "k": "shortarray",  
    "v": [-127, -126, …, 128]  
}  
```

#### Fields
{% table %}
---
* **k**
* Kev
* Must be **shortarray**.
---
* **v**
* list of short
* The value of the parameter to pass to the target
{% /table %}


### Evintarray
A parameter of type **array of integers**.
```json {% name="Evintarray" %}
{  
    "k": "intarray",  
    "v": [-2147483648, …, 2147483647]  
}  
```

#### Fields
{% table %}

---
* **k**
* Kev
* Must be **intarray**.
---
* **v**
* list of int
* The value of the parameter to pass to the target
{% /table %}


### Evlongarray
A parameter of type **array of longs**.
```json {% name="Evlongarray" %}
{  
    "k": "longarray",  
    "v": [-9223372036854775808, …, 9223372036854775807]  
}  
```

#### Fields
{% table %}
---
* **k**
* Kev
* Must be **longarray**.
---
* **v**
* list of long
* The value of the parameter to pass to the target
{% /table %}


### Evfloatarray
A parameter of type **array of floats**.
```json {% name="Evfloatarray" %}
{  
    "k": "floatarray",  
    "v": [-3.14, …, 3.14]  
}  
```

#### Fields
{% table %}
---
* **k**
* Kev
* Must be **floatarray**.
---
* **v**
* list of float
* The value of the parameter to pass to the target
{% /table %}


### Evdoublearray
A parameter of type **array of doubles**.
```json {% name="Evdoublearray" %}
{  
    "k": "doublearray",  
    "v": [-3.14, …, 3.14]  
}  
```

#### Fields
{% table %}
---
* **k**
* Kev
* Must be **doublearray**.
---
* **v**
* list of double
* The value of the parameter to pass to the target
{% /table %}


### Evbooleanarray
A parameter of type **array of booleans**.
```json {% name="Evbooleanarray" %}
{  

    "k": "booleanarray",  
    "v": [false, true, …, true]  
}  
```

#### Fields
{% table %}
---
* **k**
* Kev
* Must be **booleanarray**.
---
* **v**
* list of boolean
* The value of the parameter to pass to the target
{% /table %}


### Evstringarray
A parameter of type **array of strings**.
```json {% name="Evstringarray" %}
{  
    "k": "stringarray",  
    "v": ["Hello", "World", "!"]  
}  
```

#### Fields
{% table %}
---
* **k**
* Kev
* Must be **stringarray**.
---
* **v**
* list of string
* The value of the parameter to pass to the target
{% /table %}


### Evchararray
A parameter of type **array of chars**.
```json {% name="Evchararray" %}
{  
    "k": "chararray",  
    "v": ["H", "e", "l", …, "!"]  
}  

```

#### Fields
{% table %}
---
* **k**
* Kev
* Must be **chararray**.
---
* **v**
* list of char
* The value of the parameter to pass to the target
{% /table %}


### Evintegerarraylist
A parameter of type **arraylist of integers**.
```json {% name="Evintegerarraylist" %}
{  
    "k": "integerarraylist",  
    "v": [-2147483648, …, 2147483647]  
}  
```

#### Fields
{% table %}
---
* **k**
* Kev
* Must be **integerarraylist**.
---
* **v**
* list of int
* The value of the parameter to pass to the target
{% /table %}


### Evstringarraylist
A parameter of type **arraylist of strings**.
```json {% name="Evstringarraylist" %}
{  
    "k": "stringarraylist",  
    "v": ["Hello", "World", "!"]  
}  
```

#### Fields
{% table %}
---
* **k**
* Kev
* Must be **stringarraylist**.
---
* **v**
* list of string
* The value of the parameter to pass to the target
{% /table %}





### Evjson

A parameter of type string containing arbitrary json-formatted data.

```json {% name="Evjson" %}
{  

    "k": "json",  
    "v": {  
        "Hello": "World"  
    }  
}  
```

#### Fields
{% table %}

---
* **k**
* Kev
* Must be **json**.
---
* **v**
* json
* Arbitrary json content. Will be passed as a string parameter holding the content of this field.
{% /table %}

The above sample is a syntax sugar for the following

```json {% name="Evjson (desugared)" %}
{  

    "k": "string",  
    "v": "{\"Hello\":\"World\"}"  
 }  
```




## Widget position (Ptd)

Specifies position of the top-left corner of the widget on the screen.

```json {% name="Ptd" %}
{  
    "x": 4,  
    "y": 7.5  
}  
```

### Fields
{% table %}
---
* **x**
* number
* The horizontal position of the widget in [layout coordinate units](?p=/custom-menu-schemas#layout-coordinates).
---
* **y**
* number
* The vertical position of the widget in [layout coordinate units](?p=/custom-menu-schemas#layout-coordinates).
{% /table %}




### Layout coordinates

Widgets are laid out on the screen using a density-independent coordinate system.

The **x** axis of the coordinate system is divided into 9 equi-distant units. This axis always corresponds to the shorter edge of the screen, hence it is horizontal in portrait orientation and vertical in landscape orientation.

The **y** axis of the coordinate system is divided into 16 equi-distant units. This axis always corresponds to the longer edge of the screen, hence it is vertical in portrait orientation and horizontal in landscape orientation.

When changing orientation, the coordinate system is mirrored around the **x=y** line, or in other words it is mirrored along the vertical axis and then rotated by 90 degrees counterclockwise.

The **(x: 0, y: 0)** coordinate points to the top-left corner of the screen, whereas the **(x: 9, y: 16)** coordinate points to the bottom-right corner of the screen. The coordinate system maintains aspect ratio and thus a rectangle of **(dx: 1, dy: 1)** represents a square on the screen.

In portrait orientation **(x: 9, y: 0)** points to the top-right corner of the screen whereas in landscape orientation it points to the bottom-left corner of the screen. Similarly, **(x: 0, y: 16)** points to the bottom-left corner of the screen in portrait orientation and points to the top-right corner of the screen in landscape orientation.


## Widget size (Szd)

Specifies size of the widget on the screen.

```json {% name="Szd" %}
{  

    "dx": 4,  
    "dy": 7.5  
}  
```

### Fields
{% table %}
---
* **dx**
* number
* The horizontal size of the widget in [layout coordinate units](?p=/custom-menu-schemas#layout-coordinates).
---
* **dy**
* number
* The vertical size of the widget in [layout coordinate units](?p=/custom-menu-schemas#layout-coordinates).
{% /table %}




## Widget style (Busty)

Specifies visual appearance of the widget.

```json {% name="Busty" %}
{
    "ost18Text": {
        "_": "Messages",
        "hu": "Üzenetek",
        "de": "Mitteilungen",
        "it": "Messaggi",
    },
    "okico": "Material.Filled.Messages",
    "ocolBg": "20d0d0d0",
    "ocolFg": "ffd0d0d0",
    "okrund": {
        "k": "relative",
        "percent": "50",
    }
}
```

### Fields
{% table %}
---
* **ost18Text**
* [St18](?p=/custom-menu-schemas#internationalized-text-st18) (optional)
* The internationalized label of the widget. If omitted, the widget will have no label, or for system widgets, will have a default label provided by the app.
---
* **okico**
* [Kico](?p=/custom-menu-schemas#icon-kico) (optional)
* The icon of the widget. If omitted, the widget will have no icon, or for system widgets, will have a default icon provided by the app.
---
* **ocolBg**
* [Color](?p=/custom-menu-schemas#color) (optional)
* The background color of the widget. If omitted, the background will be semi-transparent blue, except for system widgets, which will provide a background color based on their contents.
---
* **ocolFg**
* [Color](?p=/custom-menu-schemas#color) (optional)
* The foreground color of the widget. If omitted, the foreground will be opaque white.
---
* **okrund**
* [Krund](?p=/custom-menu-schemas#corner-radius-krund) (optional)
* The corner radius of the widget. If omitted, the widget will have a circular shape.
{% /table %}



## Internationalized text (St18)

Specifies an internationalized text.

```json {% name="St18" %}
{  
    "_": "Messages",  
    "locale-1": "Üzenetek",  
    "locale-2": "Mitteilungen",  
    …,  
    "locale-N": "Messaggi",  
}  

 or  

 "Messages"  
```

### Fields
{% table %}

---
* **_**
* string
* The translation of the text in the default locale. This translation will be used if no other translation is provided that matches the language preferences of the driver.
---
* **locale**
* string (optional)
* The translation of the text in the given locale. Locale can be any **IETF BCP 47** or **ISO 639** language tag.
{% /table %}

If multiple translations are provided, the app will choose the translation that corresponds to the preferred language set by the driver on the mobile device. If multiple languages match the preferred languages, the driver’s most preferred locale will be picked. If no translations match the driver's preferred language, the translation in the default locale will be picked. The translation in the default locale is required.

If internationalization is not necessary, it is also possible to provide the translation in the default locale using the following shorthand syntax:

```json {% name="Busty" %}
{  
    "ost18Text": "Messages",  
    …  
}  
```

The above syntax is equivalent with the following expanded form

```json {% name="Busty" %}
{  
    "ost18Text": {  
        "_": "Messages"  
    },  
    …  
}  
```


## Icon (Kico)

Specifies an icon from the [Google Material Icons library](https://fonts.google.com/icons).

```json {% name="Kico" %}
"Library.Style.Icon"  
```

### Fields
{% table %}
---
* **Library**
* string
* The name of the icon library containing the icon. Presently only **Material** is supported.
---
* **Style**
* string
* The style of the icon. Possible values: **Outlined**, **Filled**, **Rounded**, **Sharp**, **TwoTone**.
---
* **Icon**
* string
* The pascal-case name of the icon as specified by the [Google Material Icons library](https://fonts.google.com/icons).
{% /table %}



## Color
Specifies a color and transparency using the **IEC 61966-2.1:1999 SRGB** color space.

```json {% name="Kico" %}
"AARRGGBB"  
```

### Fields
{% table %}
---
* **AA**
* hex
* 2 hexadecimal digits specifying the alpha color channel (opacity).
---
* **RR**
* hex
* 2 hexadecimal digits specifying the red color channel.
---
* **GG**
* hex
* 2 hexadecimal digits specifying the green color channel.
---
* **BB**
* hex
* 2 hexadecimal digits specifying the blue color channel.
{% /table %}



## Corner radius (Krund)

Specifies the corner radius of a widget.

```json {% name="Krund" %}
    {"k": "fixed", "dp": 4.5}

     or 

    {"k": "relative", "percent": 50} 

     or 

    {"k": "unit", "u": 0.1}  
```

### Fields
{% table %}
---
* **k**
* string
* Measurement unit type. Must be **fixed**, **relative** or **percent**.
---
* **dp**
* number
* Corner radius in Android Device Independent Pixels.
  See also: [android: Dp](https://developer.android.com/training/multiscreen/screendensities#dips-pels)
---
* **percent**
* integer
* Corner radius given as a percentage of widget size. **50%** means a circular shape.
---
* **u**
* number
* Corner radius given in [layout coordinate units](?p=/custom-menu-schemas#layout-coordinates)
{% /table %}



## Custom notification (Notd)

Defines a customizable notification and/or badge to be shown to the user of the app when the screen is updated on the mobile device.

A notification is shown with the text defined in the **ost18Message** field, if the field is present. A badge is displayed on the button with the number specified in the **ocBadge** field, if the field is present. If another notification/badge having the same **notid** value is already being displayed, their text/value is updated by this notification.

Pressing the button or the notification will perform the usual action as defined in the **[buta](?p=/custom-menu-schemas#widget-action-buta)** field of the **[mbut](?p=/custom-menu-schemas#widget-mbut)** structure and discard the notification/badge. Once the user marked a notification/badge as read, future notifications with the same **notid** will not be displayed.

```json {% name="Notd" %}
{  

    "notid": "2023/03/23",  
    "ost18Message": {  
        "_": "You have received new data",  
        "hu": "Önnek új adata érkezett."  
    },  

    "ocBadge": 1  
}  
```

### Fields
{% table %}
---
* **notid**
* string
* Client-generated unique identifier of the notification. Must be unique within the entire [scren](?p=/custom-menu-schemas#custom-menu-scren).
---
* **ost18Message**
* st18 (optional)
* The internationalized text of the notification message. If omitted, a notification will not be shown.
---
* **ocBadge**
* integer (optional)

* The number that should be displayed by the badge on the button. If omitted, no badge will be displayed.
{% /table %}


