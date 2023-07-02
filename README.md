# QR Coder

App to generate a QR Code locally.

<img src="https://github.com/olibu/qrcoder/blob/main/qrcoder.png" width="200px">

There are a lot of QR Code scanners available and a view generators, too.
But I didn't found one which takes care about privacy. All send the data to a backend
to generate the QR Code. 

So here is one which generated the QR Code inside of your browser (even when offline).

# Usage

You can start the application from my [github homepage](https://olibu.github.io/qrcoder/).
In case you want to be able to use the app offline, you can click on "Add to home screen"
in Safari.

Open the page in Safari on iOS or any modern browser on any other device and type in your text.
The QR Code will be generated automatically and can be shared by any other device with a QR Code scanner 
(e.g. by the camera app on iOS).

# QR Code Details

## Wifi

To provide a Wifi QR Code the following content has to be provided.

```
WIFI:T:<Authentification>;S:<SSID>;P:<PSK>;H:true;
WIFI:T:<Authentification>;S:<SSID>;P:<PSK>;;
```
* Authentification: WPA or WEP or nopass
* SSID: The SSID of the Wifi
* PSK: The password of the Wifi
* H: "H:true" if the Wifi is hidden (otherwise empty)

## Telefon

```
tel:222
```

## SMS

```
SMSTO:<mobile>:<message>
```

## Email

```
mailto:<email>?subject=<subject>&<message>
```

## VCard

```
BEGIN:VCARD
VERSION:3.0
N:<lastname>;<firstname>
FN:<firstname> <lastname>
TITLE:<title>
ORG:<org>
URL:<url>
EMAIL;TYPE=INTERNET:<email>
TEL;TYPE=voice,work,pref:<phone work>
TEL;TYPE=voice,home,pref:<phone private>
TEL;TYPE=voice,cell,pref:<phone mobile>
TEL;TYPE=fax,work,pref:<fax work>
TEL;TYPE=fax,home,pref:<fax home>
ADR:;;<street>;<city>;<state>;<zip>;<country>
END:VCARD
```

## MeCard

```
MECARD:N:<lastname>,<firstname>;NICKNAME:<nick>;TEL:<phone1>;TEL:<phone2>;TEL:<phone3>;EMAIL:<email>;BDAY:<yyyymmdd>;NOTE:<notes>;ADR:,,<street>,<city>,<state>,<zip>,<country>;;
```

## VEvent

```
BEGIN:VEVENT
SUMMARY:<event name>
LOCATION:<location>
DTSTART:YYYYMMDDTHHMMSS
DTEND:YYYYMMDDTHHMMSS
END:VEVENT
```

## Bitcoin

```
bitcoin:<receipient>?<amount>&<message>
bitcoincash:<receipient>?<amount>&<message>
ethereum:<receipient>?<amount>&<message>
litecoin:<receipient>?<amount>&<message>
dash:<receipient>?<amount>&<message>
```

# Todo:

* Further content types (e.g. wifi)
* Add raw scanner

# Releases

## 1.1.0

* Dedicated forms for several use cases

## 1.0.0

* Initial version with basic functionality
  
# Credits

To Ryan Day to provide the node package to gernerate the QR Code.

https://github.com/soldair/node-qrcode

## License

QRCoder is licensed under the [MIT License](https://tldrlegal.com/l/mit)