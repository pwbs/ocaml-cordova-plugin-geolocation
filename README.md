# ocaml-cordova-plugin-geolocation

Binding to
[cordova-plugin-geolocation](https://github.com/apache/cordova-plugin-geolocation)

[Example
application](https://github.com/dannywillems/ocaml-cordova-plugin-geolocation-example).

## What does cordova-plugin-geolocation do ?

```
This plugin provides information about the device's location, such as latitude
and longitude. Common sources of location information include Global Positioning
System (GPS) and location inferred from network signals such as IP address,
RFID, WiFi and Bluetooth MAC addresses, and GSM/CDMA cell IDs. There is no
guarantee that the API returns the device's actual location.

This API is based on the W3C Geolocation API Specification, and only executes on
devices that don't already provide an implementation.
```

Source: [cordova-plugin-geolocation](https://github.com/apache/cordova-plugin-geolocation)

## Repository branches and tags

We are migrating bindings from
[js_of_ocaml](https://github.com/ocsigen/js_of_ocaml) (low level bindings) to
[gen_js_api](https://github.com/lexifi/gen_js_api) (high level bindings).

The gen_js_api binding allows to use *pure* ocaml types (you don't have to use
the ## syntax from js_of_ocaml or Js.string type but only # and string type).

The js_of_ocaml version is available in the branch
[*js_of_ocaml*](https://github.com/dannywillems/ocaml-cordova-plugin-geolocation/tree/js_of_ocaml)
but we **recommend** to use the gen_js_api version which is the master branch.

## How to use ?

* TODO
* The coordinates type provides each properties as 'a Js.opt type because, when we
did some tests, some values were set to null. See the example application for
more informations.


## ! BE CAREFUL !

The plugin creates a new object called *navigator.geolocation*, but the object is
available when the *deviceready* event is handled.

We don't provide a *navigator.geolocation* variable in this plugin (as said in the official
documentation on js_of_ocaml). If we did, *navigator.geolocation* will be set to **undefined**
because the *navigator.geolocation* object doesn't exist when we create the variable.

Instead, we provide a function *Geolocation.t* of type *unit -> geolocation* which creates the
binding to the *navigator.geolocation* object. You must call it when the deviceready
event is handled, eg

```OCaml
let on_device_ready _ =
  let geolocation = GeolocationCordova.t () in
  (* Some code *)

let _ =
  Dom.addEventListener Dom_html.document (Dom.Event.make "deviceready")
  (Dom_html.handler on_device_ready) Js._false
```
