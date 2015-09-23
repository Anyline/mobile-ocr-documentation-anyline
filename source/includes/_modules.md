# Modules

The Anyline-Modules are use-case specific abstractions for Anyline. Each module is designed to serve a specific purpose. Currently, the following modules are available:

- [Barcode] (#barcodeModule)
- [Energy] (#energyModule)
- [MRZ (Machine Readable Zone)] (#mrzModule)

<a name="barcodeModule"> </a>
## Barcode

With the Anyline Barcode-Module 16 different kinds of bar- and QR-codes can be scanned. The result will simply be a *string* representation of the code.

#### Restrictions for the Barcode-Module Config:
- Flash mode *auto* is still in alpha stage therefore *manual* mode is preferred

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

### Android

#### Example

The following example files illustrate a simple use-case of the barcode module.

###### Example  Activity
> in onCreate or onActivityCreated lifecycle methods

```java
barcodeScanView = (BarcodeScanView) findViewById(R.id.barcode_scan_view);
barcodeScanView.setConfigFromAsset("barcode_view_config.json");

// initialize Anyline with your license key and a Listener that is called if a result is found
barcodeScanView.initAnyline(getString(R.string.anyline_license_key), new BarcodeResultListener() {
    @Override
    public void onResult(String result, AnylineImage resultImage) {
        // This is called when a result is found.
    }
});
```
> in onResume()

```java
barcodeScanView.startScanning();
```

> in onPause()

```java
barcodeScanView.cancelScanning();
//IMPORTANT: always release the camera in onPause
barcodeScanView.releaseCameraInBackground();
```

There are four simple steps necessary to get started:

1. If you do not use XML configuration set the [config-file] (#barcodeConfig) to your BarcodeScanView using the *setConfigFromAsset* method and make sure that the json config-file is located in the Android assets folder.
2. Call initAnyline with your valid license key and a new instance of BarcodeResultListener, which can be used to handle the bar/QR code results.
3. Call *startScanning()*
4. When done call *cancelScanning()* and *releaseCameraInBackground()* or *releaseCamera()*

###### &NewLine;

```java
// limit the barcode scanner to QR codes or CODE_128 codes
barcodeScanView.setBarcodeFormats(BarcodeScanView.BarcodeFormat.QR_CODE, BarcodeScanView.BarcodeFormat.CODE_128);
```

In an optional step, you can limit the barcode scanning to one or multiple barcode formats with setBarcodeFormats(BarcodeScanView.BarcodeFormat...);

###### Example Activity Layout

```xml
<RelativeLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

 <at.nineyards.anyline.modules.barcode.BarcodeScanView
    android:id="@+id/barcode_scan_view"
    android:layout_width="match_parent"
    android:layout_height="match_parent"/>
</RelativeLayout>
```

The BarcodeScanView can simply be included in the activity layout file, just like any other view. The view can be either configured here via XML or otherwise a config json file can be used to adapt the scan view.

<a name="barcodeConfig"> </a>
###### Example config for the Barcode Module

```json
{
    "captureResolution": "720p",
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

The config file enables a quick and easy adaption of the scan view.

Some of the most important config options may be:

Parameter | Description
--------- | -----------
captureResolution | the preferred camera preview size
cutout | defining which area of the preview will be "cutout" (analyzed to find bar/QR code)
flash | defines the flash mode, where to place the flash symbol, etc.
beepOnResult | enables acoustic feedback on successful scan
vibrateOnResult | haptic feedback on successful scan
blinkOnResult |visual feedback on successful scan
cancelOnResult | true, if the scanning process should be stopped after one result; needs manual restart for additional scans

A detailed description of all available config items can be found in [Anyline Config] (#anyline-config)

It is also possible to use xml-attributes instead of the json config file. For more detailed information see [XML Configuration] (#configureViaXML)


### iOS

There are three steps necessary to get a scan result:

###### 1. Initialize the module in viewDidLoad

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
```

Create a property, initialize the module in the viewDidLoad method and add it to the view of the view controller. Supply the license key and set the delegate, which will receive a call once a result is found. The boolean returned by the setup routine notifies you if the Anyline set up was successful. If an error occurred the error needs to be handled. If the setup was successful, set the barcode types that should be scanned.

Valid types are:

- kCodeTypeAztec
- kCodeTypeCodabar
- kCodeTypeCode39
- kCodeTypeCode93
- kCodeTypeCode128,
- kCodeTypeDataMatrix
- kCodeTypeEAN8
- kCodeTypeEAN13
- kCodeTypeITF
- kCodeTypePDF417
- kCodeTypeQR,
- kCodeTypeRSS14
- kCodeTypeRSSExpanded
- kCodeTypeUPCA
- kCodeTypeUPCE
- kCodeTypeUPCEANExtension

###### 2. Start the scanning process in viewDidAppear

```objective_c
- (void)viewDidAppear:(BOOL)animated {
    [super viewDidAppear:animated];
    NSError *error;
    BOOL success = [self.barcodeModule startScanningAndReturnError:&error];
    NSAssert(success, @"Start failed: %@",error.debugDescription);
    if( !success ) {
        // Handle the error here
    }
}
```   
Once Anyline is set up successful, start the scanning process in viewDidAppear.
If there is a problem starting the scanning process an error object will be set, so the error can be handled.

###### 3. Implement the delegate method and receive results

```objective_c
- (void)anylineBarcodeModuleView:(AnylineBarcodeModuleView *)anylineBarcodeModuleView
               didFindScanResult:(NSString *)scanResult
                         atImage:(UIImage *)image {
    NSLog("Scan result: %@", scanResult);
}
```
When a valid result is found, it will call the delegate. The scan result will be a *string* containing the scanned code.

### Cordova Plugin

###### Example call for the Barcode Module
Call the exec method with <i><b>scanBarcode</b></i> (all other parameters are as explained in [Quick Start] (#cordova-example)).

```java
cordova.exec(onResult, onError, "AnylineSDK", "scanBarcode",
    [
        "YOUR_LICENSE_KEY",
        {
            "captureResolution": "720p",
            "cutout": {
                "style": "rect",
                "maxWidthPercent": "80%",
                "maxHeightPercent": "80%",
                "alignment": "center",
                "ratioFromSize": {
                    "width": 100,
                    "height": 80
                },
                "strokeWidth": 4,
                "cornerRadius": 10,
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
            "cancelOnResult": false
        }
    ]
);
```



<a name="energyModule"> </a>
## Energy

The Anyline Energy-Module is capable of scanning analog electric- and gas-meter-readings.
It is also possible to scan bar- and QR-codes (useful for identifying meters).

For each successful scan, you will receive four result-attributes:

- **scanMode:** the mode the result belongs to (gas, electric, barcode)
- **result**: the detected value as a String
- **resultImage**:
	 - scanMode = meter: the cropped image that has been used to scan the meter value
	 - scanMode = code: null
- **fullImage**:
	 - scanMode = meter: the full image (before cropping)
	 - scanMode = code: null


### Android

#### Restrictions for the Energy-Module Config
- Capture resolution is currently fixed to 720p on Android (optimized for good results on as many devices as possible).
- The size and ratio of the cutout is predefined and cannot be changed (sizes are optimized for best results)
- The cutout should be placed fairly high (use alignment top and a small y offset), because this reduces reflections considerably when used with flash.
- Flash mode *auto* is still in alpha stage therefore *manual* mode is preferred

#### Example
The following example files illustrate a simple use-case of the energy module.

###### Example  Activity
> in onCreate or onActivityCreated lifecycle methods

```java
energyScanView = (EnergyScanView) findViewById(R.id.energy_scan_view);

// set the scan mode to start with
energyScanView.setScanMode(EnergyScanView.ScanMode.ELECTRIC_METER);

// initialize Anyline with your license key and a Listener that is called if a result is found
energyScanView.initAnyline(getString(R.string.anyline_license_key), new EnergyResultListener() {
    @Override
    public void onResult(EnergyScanView.ScanMode scanMode, String result,
                         AnylineImage resultImage, AnylineImage fullImage) {

        // This is called when a result is found.
        // The scanMode is the mode the result was found for. The result is the actual result.
        // If the a meter reading was scanned two images are provided as well, one shows the targeted area only
        // the other shows the full image. (Images are null in barcode mode)
        // The result for meter readings is a String with leading zeros (if any) and no decimals.

    }
});
```
> in onResume()

```java
energyScanView.startScanning();
```

> in onPause()

```java
energyScanView.cancelScanning();
//IMPORTANT: always release the camera in onPause
energyScanView.releaseCameraInBackground();
```

In order to start the scan process, perform the following steps:

1. If you prefer a json-file for configuration, use the *setConfigFromAsset* method and place the json-config in the Android assets folder, otherwise configure the view using the xml attributes in the activity layout file.
2. Set the scan mode; available are: ELECTRIC_METER, GAS_METER, BAR_CODE
3. Call *initAnyline* with your valid license key and a new instance of EnergyResultListener, which is the callback for handling the results
4. Call *startScanning()*
5. When done call *cancelScanning()* and *releaseCameraInBackground()* or *releaseCamera()*


###### Example Activity Layout

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

The EnergyScanView can simply be included in the activity layout.

For custom configuration (e.g. cutout, flash, feedback on successful scan, etc.) you can either use a json-file or XML-attributes like in the example. If you need more detailed information about all available config items see [Anyline Config] (#anyline-config).

<aside class="notice">
Capture resolution is currently fixed to 720p on Android, which was optimized for good results on as many devices as possible.
</aside>

###### Reporting

The reporting of Analog Meter Results in the Community Edition (including an image of a scanned meter)
helps us to improving our product, and the customer experience.
It is possible to turn that feature off by calling *setReportingEnabled(false)* on the EnergyScanView.

```java
energyScanView.setReportingEnabled(false);
```


### iOS

#### Restrictions for the Energy-Module Config:
- Flash mode *auto* is still in alpha stage therefore *manual* mode is preferred

#### Example

In order to get scan results, it is necessary to perform the following three steps:

###### 1. Initialize the module in viewDidLoad

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
```

Create a property, initialize the module and add it to the view of our view controller.
Afterwards, supply the license key and set the delegate. The delegate will receive a call when a result is found.
If the Anyline set up returned an error the error object will be set and you can handle the error.
Furthermore it is necessary to set the scan mode utilizing *setScanMode*.

Possible values:

- ALElectricMeter
- ALGasMeter
- ALBarcode
- ALSerialNumber

###### 2. Start the scanning process in viewDidAppear

```objective_c
- (void)viewDidAppear:(BOOL)animated {
    [super viewDidAppear:animated];
    NSError *error;
    BOOL success = [self.energyModule startScanningAndReturnError:&error];
    NSAssert(success, @"Start failed: %@",error.debugDescription);
    if( !success ) {
        // Handle the error here
    }
}
```
If there is an error staring the scanning process the error object will be set and you can handle the error.

###### 3. Implement the delegate method and receive results


```objective_c
- (void)anylineEnergyModuleView:(AnylineEnergyModuleView *)anylineEnergyModuleView
              didFindScanResult:(NSString *)scanResult
                      cropImage:(UIImage *)image
                      fullImage:(UIImage *)fullImage
                         inMode:(ALScanMode)scanMode; {
    NSLog("Scan result: %@", scanResult);
}
```

Once Anyline has found a valid result the delegate is called and you get an result object with four result attributes.

- **scanMode:** the mode the result belongs to
- **scanResult**: the detected value as *string*
- **image**:
	 - scanMode = meter: the cropped image that has been used to scan the meter value
	 - scanMode = code: null
- **fullImage**:
	 - scanMode = meter: the full image (before cropping)
	 - scanMode = code: null

### Cordova Plugin

###### Example call for the Energy Module

There is a distinction between scanning electric and gas meter:
Call the exec method either with <i><b>scanElectricMeter</b></i> or <i><b>scanGasMeter</b></i> (all other parameters are as explained in [Quick Start] (#cordova-example)).

```java
cordova.exec(onResult, onError, "AnylineSDK", "scanElectricMeter",
    [
        "YOUR_LICENSE_KEY",
        {
            "captureResolution": "720p",
            "cutout": {
                "style": "rect",
                "alignment": "top",
                "offset": {
                    "x": 0,
                    "y": 120
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
    ]
);
```



<a name="mrzModule"> </a>
## MRZ

The Anyline MRZ-Module provides the functionality to scan passports and other IDs using the MRZ (Machine Readable Zone).

For each scan result the module generates an identification object, containing all relevant information as well as the image of the scanned document.

<a name="scannedInfo"> </a>
**Information scanned:**

value | description
----- | -----------
expirationDate | expiration date of the document
dob |	date of birth
checkDigitDob | check digit for the date of birth
checkDigitExpiration | check digit for the expiration date
code |	country code
surname	| surname
givenNames| all given first names
checkDigitDates	| check digit for both dates
type |	type of the document that was read. (ID/P)
checkDigitNumber |	check digit for the document number
checkDigitFinal	| check digit
sex	 | gender of the person

<aside class="notice">
Please be aware that not every property is filled for every document type (e.g. some ID MRZ do not contain a sex information)
</aside>

### Android

##### Restrictions for the MRZ-Module Config:
- The ratio of the cutout cannot be changed and is predefined to fit passports and IDs well.
- Flash mode *auto* is still in alpha stage therefore *manual* mode is preferred

#### Example
The following example files illustrate a simple use-case of the MRZ module.

###### Example Activity
> in onCreate or onActivityCreated lifecycle methods

```java
mrzResultView = (MrzScanView) findViewById(R.id.mrz_result);
mrzScanView.setConfigFromAsset("mrz_view_config.json");

// initialize Anyline with your license key and a Listener that is called if a result is found
mrzScanView.initAnyline(getString(R.string.anyline_license_key), new MrzResultListener() {

    @Override
    public void onResult(Identification mrzResult, AnylineImage anylineImage) {

        // This is called when a result is found.
        // The Identification includes all the data read from the MRZ
        // as scanned and the given image shows the scanned ID/Passport
    }
});
mrzScanView.startScanning();
```
> in onResume()

```java
mrzScanView.startScanning();
```

> in onPause()

```java
mrzScanView.cancelScanning();
//IMPORTANT: always release the camera in onPause
mrzScanView.releaseCameraInBackground();
```

There are four simple steps necessary to get started:

1. If you do not use XML configuration set the [config-file] (#mrzConfig) to your MrzScanView using the *setConfigFromAsset* method and make sure that the json config-file is located in the Android assets folder
2. Call *initAnyline* with your valid license key and a new instance of MrzResultListener, which will be used as callback for each successful scan.
3. Call *startScanning()*
4. When done call *cancelScanning()* and *releaseCameraInBackground()* or *releaseCamera()*

###### Example Activity Layout

```xml
<RelativeLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <at.nineyards.anyline.modules.mrz.MrzScanView
        android:id="@+id/mrz_view"
        android:layout_width="match_parent"
        android:layout_height="match_parent"/>
</RelativeLayout>
```

The MrzScanView can simply be included in the activity layout file, just like any other view. The view can be either configured via XML or with a config json.

<a name="mrzConfig"> </a>
###### Example config for the MRZ Module

```json
{
    "captureResolution": "1080p",
    "cutout": {
        "style": "rect",
        "maxWidthPercent": "90%",
        "maxHeightPercent": "90%",
        "alignment": "top_half",
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

A detailed description of all available config items can be found in [Anyline Config] (#anyline-config)

It is also possible to use xml-attributes instead of the json config file. For more detailed information see [XML Configuration] (#configureViaXML)



### iOS

##### Restrictions for the MRZ-Module Config:
- Flash mode *auto* is still in alpha stage therefore *manual* mode is preferred

#### Example

There are three steps necessary to get a scan result:

###### 1. Initialize the module in viewDidLoad

```objective_c
- (void)viewDidLoad {
    self.mrzModule = [[AnylineMRZModuleView alloc] initWithFrame:CGRectMake(0, 0, 640, 640)];
   [self.view addSubview:self.mrzModule];
    BOOL success = [self.mrzModule setupWithLicenseKey:kMyLicenseKey delegate:self error:&error];
    if( !success ) {
        // Handle the error here
    }
}
```

Create a property, initialize the module in the viewDidLoad method and add it to the view of the view controller. Supply the license key and set the delegate, which will receive a call once a result is found. The boolean returned by the setup routine notifies you if the Anyline set up was successful. If an error occurred the error needs to be handled.


###### 2. Start scanning process in viewDidAppear

```objective_c
- (void)viewDidAppear:(BOOL)animated {
    [super viewDidAppear:animated];
    NSError *error;
    BOOL success = [self.mrzModule startScanningAndReturnError:&error];
    NSAssert(success, @"Start failed: %@",error.debugDescription);
    if( !success ) {
        // Handle the error here
    }
}
```   
If there is a problem starting the scanning process an error object will be set, so the error can be handled.

###### 3. Implement the delegate method and receive results

```objective_c
- (void)anylineMRZModuleView:(AnylineMRZModuleView *)anylineMRZModuleView
           didFindScanResult:(ALIdentification *)scanResult
                     atImage:(UIImage *)image; {
    NSLog("Scan result: %@", scanResult);
}
```
When a valid result is found, it will call the delegate. ScanResult is an object of type ALIdentification containing the [information scanned.] (#scannedInfo)

### Cordova Plugin

###### Example call for the MRZ Module

Call the exec method with <i><b>scanMRZ</b></i> (all other parameters are as explained in [Quick Start] (#cordova-example)).

```java
cordova.exec(onResult, onError, "AnylineSDK", "scanMRZ", 	
    [
        "YOUR_LICENSE_KEY",
        {
            "captureResolution": "1080p",
            "cutout": {
                "style": "rect",
                "maxWidthPercent": "90%",
                "maxHeightPercent": "90%",
                "alignment": "top_half",
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
    ]
);
```
