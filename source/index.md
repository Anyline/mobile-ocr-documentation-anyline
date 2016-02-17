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

### Extensions
- [Cordova Plugin] (#cordova-plugin)
- [Xamarin] (#xamarin-setup-guide) - Availabe for Android and iOS. Expected WP Support Q1 2016.

### Smart Glasses

- <a href=https://developers.google.com/glass/>Google Glass</a>
- <a href='https://tech.moverio.epson.com/en/work/'>Epson BT2000</a> 

First prototypes for both glasses are available on request. The versions include one additional module requiring a special license which cannot be acquired via our customer portal. For more information and a trial license please <a href="https://www.anyline.io/support-request/">contact</a> us.

### Available Modules
- [**Barcode:**] (#barcodeModule)  Scan 23 types of international barcode & QR code formats.
- [**Energy:**] (#energyModule) Scan meter readings of various electric, gas, and water meters.
- [**MRZ:**] (#mrzModule)  Reliable scanning of data from passports' and IDs' machine readable zones (MRZ)
- [**Document:**] (#documentModule) Detects document outlines and delivers a high-resolution image
- [**Order Code:**] (#ordercodeModule)  Scan an alphanumeric order code - only available for smart glasses
- **Custom:** Got any other ideas? We will support you when implementing other use cases for mobile OCR technology.
