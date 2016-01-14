---
title: API Reference

toc_footers:
  - <a href='https://www.anyline.io/download/'>Get the Anyline&reg SDK</a>


search: true

includes:
- gettingStartedLicense
- gettingStartedAndroid
- gettingStartedIOS
- gettingStartedCordova
- xamarinSetupGuide
- configuration
- modules
- advancedAndroidSetup


---

<!-- Content is split up in multiple files listed above, which can be found in the includes directory -->

# Introduction

Anyline provides an easy-to-use SDK for applications to enable Optical Character Recognition (OCR) on mobile devices.
This API contains a [Quick Start Guide] (#getting-started) for all supported platforms, a detailed description of the [Configuration] (#configuration), as well as descriptions and examples for all available [Modules] (#modules).<br/>


### Supported Platforms
- Android
- iOS
- WP (expected release: Q1 2016)

Anyline is also available as [Cordova Plugin] (#cordova-plugin).
Android and iOS modules are also available for [Xamarin] (#xamarin-setup-guide).

(Xamarin WP Support Q1 2016).

Furthermore there is a version of the Anyline SDK specific for the <a href='https://tech.moverio.epson.com/en/work/'>Epson BT2000</a>. This verison includes one additional module requiring a special license which can not be acquired via our customer portal. For more information or a trial license please <a href="https://www.anyline.io/support-request/">contact</a> us.

### Available Modules
- [**Barcode:**] (#barcodeModule)  Scan 23 types of international barcode & QR code formats.
- [**Energy:**] (#energyModule) Scan meter readings of various electric, gas, and water meters.
- [**MRZ:**] (#mrzModule)  Reliable scanning of data from passports' and IDs' machine readable zones (MRZ)
- [**Order Code:**] (#ordercodeModule)  Scan an alphanumeric order code - only available for Epson BT2000
- **Custom:** Got any other ideas? We will support you when implementing other use cases for mobile OCR technology.
