# Modules

The Anyline-Modules are use-case specific abstractions for Anyline. Each module is designed to serve a specific purpose. Currently, the following modules are available:

- [Barcode] (#barcodeModule)
- [Energy] (#energyModule)
- [MRZ (Machine Readable Zone)] (#mrzModule)
- [Document] (#documentModule)
- [AnylineOCR] (#anylineOcrModule)
- [Order Code] (#ordercodeModule) - only available for smart glasses (on request)

<a name="barcodeModule"> </a>
## Barcode

With the Anyline Barcode-Module 23 different types of bar- and QR-codes can be scanned. The result will simply be a *string* representation of the code.

#### Restrictions for the Barcode-Module Config:
- Flash mode *auto* is still in alpha stage therefore *manual* mode is preferred

#### Available Barcode Formats:

###### Fully Supported
- UPC A
- UPC E
- EAN-8
- EAN-13
- EAN-14
- EAN-18
- EAN-99
- EAN-128
- Identcode
- Leitcode
- ISBN-13
- ISBN-10
- ISSN
- ISMN
- ITF-14
- Data Matrix
- Aztec [not vCard - see below]
- Codabar
- QR-Code


###### Experimental Support:
- Code 39
- Code 93
- Code-128 [without rotation]
- PDF 417
- Aztec vCard

###### Currently not supported
- RSS 14
- RSS Expanded
- MSI/Plessey
- Code 93 Extended

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
    public void onResult(String result, BarcodeScanView.BarcodeFormat format, AnylineImage image) {
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
- kCodeTypeCode128
- kCodeTypeDataMatrix
- kCodeTypeEAN8
- kCodeTypeEAN13
- kCodeTypeITF
- kCodeTypePDF417
- kCodeTypeQR
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
                   barcodeFormat:(NSString *)barcodeFormat
                         atImage:(UIImage *)image {
    NSLog("Scan result: %@", scanResult);
}
```
When a valid result is found, it will call the delegate. The scan result will be a *string* containing the scanned code. barcodeFormat will contain the format of the barcode.

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

The Anyline Energy-Module is capable of scanning analog electric-, gas- and water-meter-readings.
It is also possible to scan bar- and QR-codes (useful for identifying meters).
Common digital meters and heat meters can also be scanned (but this is ALPHA).

#### All the possible scan modes

**Electric Meter**<br/>
Android: *ELECTRIC_METER*, iOS: *ALElectricMeter*<br/>
Scan analog electric meters with 5 or 6 main digits and one decimal digit.
The digit count is automatically detected. The decimal place is not included in the result. The auto-detection requires the decimal area to be highlighted in red.

**Electric Meter 5 main digits 1 decimal (ALPHA)**<br/>
Android: *ELECTRIC_METER_5_1*, iOS: *ALElectricMeter5_1*<br/>
Scan analog electric meters with 5 main digits and one decimal digit. If detected, the decimal is included in the result, otherwise the decimal place is omitted.
The decimal is represented by a dot in the result, not a comma.
This mode is useful if there is no red marking for the decimal place or the decimal place it self is relevant.

This mode may be removed in the future, if the same can be achieved with the automatic mode.

**Electric Meter 6 main digits 1 decimal (ALPHA)**<br/>
Android: *ELECTRIC_METER_6_1*, iOS: *ALElectricMeter6_1*<br/>
Same as above with 6 main digits.

**Gas Meter**<br/>
Android: *GAS_METER*, iOS: *ALGasMeter*<br/>
Scan analog gas meters with 5 digits before the point. The decimal places are ignored.

**Water Meter White (ALPHA)**<br/>
Android: *WATER_METER_WHITE*, iOS: *ALWaterMeterWhite*<br/>
Scan analog water meters with white background (black digits) with 5 digits before the point.
The decimal places are ignored.

**Water Meter Black (ALPHA)**<br/>
Android: *WATER_METER_BLACK*, iOS: *ALWaterMeterBlack*<br/>
Scan analog water meters with black background (white digits) with 5 digits before the point.
The decimal places are ignored.

**Digital Meter (ALPHA)**<br/>
Android: *DIGITAL_METER*, iOS: *ALDigitalMeter*<br/>
Is a general scanner for digital meters with at least 5 digits. It will try to find the highest number of connected digits and return the result without decimal marker.

**Heat Meter with 4 main (up to 3 decimal) digits (ALPHA)**<br/>
Android: *HEAT_METER_4*, iOS: *ALHeatMeter4*<br/>
Scan digital heat meters with 4 main and up to 3 decimal digits. If detected, the decimal digits are included in the result, otherwise they are omitted.

This mode may be replaced in the future with a mode that automatically detects the amount of digits.

**Heat Meter with 5 main (up to 3 decimal) digits (ALPHA)**<br/>
Android: *HEAT_METER_5*, iOS: *ALHeatMeter5*<br/>
Same as above with 5 digits before the point.

**Heat Meter with 6 main (up to 3 decimal) digits (ALPHA)**<br/>
Android: *HEAT_METER_6*, iOS: *ALHeatMeter6*<br/>
Same as above with 6 digits before the point.

**Bar- and QR-Codes**<br/>
Android: *BAR_CODE*, iOS: *ALBarcode*<br/>
Scan bar and qr codes. This mode can be used to identify a meter. See the [barcode module] (#available-barcode-formats) for supported types.

**Serial Numbers (ALPHA)**<br/>
Android: *SERIAL_NUMBER*, iOS: *ALSerialNumber*<br/>
Scan serial numbers that are engraved or printed onto a meter (consisting of numbers 0-9).


### Android

#### Restrictions for the Energy-Module Config
- Capture resolution is currently fixed to 720p on Android (optimized for good results on as many devices as possible).
- <b>The size and ratio of the cutout is predefined and cannot be changed </b>(sizes are optimized for best results)
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
2. Set the scan mode.
3. Call *initAnyline* with your valid license key and a new instance of EnergyResultListener, which is the callback for handling the results
4. Call *startScanning()*
5. When done call *cancelScanning()* and *releaseCameraInBackground()* or *releaseCamera()*

For each successful scan, you will receive four result-attributes:

- **scanMode:** the mode the result was found in
- **result**: the detected value as a String
- **resultImage**:
	 - the cropped image that has been used to scan the value
- **fullImage**:
	 - the full image (before cropping) (null in scan mode BAR_CODE and SERIAL_NUMBER)


###### Example: switching the scan mode
It is possible to switch the scan mode any time, but one need to call startScanning() again!
The example shows how switching could be implemented with a radio group selection.

```java
 // switch the scan mode depending on user selection
        radioGroup.setOnCheckedChangeListener(new RadioGroup.OnCheckedChangeListener() {
            @Override
            public void onCheckedChanged(RadioGroup group, int checkedId) {
                if (R.id.radio_electric == checkedId) {
                    energyScanView.setScanMode(EnergyScanView.ScanMode.ELECTRIC_METER);
                    energyScanView.startScanning();
                } else if (R.id.radio_electric_6 == checkedId) {
                    energyScanView.setScanMode(EnergyScanView.ScanMode.ELECTRIC_METER_5_1);
                    energyScanView.startScanning();
                } else if (R.id.radio_electric_7 == checkedId) {
                    energyScanView.setScanMode(EnergyScanView.ScanMode.ELECTRIC_METER_6_1);
                    energyScanView.startScanning();
                } else if (R.id.radio_gas == checkedId) {
                    energyScanView.setScanMode(EnergyScanView.ScanMode.GAS_METER);
                    energyScanView.startScanning();
                } else if (R.id.radio_water_white == checkedId){
                    energyScanView.setScanMode(EnergyScanView.ScanMode.WATER_METER_WHITE);
                    energyScanView.startScanning();
                } else{
                    energyScanView.setScanMode(EnergyScanView.ScanMode.WATER_METER_BLACK);
                    energyScanView.startScanning();
                }
            }

```



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


In the example *cancelOnResult* is set *true*, which means scanning will be stopped once a result is returned.

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

###### Reporting

The reporting of Analog Meter Results in the Community Edition (including an image of a scanned meter)
helps us to improving our product, and the customer experience.
It is possible to turn that feature off by calling *-(void)enableReporting:(BOOL)enable* on the AnylineEnergyModuleView.

```objective_c
[self.anylineEnergyView enableReporting:NO];
```

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
###### Reporting

The reporting of Analog Meter Results in the Community Edition (including an image of a scanned meter)
helps us to improving our product, and the customer experience.
It is possible to turn that feature off by adding *"reportingEnabled": false* to the above config.

```js
//...
            "blinkAnimationOnResult": true,
            "cancelOnResult": true,
            "reportingEnabled": false
        }
    ]
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
dayOfBirth | date of birth
checkDigitDayOfBirth | check digit for the date of birth
checkDigitExpirationDate | check digit for the expiration date
issuingCountryCode | issuing country code
nationalityCountryCode | nationality country code
countryCode | country code (Deprecated since 3.2.1. Use issuingCountryCode and nationalityCountryCode instead)
surNames    | surnames
givenNames | all given first names
personalNumber | personal number
personalNumber2 | 2nd personal number on TD1 sized MROTDs
checkDigitPersonalNumber | check digit for personal number
checkDigitDates | check digit for both dates
documentType |  type of the document that was read. (ID/P/IT)
documentNumber | document number
checkDigitNumber |  check digit for the document number
checkDigitFinal  | check digit
sex  | gender of the person
allCheckDigitsValid | flag indicating if all check digits are valid


<aside class="notice">
Please be aware that not every property is filled for every document type (e.g. some ID MRZ do not contain a sex information).
<p></p>
Be also aware, that the names may not be printed completely as the machine readable zone has a limited amount of characters.
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
If there is a problem while starting the scanning process, an error object will be set so the error can be handled.

###### 3. Implement the delegate method and receive results

```objective_c
- (void)anylineMRZModuleView:(AnylineMRZModuleView *)anylineMRZModuleView
           didFindScanResult:(ALIdentification *)scanResult
          allCheckDigitsValid:(BOOL)allCheckDigitsValid
                     atImage:(UIImage *)image {
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
<a name="documentModule"> </a>
## Document

The Anyline Document-Module detects document outlines, validates the interior angles to ensure the document is not skewed, detects the sharpness of the text and rectifies the document.

In the first step the preview frames are analyzed. Once a valid and sharp document is detected, a high resolution image is taken from the camera, analyzed, and, if valid and sharp, rectified and cropped.

The module does not perform the OCR step, but provides a high resolution, rectified, cropped document image, which is ensured to hold sharp text.

<aside class="notice">
The sharpness detection is robust in the face of text contrast, which means that sharpness detection works on gray text as well as on black text on a white background.
</aside>

 <a name="addtionalConfiguration"> </a>
**Additional Configuration:**
The resolution of the final high resolution image can be set in the [Configuration] (#anyline-config).
The additional configuration fields are:

field | description
----- | -----------
[pictureResolution](#pictureResolution) | Resolution of the high resolution document image

### Restrictions/hints for the Document-Module Config
- The bigger the cutout, the better
- It is advised to hide the cutout via setting the [strokeColor](#cutout_strokeColor) to #00000000
- It is that the [cutoutWidth](#cutout_width) matches the [captureResolution](#captureResolution)
- At this stage, the document scanner is limited to upright rectangular documents (like DIN-A4). Because of that, the [ratioFromSize](#cutout_ratioFromSize) should be set at around 10:15


### Android


#### Example
The following example files illustrate a simple use-case of the document module.

###### Example Activity
> in onCreate or onActivityCreated lifecycle methods

```java
documentScanView = (DocumentScanView) findViewById(R.id.document_scan_view);

// initialize Anyline with your license key and a Listener that is called if a result is found
documentScanView.initAnyline(getString(R.string.anyline_license_key), new DocumentResultListener() {
    @Override
    public void onResult(AnylineImage transformedImage, AnylineImage fullFrame) {

        // This is called once a result (a valid document containing sharp text) is found.
        // The transformedImage contains the rectified and cropped document image,
        // the fullFrame contains the full picture that was taken

        /**
         * IMPORTANT: cache provided frames here, and release them at the end of this callback. Because
         * keeping them in memory (e.g. setting the full frame to an ImageView)
         * could result in a OutOfMemoryError (depending on the chosen pictureResoultion)). This error is reported in {@link #onTakePictureError
         * (Throwable)}
         *
         * Use a DiskCache http://developer.android.com/training/displaying-bitmaps/cache-bitmap.html#disk-cache
         * for example
         *
         */

    }

    @Override
    public void onPictureProcessingFailure(DocumentScanView.DocumentError documentError) {
        // handle an error while processing the full picture here
        // the preview will be restarted automatically
    }

    @Override
    public void onPreviewProcessingSuccess(AnylineImage anylineImage) {
        // this is called after the preview of the document is completed, and a full picture will be
        // processed automatically
    }

    @Override
    public void onPreviewProcessingFailure(DocumentScanView.DocumentError documentError) {
        // this is called on any error while processing the document image
        // Note: this is called every time an error occurs in a run, so that might be quite often
        // An error message should only be presented to the user after some time
    }

    @Override
    public boolean onDocumentOutlineDetected(List<PointF> list, boolean anglesValid) {
        // is called when the outline of the document is detected. return true if the outline is consumed by
        // the implementation here, false if the outline should be drawn by the DocumentScanView
    }

    @Override
    public void onTakePictureSuccess() {
        // this is called after the image has been captured from the camera and is about to be processed
    }

    @Override
    public void onTakePictureError(Throwable throwable) {
        // This is called if the image could not be captured from the camera (most probably because of an
        // OutOfMemoryError)
    }
});

// optionally stop the scan once a valid result was returned
// documentScanView.cancelOnResult(true);       
```
> in onResume()

```java
documentScanView.startScanning();
```

> in onPause()

```java
documentScanView.cancelScanning();
//IMPORTANT: always release the camera in onPause
documentScanView.releaseCameraInBackground();
```

In order to start the scan process, perform the following steps:

1. If you prefer a json-file for configuration, use the *setConfigFromAsset* method and place the json-config in the Android assets folder, otherwise configure the view using the xml attributes in the activity layout file
2. Call *initAnyline* with your valid license key and a new instance of DocumentResultListener, which is the callback for handling the results
3. Call *startScanning()*
4. When done call *cancelScanning()* and *releaseCameraInBackground()* or *releaseCamera()*

For each successful scan, you will receive two result-attributes:

- **transformedImage:** the rectified document image
- **fullFrame**: the full frame the document was anylised from

###### Example Activity Layout

```xml
<RelativeLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

<at.nineyards.anyline.modules.document.DocumentScanView
    android:id="@+id/document_scan_view"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    />

</RelativeLayout>
```

The DocumentScanView can simply be included in the activity layout.

For custom configuration (e.g. cutout, flash, feedback on successful scan, etc.) you can either use a json-file or XML-attributes like in the example. If you need more detailed information about all available config items see [Anyline Config] (#anyline-config).


### iOS

#### Example
The following example illustrate a simple use-case of the document module.

In order to start the scan process, perform the following steps:

1. Create a `ALUIConfiguration` object and use its properties to configure the user interface (or load a layout file)
3. Initialize the module with a license key, delegate. The delegate will be called with the result (and other information) according to the `AnylineDocumentModuleDelegate` protocol
4. In `viewDidAppear:` call `startScanningAndReturnError:`
5. After you received a result call `cancelScanningAndReturnError:` (This also has to be called in viewWillDisappear: to clean up the module)


> in viewDidLoad

```objective_c
    CGRect frame = [[UIScreen mainScreen] applicationFrame];
    frame = CGRectMake(frame.origin.x, frame.origin.y + self.navigationController.navigationBar.frame.size.height, frame.size.width, frame.size.height - self.navigationController.navigationBar.frame.size.height);
    self.documentModuleView = [[AnylineDocumentModuleView alloc] initWithFrame:frame];
    
    NSError *error = nil;
    // We tell the module to bootstrap itself with the license key and delegate. The delegate will later get called
    // by the module once we start receiving results.
    BOOL success = [self.documentModuleView setupWithLicenseKey:kDocumentScanLicenseKey delegate:self error:&error];
    // setupWithLicenseKey:delegate:error returns true if everything went fine. In the case something wrong
    // we have to check the error object for the error message.
    if( !success ) {
        // Something went wrong. The error object contains the error description
        NSAssert(success, @"Setup Error: %@", error.debugDescription);
    }
    
    // After setup is complete we add the module to the view of this view controller
    [self.view addSubview:self.documentModuleView];

```
> in viewDidAppear()

```objective_c
[self.documentModuleView startScanningAndReturnError:&error];
```

> in viewWillDisappear()

```objective_c
[self.documentModuleView cancelScanningAndReturnError:&error];
```

To receive results implement the following delegate method:

```objective_c
- (void)anylineDocumentModuleView:(AnylineDocumentModuleView *)anylineEnergyModuleView hasResult:(UIImage *)transformedImage fullImage:(UIImage *)fullFrame {
   NSLog(@"Anyline has a result");
}
```

For each successful scan, you will receive two objects:

- **transformedImage:** the cropped and transformed image of the document
- **fullFrame**: the full image taken by the camera

The AnylineDocumentModuleView can simply be added as a subview.

For custom configuration (e.g. cutout, flash, feedback on successful scan, etc.) you can either use a json-file or configure it by accessing the properties of the module. If you need more detailed information about all available config items see [Anyline Config] (#anyline-config).


<a name="anylineOcrModule"> </a>
## Anyline OCR

With the Anyline OCR Module it is possible to set up scanning for special/custom use cases.
There are two different modes for this module: LINE and GRID.
The LINE mode is optimal for scanning one or more lines of variable length or font (like IBANs or addresses).
The GRID mode is optimal for characters with equal size laid out in a grid with a constant font, background and character count (like loyalty codes inside bottle caps).

If the provided settings are not enough to achieve a great result, you can
<a href="https://www.anyline.io/support-request/">contact us</a> and we can create a command file that is optimized
for that your case. This script can then be set as a parameter to this module (so minimal changes are required to your app).

#### Settings Common

property | description
----- | -----------
scanMode | Available modes: `LINE` or `GRID`
customCmdFile | An optional custom command file
minCharHeight | The minimum height of a character to scan in pixels (relative to the configured capture resolution)
maxCharHeight | The maximum height of a character to scan in pixels (relative to the configured capture resolution)
tesseractLanguages | The languages to use for the OCR (e.g. to use custom.traineddata set this value "custom")
charWhitelist | Filter the recognized characters to only allow the ones included in this list (e.g. `"ABCDEFGHIJKLMNOPQRSTUVWXYZ1234567890"` for capital letters and numbers only)
validationRegex | A regex string to validate the result (invalid results will not be returned). (regex is in <a href="http://www.cplusplus.com/reference/regex/ECMAScript/">ECMAScript regular expressions pattern syntax</a>)
minConfidence | The minimum confidence required to return a result, a value between 0 and 100. (higher confidence means it may take longer to get a result but it is more likely to be accurate)

#### Settings LINE mode only
property | description
----- | -----------
removeSmallContours | If this is `true` small contours will be ignored (This is useful for latin capital letters and numbers. This is detrimental if details are relevant, like the dot above the letter `i`)

#### Settings GRID mode only
property | description
----- | -----------
charCountX | The number of horizontal characters
charCountY | The number of vertical characters
charPaddingXFactor | The average horizontal distance between two characters, measured in percentage of the characters width
charPaddingYFactor | The average vertical distance between two characters, measured in percentage of the character height
isBrightTextOnDark | If `true` this scans bright text on dark background. <br> If `false` this scans dark text on bright background

### Android

#### Example
The following example illustrates an IBAN scanner use-case realized with the Anyline OCR module.

###### Example Activity for the IBAN use case
> in `onCreate` or `onActivityCreated` lifecycle methods

There are five simple steps necessary to get started:

1. Set the view config with `setConfig` (or in the layout xml)
2. Set OCR parameters with `setOcrConfig`
3. Initialize the module with a license and a listener. The listener is called with the result (and other information) using `initAnyline`
4. Call `startScanning()`
5. When you are done call `cancelScanning()` and `releaseCameraInBackground()` or `releaseCamera()`

```java
// Get the view from the layout
scanView = (AnylineOcrScanView) findViewById(R.id.scan_view);
// Configure the view (cutout, the camera resolution, etc.) via json (can also be done in xml in the layout)
scanView.setConfig(new AnylineViewConfig(this, "iban_view_config.json"));

// Copies given traineddata-file to a place where the core can access it.
// This MUST be called for every traineddata file that is used (before startScanning() is called).
// The file must be located directly in the assets directory (or in tessdata/ but no other folders are allowed)
scanView.copyTrainedData("tessdata/eng.traineddata", "f4b86e0b7eb03c682f0e5e6519e02d32");
scanView.copyTrainedData("tessdata/deu.traineddata", "2d5190b9b62e28fa6d17b728ca195776");

//Configure the OCR for IBANs
AnylineOcrConfig anylineOcrConfig = new AnylineOcrConfig();
// use the line mode (line length and font may vary)
anylineOcrConfig.setScanMode(AnylineOcrConfig.ScanMode.LINE);
// set the languages used for OCR
anylineOcrConfig.setTesseractLanguages("eng", "deu");
// allow only capital letters and numbers
anylineOcrConfig.setCharWhitelist("ABCDEFGHIJKLMNOPQRSTUVWXYZ1234567890");
// set the height range the text can have
anylineOcrConfig.setMinCharHeight(30);
anylineOcrConfig.setMaxCharHeight(60);
// The minimum confidence required to return a result, a value between 0 and 100.
// (higher confidence means less likely to get a wrong result, but may be slower to get a result)
anylineOcrConfig.setMinConfidence(65);
// a simple regex for a basic validation of the IBAN, results that don't match this, will not be returned
// (full validation is more complex, as different countries have different formats)
anylineOcrConfig.setValidationRegex("^[A-Z]{2}([0-9A-Z]\\s*){13,32}$");
// removes small contours (helpful in this case as no letters with small artifacts are allowed, like iöäü)
anylineOcrConfig.setRemoveSmallContours(true);
// set the ocr config
scanView.setAnylineOcrConfig(anylineOcrConfig);

// initialize with the license and a listener
scanView.initAnyline(license, new AnylineOcrListener() {
    @Override
    public void onReport(String identifier, Object value) {
        // Called with interesting values, that arise during processing.
        // Some possibly reported values:
        //
        // $brightness - the brightness of the center region of the cutout as a float value
        // $confidence - the confidence, an Integer value between 0 and 100
        // $thresholdedImage - the current image transformed into black and white
    }

    @Override
    public boolean onTextOutlineDetected(List<PointF> list) {
        // Called when the outline of a possible text is detected.
        // If false is returned, the outline is drawn automatically.
        return false;
    }

    @Override
    public void onResult(AnylineOcrResult result) {
        // Called when a result is found (minimum confidence is exceeded and validation with regex was ok)
        ibanResultView.setResult(result.getText());
        ibanResultView.setVisibility(View.VISIBLE);
    }

    @Override
    public void onAbortRun(AnylineOcrError code, String message) {
        // Is called when no result was found for the current image.
        // E.g. if no text was found or the result is not valid.
    }
});
```
> in onResume()

```java
scanView.startScanning();
```

> in onPause()

```java
scanView.cancelScanning();
//IMPORTANT: always release the camera in onPause
scanView.releaseCameraInBackground();
```

###### Example Activity Layout

```xml
<RelativeLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <at.nineyards.anyline.modules.ocr.AnylineOcrScanView
        android:id="@+id/scan_view"
        android:layout_width="match_parent"
        android:layout_height="match_parent"/>
</RelativeLayout>
```

The AnylineOcrScanView can simply be included in the activity layout file, just like any other view. The view can be either configured via XML or with a config json.

<a name="ibanViewConfig"> </a>
###### Example view config for the IBAN use case

```json
{
  "captureResolution":"1080",
  "cutout": {
    "style": "rect",
    "maxWidthPercent": "80%",
    "maxHeightPercent": "80%",
    "alignment": "top_half",
    "width": 870,
    "ratioFromSize": {
      "width": 5,
      "height": 1
    },
    "strokeWidth": 2,
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
  "cancelOnResult": true
}
```

A detailed description of all available config items can be found in the chapter [Anyline Config] (#anyline-config)

It is also possible to use xml-attributes instead of the json config file. For more detailed information see [XML Configuration] (#configureViaXML)

##### Other Examples

For more example use cases of the Anyline OCR Module, check out the Examples app in the download package (available here: <a href="https://www.anyline.io/download/">https://www.anyline.io/download/</a>)

### iOS

#### Example
The following example illustrates an IBAN scanner use-case realized with the Anyline OCR module.

###### Example Activity for the IBAN use case
> in `viewDidLoad` lifecycle method

There are five simple steps necessary to get started:

1. Create a `ALUIConfiguration` object and use its properties to configure the user interface (or load a layout file)
2. Create a `ALOCRConfig` object and use its properties to configure the module
3. Initialize the module with a license key, delegate and the `ALOCRConfig` object. The delegate will be called with the result (and other information) according to the `AnylineOCRModuleDelegate` protocol
4. In `viewDidAppear:` call `startScanningAndReturnError:`
5. After you received a result call `cancelScanningAndReturnError:` (This also has to be called in viewWillDisappear: to clean up the module)

```objective_c
// The controller has to conform to <AnylineOCRModuleDelegate> to be able to receive results
@interface ALISBNScanViewController ()<AnylineOCRModuleDelegate>
// The Anyline module used for OCR
@property (nonatomic, strong) AnylineOCRModuleView *ocrModuleView;

@end

@implementation ALISBNScanViewController
/*
 We will do our main setup in viewDidLoad. Its called once the view controller is getting ready to be displayed.
 */
- (void)viewDidLoad {
    [super viewDidLoad];
    
    // Initializing the module. Its a UIView subclass. We set the frame to fill the whole screen
    CGRect frame = [[UIScreen mainScreen] applicationFrame];
    frame = CGRectMake(frame.origin.x, frame.origin.y + self.navigationController.navigationBar.frame.size.height, frame.size.width, frame.size.height - self.navigationController.navigationBar.frame.size.height);
    self.ocrModuleView = [[AnylineOCRModuleView alloc] initWithFrame:frame];
    
    ALOCRConfig *config = [[ALOCRConfig alloc] init];
    config.charHeight = ALRangeMake(20, 70);
    config.tesseractLanguages = @[@"eng", @"deu"];
    config.charWhiteList = @"ISBN0123456789<>-X";
    config.minConfidence = 65;
    config.validationRegex = @"^ISBN\\s*((978|979){1}-?\\s*)*[0-9]{1,5}-?\\s*[0-9]{2,7}-?\\s*[0-9]{2,7}-?\\s*[0-9X]$";
    config.removeSmallContours = YES;
    config.scanMode = ALLine;
    
    NSError *error = nil;
    // We tell the module to bootstrap itself with the license key and delegate. The delegate will later get called
    // by the module once we start receiving results.
    BOOL success = [self.ocrModuleView setupWithLicenseKey:kISBNLicenseKey
                                                  delegate:self
                                                 ocrConfig:config
                                                     error:&error];
    // setupWithLicenseKey:delegate:error returns true if everything went fine. In the case something wrong
    // we have to check the error object for the error message.
    if (!success) {
        // Something went wrong. The error object contains the error description
        NSAssert(success, @"Setup Error: %@", error.debugDescription);
    }

    // Load the UI Configuration from a json file
    NSString *confPath = [[NSBundle mainBundle] pathForResource:@"isbn_config" ofType:@"json"];
    ALUIConfiguration *ibanConf = [ALUIConfiguration cutoutConfigurationFromJsonFile:confPath];
    self.ocrModuleView.currentConfiguration = ibanConf;
    
    // After setup is complete we add the module to the view of this view controller
    [self.view addSubview:self.ocrModuleView];
}

/*
 This is the main delegate method Anyline uses to report its results
 */
- (void)anylineOCRModuleView:(AnylineOCRModuleView *)anylineOCRModuleView
               didFindResult:(ALOCRResult *)result {
    NSLog(@"Anyline reports result:", result.text);
}

- (void)anylineOCRModuleView:(AnylineOCRModuleView *)anylineOCRModuleView
             reportsVariable:(NSString *)variableName
                       value:(id)value {
    NSLog(@"Anyline reports varaible: %@ with value: %@", variableName, value);
}

- (void)anylineOCRModuleView:(AnylineOCRModuleView *)anylineOCRModuleView
           reportsRunFailure:(ALOCRError)error {
    NSLog(@"Anyline reports varaible an error: %@", error);
}

@end
```

> in viewDidAppear()

```objective_c
[self.ocrModuleView startScanningAndReturnError:&error];
```

> in viewWillDisappear()

```objective_c
[self.ocrModuleView cancelScanningAndReturnError:&error];
```

To receive results implement the following delegate method:

```objective_c
- (void)anylineOCRModuleView:(AnylineOCRModuleView *)anylineOCRModuleView didFindResult:(ALOCRResult *)result {
   NSLog(@"Anyline has a result");
}
```


<a name="ibanViewConfig"> </a>
###### Example view config for the IBAN use case

The ocr module can simply be added as a subview, just like any other view. The view can be either configured via `ALUIConfiguration` or with a config json file.

```json
{
  "captureResolution":"1080",
  "cutout": {
    "style": "rect",
    "maxWidthPercent": "80%",
    "maxHeightPercent": "80%",
    "alignment": "top_half",
    "width": 870,
    "ratioFromSize": {
      "width": 5,
      "height": 1
    },
    "strokeWidth": 2,
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
  "cancelOnResult": true
}
```

A detailed description of all available config items can be found in the chapter [Anyline Config] (#anyline-config)

##### Other Examples

For more example use cases of the Anyline OCR Module, check out the Examples app in the download package (available here: <a href="https://www.anyline.io/download/">https://www.anyline.io/download/</a>)

<a name="ordercodeModule"> </a>
## Order Code

<aside class="notice">
<b>A first version of the Order Code Module for smart glasses is available on request. For a valid license please <a href="https://www.anyline.io/support-request/">contact</a> us </b>
</aside>

With the Anyline Order Code Module it is possible to scan a specific type of Ordercode.
The code consists of 11 alphanumeric characters in different font sizes. The result will simply be a *string* representation of the code.

#### Example
The following example files illustrate a simple use-case of the order code module.

###### Example Activity
> in onCreate or onActivityCreated lifecycle methods

```java
ordercodeScanView = (OrdercodeScanView) findViewById(R.id.order_code_scan_view);
ordercodeScanView.setConfigFromAsset("order_code_view_config.json");

// initialize Anyline with your license key and a Listener that is called if a result is found
ordercodeScanView.initAnyline(getString(R.string.anyline_license_key), new OrdercodeResultListener() {

    @Override
    public void onResult(String result, AnylineImage image) {

        // This is called when a result is found.
    }
});
ordercodeScanView.startScanning();
```
> in onResume()

```java
ordercodeScanView.startScanning();
```

> in onPause()

```java
ordercodeScanView.cancelScanning();
//IMPORTANT: always release the camera in onPause
ordercodeScanView.releaseCameraInBackground();
```

There are four simple steps necessary to get started:

1. If you do not use XML configuration set the [config-file] (#ordercodeConfig) to your OrdercodeScanView using the *setConfigFromAsset* method and make sure that the json config-file is located in the Android assets folder.
2. Call initAnyline with your valid license key and a new instance of OrdercodeResultListener, which can be used to handle the results.
3. Call *startScanning()*
4. When done call *cancelScanning()* and *releaseCameraInBackground()* or *releaseCamera()*

###### &NewLine;

###### Example Activity Layout

```xml
<RelativeLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

 <at.nineyards.anyline.modules.ordercode.OrdercodeScanView
    android:id="@+id/ordercode_scan_view"
    android:layout_width="match_parent"
    android:layout_height="match_parent"/>
</RelativeLayout>
```

The OrdercodeScanView can simply be included in the activity layout file, just like any other view. The view can be either configured here via XML or otherwise a config json file can be used to adapt the scan view.

<a name="ordercodeConfig"> </a>
###### Example config for the Barcode Module

```json
{
  "captureResolution":"1080p",
  "cutout": {
    "style": "rect",
    "maxWidthPercent": "40%",
    "alignment": "center",
    "ratioFromSize": {
      "width": 340,
      "height": 76
    },
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
    "strokeWidth": 3,
    "cornerRadius": 10,
    "strokeColor": "FFFFFF",
    "outerColor": "000000",
    "outerAlpha": 0.3
  },
  "beepOnResult": false,
  "blinkAnimationOnResult": true,
  "cancelOnResult": false
}
```

The config file enables a quick and easy adaption of the scan view.

Some of the most important config options may be:

Parameter | Description
--------- | -----------
captureResolution | the preferred camera preview size
cutout | defining which area of the preview will be "cutout" (analyzed to find bar/QR code)
beepOnResult | enables acoustic feedback on successful scan - only available when headset is connected
blinkOnResult | visual feedback on successful scan
cancelOnResult | true, if the scanning process should be stopped after one result; needs manual restart for additional scans

A detailed description of all available config items can be found in [Anyline Config] (#anyline-config)

It is also possible to use xml-attributes instead of the json config file. For more detailed information see [XML Configuration] (#configureViaXML)
