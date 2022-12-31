# QR Coder

App to generate a QR Code locally.

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
WIFI:T:<Authentifikation>;S:<SSID>;P:<PSK>:H:<HIDDEN>;
```
* Authentification: WPA or WEP or nopass
* SSID: The SSID of the Wifi
* PSK: The password of the Wifi
* HIDDEN: "HIDDEN" if the Wifi is hidden (otherwise empty)

# Todo:

* Further content types (e.g. wifi)
* Add scanner

# Releases

## 1.0.0

* Initial version with basic functionality
  
# Credits

To Ryan Day to provide the node package to gernerate the QR Code.

https://github.com/soldair/node-qrcode

## License

QRCoder is licensed under the [MIT License](https://tldrlegal.com/l/mit)