
##  AnylineSDK for iOS  ##

- Framework:        contains the Anyline.framework & AnylineResources.bundle
- Documentation:    contains a html & docset version of an appledoc style interface documentation
- AnylineExamples:  contains a simple app where each module is implemented, it can be installed right away


### Requirements


- minimum iOS 7.0
- minimum iPhone4s




### Quick Start - Setup


##### 1. Simply drag & drop Anyline.framework & AnylineResources.bundle into your project tree.


##### 2.    In the import screen select "Copy items if needed" and "Create groups" and add the files to your target.


##### 3.    After the framework and bundle got imported go to your project inspector. Under the "Build Phases" tab add the following libraries:

- libc++.dylib
- libstdc++.6.0.9.dylib
- libiconv.dylib
- AssetsLibrary.framework


##### 4.    Init an AnylineModuleView in your ViewController or Storyboard
There are module specific options - take a look at the description in the desired module description below.


##### 5. Enjoy scanning and have fun :)

# 
### Modules

### Barcode Module

With the Anyline-Barcode-Module any kind of bar- and qr-codes can be scanned.
The result will be simply a String representation of the code.



Restrictions for the Barcode-Module Config:
- Flash mode "auto" is not (yet) supported.


Init Anyline:
- Init an AnylineBarcodeModuleView View with frame or add the View to your Storyboard.
- Setup the AnylineBarcodeModuleView with a valid license key and delegate
- Optional you may also limit the barcode scanning to one or multiple barcode formats (see also 'Available Barcode Formats' below)
- Call startScanningAndReturnError: at viewDidAppear: or later.

Example Code:

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
    
Available Barcode Formats:              
        AZTEC
        CODABAR
        CODE_39
        CODE_93
        CODE_128
        DATA_MATRIX
        EAN_8
        EAN_13
        ITF
        PDF_417
        QR_CODE
        RSS_14
        RSS_EXPANDED
        UPC_A
        UPC_E
        UPC_EAN_EXTENSION
    



### MRZ Module

The Anyline-MRZ-Module provides the functionality to scan passports and other IDs with a MRZ (machine-readable-zone).
For each scan result the module generates an Identification Object, containing all relevant information 
(e.g. document type and number, name, day of birth, etc.) as well as the image of the scanned document.



Restrictions for the MRZ-Module Config:
- Flash mode "auto" is not (yet) supported.


Init Anyline:
- Initialize the module in viewDidLoad
- Start the scanning process in viewDidAppear
- Implement the delegate method and receive results


```Obj-C
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


### Energy Module

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
    

```Obj-C
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