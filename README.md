# QR Code Scanner (fork)
[![GH Actions](https://github.com/juliuscanute/qr_code_scanner/workflows/dart/badge.svg)](https://github.com/juliuscanute/qr_code_scanner/actions)

A QR code scanner that works on both iOS and Android by natively embedding the platform view within Flutter. The integration with Flutter is seamless, much better than jumping into a native Activity or a ViewController to perform the scan.

## Get Scanned QR Code
When a QR code is recognized, the Barcode object will be received.
For Android the object contains a full set of props, for iOS - only scanned text.

```dart
class _QRViewExampleState extends State<QRViewExample> {
  final GlobalKey qrKey = GlobalKey(debugLabel: 'QR');
  Barcode barcode;
  QRViewController controller;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Column(
        children: <Widget>[
          Expanded(
            flex: 5,
            child: QRView(
              key: qrKey,
              onQRViewCreated: _onQRViewCreated,
            ),
          ),
          Expanded(
            flex: 1,
            child: Center(
              child: Text('Scan result: ${barcode?.code}'),
            ),
          )
        ],
      ),
    );
  }

  void _onQRViewCreated(QRViewController controller) {
    this.controller = controller;
    controller.scannedDataStream.listen((scanData) {
      setState(() {
        barcode = scanData;
      });
    });
  }

  @override
  void dispose() {
    controller?.dispose();
    super.dispose();
  }
}
```

## iOS Integration
In order to use this plugin, add the following to your Info.plist file:
```
<key>io.flutter.embedded_views_preview</key>
<true/>
<key>NSCameraUsageDescription</key>
<string>This app needs camera access to scan QR codes</string>
```

## Flip Camera (Back/Front)
The default camera is the back camera.
```dart
controller.flipCamera();
```

## Flash (Off/On)
By default, flash is OFF.
```dart
controller.toggleFlash();
```

## Resume/Pause
Pause camera stream and scanner.
```dart
controller.pause();
```
Resume camera stream and scanner.
```dart
controller.resume();
```


# SDK
Requires at least SDK 24 (Android 7.0).

# Credits
* Forked from: https://github.com/juliuscanute/qr_code_scanner