---
title: API Reference

language_tabs:
  - java: Android
  - objective_c: iOS

toc_footers:
  - <a href='https://anyline.io/anyline-sdk-modules/'>Get the Anyline&reg SDK</a>


search: true
---

# Introduction

Anyline provides an easy-to-use SDK for applications to enable Optical Character Recognition (OCR) on mobile devices. 
This API contains a [Quick Start Guide] (#getting-started) for all supported platforms, a detailed description for the [Anyline Config] (#anyline-config) as well as descriptions and examples for the available [modules] (#modules).

### Supported Platforms
- Android
- iOS
- WP (by Q3 2015)
	
### Available Modules
- **Barcode & QR Code:**  Scan 16 international barcodes & QR codes
- **Meter Reading:** Scan meter readings of various electric, gas, water or heat meters
- **Passport (MRZ):**  Reliable scanning of data from passports' machine readable zones (MRZ)
- **Custom:** We will support you when implementing other use cases for mobile OCR technology.

# Getting Started

## Android ##

The bundle contains the following parts: 



- **AnylineSDK:**      contains the anyline-android-release.aar
- **Documentation:**    contains java doc for the anyline-sdk
- **AnylineSDKExamples:**      contains a simple app where each module is implemented, it can be installed right away
- **LICENSE:** 			third party license agreements
- **README:**			containing a quick start - setup and module description


### Requirements

- Android device with SDK >= 15
- decent camera functionality (recommended: 720p and adequate auto focus)


### Quick Start Guide
> Add AnylineSDK to the dependencies in build.gradle

```plaintext
//root section of the file
repositories {
    flatDir {
        dirs 'libs'
    }
}
 
dependencies {
    compile(name:'AnyLineSDK', ext:'aar')
    //... your other dependencies
}
```

#### 1. Copy AnylineSDK.aar to your apps "libs" directory and modify the build.gradle as shown in the example.

###### &NewLine;  

#### 2. Provide a config file (json or xml)

> Example barcode_view_config.json:

```json
{
	"captureResolution":"720p",
  	"cutout": {
		"style": "rect",
		"maxWidthPercent": "80%",
		"alignment": "center",
		"ratioFromSize": {
	     	"width": 100,
	     	"height": 80
		},
		"strokeWidth": 2,
		"cornerRadius": 4,
		"strokeColor": "FFFFFF",
		"outerColor": "000000",
		"outerAlpha": 0.3
	 },
	 "flash": {
		"mode": "manual",
	 	"alignment": "bottom_right"
	 },
	 "beepOnResult": true,
	 "vibrateOnResult": true,
	 "blinkAnimationOnResult": true,
	 "cancelOnResult": true
}
```

The config file enables a quick and easy adaption of the "Scan-View". You can either provide a json-file or use the XML-attributes directly in the layout file.
The detailed description of all available attributes can be found in [Anyline Config] (#anyline-config)

##### JSON

This config file must be located in the "assets" folder of your Android Project.

Some of the most important config options may be:

Paramter | Description 
-------- | -----------
captureResolution | the preferred camera preview size
cutout | defining which area of the preview will be "cutout" (analysed to find bar/qr code)
flash | defines the flash mode, where to place the flash symbol etc.
beepOnResult | enables sound on successful scanning process
vibrateOnResult | provides haptic feedback for a successful scanning process
blinkOnResult |  visual feedback for a successful scanning process
cancelOnResult | if true, the scanning process will be stopped after one result and needs to be started manually again

###### XML
> Example activity_layout.xml

```xml
<RelativeLayout
	 xmlns:android="http://schemas.android.com/apk/res/android"
	 xmlns:app="http://schemas.android.com/apk/res-auto"
	 android:layout_width="match_parent"
	 android:layout_height="match_parent">
	<at.nineyards.anyline.modules.energy.EnergyScanView
	    android:id="@+id/energy_scan_view"
	    android:layout_width="match_parent"
	    android:layout_height="match_parent"
	    app:cutout_alignment="top"
	    app:cutout_style="rect"
	    app:cutout_outside_color="#55000000"
	    app:cutout_offset_y="120"
	    app:cutout_rect_corner_radius_in_dp="4"
	    app:cutout_stroke_width_in_dp="2"
	    app:cutout_stroke_color="#FFFFFF"
	    app:flash_mode="manual"
	    app:flash_alignment="bottom_right"
	    app:beep_on_result="true"
	    app:vibrate_on_result="true"
	    app:blink_animation_on_result="true"
	    app:cancel_on_result="true"
	 />
</RelativeLayout>
```

Alternatively you may also configure the view (EnergyScanView, MrzScanView, BarcodeScanView) using XML-attributes in the layout-file.

###### &NewLine;  

####3.Init Anyline in your Activity
There are module specific options - take a look at the description in the desired [module] (#modules) description.

####4. Enjoy scanning and have fun :)


##iOS##

- **Framework:**        contains the Anyline.framework & AnylineResources.bundle
- **Documentation:**    contains a html & docset version of an appledoc style interface documentation
- **AnylineExamples:**  contains a simple app where each module is implemented, it can be installed right away
- **LICENSE:** 			third party license agreements
- **README:**			containing a quick start - setup and module description
- **RELEASE_NOTES**


### Requirements

- minimum iOS 7.0
- minimum iPhone4s


### Quick Start - Setup

#### 1. Import the files

Simply drag & drop Anyline.framework & AnylineResources.bundle into your project tree.
![Add Frameworks](images/AddFramework.jpg)


In the import screen select "Copy items if needed" and "Create groups" and add the files to your target.
![Copy Frameworks](images/CopyFramework.jpg)

#### 2. Linking Frameworks

After the framework and bundle got imported go to your project inspector. Under the "Build Phases" tab add the following libraries:

- libc++.dylib
- libstdc++.6.0.9.dylib
- libiconv.dylib
- AssetsLibrary.framework	

After you have added them it should look like this (notice the AnylineResources bundle. If its not in "Copy Bundle Resources" add it):
![Link Frameworks](images/LinkFrameworks.jpg)

#### 3. Linker Flags 

In your project inspector switch to the "Build Settings" tab  and search for "Other Linker Flags". Under Other - "Other Linker Flags" add "-ObjC". 
This flag causes the linker to load every object file in the library that defines an Objective-C class or category.

![Linker Flags](images/LinkerFlags.jpg)


####4. Init an AnylineModuleView in your ViewController or Storyboard

There are module specific options - take a look at the description in the desired [module] (#modules) description.



####5. Enjoy scanning and have fun :)


# Anyline Config

With the config file the views can be configured in regard of postion and looks of the cutout as well as flash behaviour, feedback on scan, etc. 

## Item description

### captureResolution

The Resolution of the preview frame.

**Possible values:**

value | description
----- | -----------
1080p |	use 1920x1080 as preferred preview size
720p  | use 1280x720 as preferred preview size
480p  |	use 480x854 as preferred preview size


 - The p is optional. 
 - The defined capture resolution is just the preferred resolution. If this resolution is not available the resolution that best fits inside the desired resolution should be used.
 
 
###cutout

This contains all the settings for the overlay/cutout.



#### width 

Specify an exact desired pixel width (Pixels in the frame, not on the Display). If this is bigger than the views width, it will be limited to it. E.g. Android devices may have a 720p preview but only a 540p wide display. If a width of 600 is specified, the cutout will still only be 540.

- **Type:** 		int
- **Default:** 		none

#### maxWidthPercent

Specify the maximum width of the cutout in percent.
This is the desired width if 'width' parameter is not used.

- **Type:**			String
- **Format:**		###%
- **Range:**		1% - 100%
- **Default:**		100%

####maxHeightPercent
 
Specify the maximum height of the cutout in %.

- **Type:**			String
- **Format:**		###%
- **Range:**		1% - 100%
- **Default:**		100%

**_Why width and maxWidth and maxHeight but no hight?_**

The form of the cutout can be defined by the ratio, so there is no direct need for width AND height parameter.
But if there is only one width parameter where % or pixel value is allowed, it is not possible to limit the height to a certain percentage.
That limits the usability in landscape mode quite a bit.

####ratioFromSize
The ratio of the cutout can be defined by just setting this parameters width and height to the size of the real object that should be
scanned.

E.g. width = 100 height = 50 is the same as width = 200 height = 100, because only the ratio matters here.

- **Type of width and height:** int
- **Default ratio:** 1 (width = 1, height = 1)
	
####alignment
The alignment of the cutout.

**Possible values:**

value | description
----- | -----------
top | upper border of the cutout is at the upper border of the view
top_half | offsetY = (viewHeight - cutoutHeight) / 3
center | cutout is centered in the view vertically and horizontally
bottom | lower border of the cutout is at the lower border of the view
bottom_half | the same as tophalf just from below: offsetY = (viewHeight - cutoutHeight) / 3 * 2


```json
{
      "captureResolution": "720p",
      "cutout": {
          "style": "rect",
          "width": 540,
          "maxWidthPercent": "80%",
          "maxHeightPercent": "80%",
          "alignment": "center",
          "image":"Overlay",
          "ratioFromSize": {
              "width": 125,
              "height": 85
          },
          "strokeWidth": 2,
          "strokeColor": "FFFFFF",
          "cornerRadius": 4,
          "offset": {
              "x": 0,
              "y": 0
          },
          "cropPadding": {
              "x": 0,
              "y": 0
          },
          "cropOffset": {
              "x": 0,
              "y": 0
          },
          "outerColor": "000000",
          "outerAlpha": 0.3
      },
      "flash": {
          "mode": "manual",
          "alignment": "bottom_right",
          "imageOn": "ic_flash_on",
          "imageOff": "ic_flash_off",
          "imageAuto": "ic_flash_auto"
      },
      "beepOnResult": false,
      "vibrateOnResult": true,
      "blinkAnimationOnResult": true,
      "cancelOnResult": true
}
```


# Modules

The Anyline-Modules are use-case specific abstractions for Anyline to scan specific stuff.

## Barcode 

With the Anyline-Barcode-Module 16 different kinds of bar- and qr-codes can be scanned. The result will simply be a *String* representation of the code.

#### Available Barcode Formats:  
            
- AZTEC
- CODABAR
- CODE_39
- CODE_93
- CODE_128
- DATA_MATRIX
- EAN_8
- EAN_13
- ITF
- PDF_417
- QR_CODE
- RSS_14
- RSS_EXPANDED
- UPC_A
- UPC_E
- UPC_EAN_EXTENSION

#### Example config for the Barcode Module
The config file enables a quick and easy adaption of the "Scan-View".


Some of the most important config options may be:

Parameter | Description
--------- | -----------
captureResolution | the preferred camera preview size
cutout | defining which area of the preview will be "cutout" (analysed to find bar/qr code)
flash | defines the flash mode, where to place the flash symbol etc.
beepOnResult | enables sound
vibrateOnResult | haptic feedback for a succesful scanning process
blinkOnResult |visual feedback for a successful scanning process
cancelOnResult | if true, the scanning process will be stopped after one result and needs to be started manually again

### Android






### iOS

### Restrictions for the Barcode-Module Config:
- Flash mode "auto" is not (yet) supported.

### Init Anyline:

- Init an AnylineBarcodeModuleView View with frame or add the View to your Storyboard.
- Setup the AnylineBarcodeModuleView with a valid license key and delegate
- Optional you may also limit the barcode scanning to one or multiple barcode formats (see also 'Available Barcode Formats' below)
- Call startScanningAndReturnError: at viewDidAppear: or later.




```java
barcodeScanView = (BarcodeScanView) findViewById(R.id.barcode_scan_view);
barcodeScanView.setConfigFromAsset("barcode_view_config.json");
 
// initialize Anyline with your license key and a Listener that is called if a result is found
barcodeScanView.initAnyline(ANYLINE_LICENSE, new BarcodeResultListener() {
    @Override
    public void onResult(String result) {
        // This is called when a result is found.
 
    }
});
barcodeScanView.startScanning();
```



```objective_c
    - (void)viewDidLoad {
        self.barcodeModule = [[AnylineBarcodeModuleView alloc] initWithFrame:CGRectMake(0, 0, 640, 640)];
        [self.view addSubview:self.barcodeModule];
        BOOL success = [self.barcodeModule setupWithLicenseKey:kMyLicenseKey delegate:self error:&error];
        if( !success ) {
            // Handle the error here
        }
        [self.barcodeModule setBarcodeFormats:@[kCodeTypeEAN8, kCodeTypeEAN13, kCodeTypeQR]]
    }

    - (void)viewDidAppear:(BOOL)animated {
        [super viewDidAppear:animated];
        NSError *error;
        BOOL success = [self.barcodeModule startScanningAndReturnError:&error];
        NSAssert(success, @"Start failed: %@",error.debugDescription);
        if( !success ) {
            // Handle the error here
        }
    }
    
    - (void)anylineBarcodeModuleView:(AnylineBarcodeModuleView *)anylineBarcodeModuleView
                   didFindScanResult:(NSString *)scanResult
                             atImage:(UIImage *)image {
        NSLog("Scan result: %@", scanResult);
    }
```
   
    






## Energy 

The Anyline-Energy-Module is capable of scanning analog electric- and gas-meter-readings.
Moreover, it is possible to scan bar- and qr-codes for meter identification.

For each successful scan, you will receive four result-attributes (images will be null for bar/qr code mode):
    scanMode: the mode the result belongs to
    result (for meter reading): the detected value as a String
    resultImage (for meter reading): the cropped image that has been used to scan the meter value
    fullImage (for meter reading): the full image (before cropping)


Restrictions for the Energy-Module Config:
- Flash mode "auto" is not (yet) supported.
    
    
Init Anyline:
- Initialize the module in viewDidLoad
- Start the scanning process in viewDidAppear
- Implement the delegate method and receive results
    

```objective_c
    - (void)viewDidLoad {
        self.energyModule = [[AnylineEnergyModuleView alloc] initWithFrame:CGRectMake(0, 0, 640, 640)];
        [self.view addSubview:self.energyModule];
        BOOL success = [self.energyModule setupWithLicenseKey:kMyLicenseKey delegate:self error:&error];
        if( !success ) {
            // Handle the error here
        }
        [self.energyModule setScanMode:ALElectricMeter];
    }
    - (void)viewDidAppear:(BOOL)animated {
        [super viewDidAppear:animated];
        NSError *error;
        BOOL success = [self.energyModule startScanningAndReturnError:&error];
        NSAssert(success, @"Start failed: %@",error.debugDescription);
        if( !success ) {
            // Handle the error here
        }
    }
    - (void)anylineEnergyModuleView:(AnylineEnergyModuleView *)anylineEnergyModuleView
                  didFindScanResult:(NSString *)scanResult
                          cropImage:(UIImage *)image
                          fullImage:(UIImage *)fullImage
                             inMode:(ALScanMode)scanMode; {
        NSLog("Scan result: %@", scanResult);
    }
```

## MRZ 

The Anyline-MRZ-Module provides the functionality to scan passports and other IDs with a MRZ (machine-readable-zone).
For each scan result the module generates an Identification Object, containing all relevant information 
(e.g. document type and number, name, day of birth, etc.) as well as the image of the scanned document.



Restrictions for the MRZ-Module Config:
- Flash mode "auto" is not (yet) supported.


Init Anyline:
- Initialize the module in viewDidLoad
- Start the scanning process in viewDidAppear
- Implement the delegate method and receive results


```objective_c
    - (void)viewDidLoad {
        self.mrzModule = [[AnylineMRZModuleView alloc] initWithFrame:CGRectMake(0, 0, 640, 640)];
        [self.view addSubview:self.mrzModule];
        BOOL success = [self.mrzModule setupWithLicenseKey:kMyLicenseKey delegate:self error:&error];
        if( !success ) {
            // Handle the error here
        }
    }
    - (void)viewDidAppear:(BOOL)animated {
        [super viewDidAppear:animated];
        NSError *error;
        BOOL success = [self.mrzModule startScanningAndReturnError:&error];
        NSAssert(success, @"Start failed: %@",error.debugDescription);
        if( !success ) {
            // Handle the error here
        }
    }               
    - (void)anylineMRZModuleView:(AnylineMRZModuleView *)anylineMRZModuleView
               didFindScanResult:(ALIdentification *)scanResult
                         atImage:(UIImage *)image; {
        NSLog("Scan result: %@", scanResult);
    }    
```