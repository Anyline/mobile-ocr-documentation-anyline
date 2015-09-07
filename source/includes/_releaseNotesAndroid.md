# Anyline SDK Android Release Notes #

## Anyline SDK 3.2 RC1##

### New ###
- Community Version
- early alpha of the Debit-Card-Module
- New Licensing (Update requires new License Key)
- New internal build system with nightly tests

### Improved ###
- Energy Module: Electric Meter scan improved (better 6-7 digit distinction, less errors)

### Fixed ###
- CameraOpenListener in Modules not called


## Anyline SDK 3.1.1 ##
Release Date 07.09.2015

- fixed a reporting bug


## Anyline SDK 3.1 ##
Release Date 01.09.2015

- RC4 promoted to final 3.1 release


## Anyline SDK 3.1 RC4 ##
Release Date 24.07.2015

### Fixed ###
- ID Card MRZ Scanning does not work on multiple surnames
- Barcode scanning not working in Energy-Module

### New ###
- auto-flash support for all modules (alpha stage ... still prefere manual)

### Changed ###
- Barcode Module now also Reports an Image

```
//Listener changed from
void onResult(String result);
//To
void onResult(String result, AnylineImage anylineImage);
```

- MRZ Module getter Name in Identification Object changed (to be aliened with iOS)

```
//old
identification.getType();
//new
identification.getDocumentType();

//old
identification.getCheckDigitExpiration();
//new
identification.getCheckDigitExpirationDate();
```


## Anyline SDK 3.1 RC3 ##
Release Date 15.07.2015

### Improved ###
- Added support for all machine readable german passports in MRZ module
- Refined ID recognition in MRZ module


## Anyline SDK 3.1 RC2 ##
Release Date 14.07.2015

### New ###
- Added general support for german passports in MRZ module


## Anyline SDK 3.1 RC 1##
Release Date 07.07.2015

### New ###
- Ported 50 Operations to C++
- Added Modules and Views for:
- Anyline Barcode
- Anyline Energy
- Anyline MRZ
- Visual, haptic, sound feedback for successful scanning result
- Module interfaces configurable with interface builder
- New Operations:
- Count Nonzero Pixels
- Entropy in Rect
- Barcode find and rotate
- Contour Count
- Find Rotate Angle for Contour Lines
- Rect from Contours
- Remove Data Points
- Resolve Contour Intersect Contour
- Create Mask filled
- Draw Contours
- Draw Lines
- Draw Rect
- Draw Specs
- Find Hough Lines
- Histogram Equalization
- Init Contour Template
- Init Regex
- Init Size
- Is Image Equal
- Rect Distance
- Contour Template Finder
- Contour Template Loader
- OCR Contour
- Contrast Threshold
- Gradient Threshold
- Morphology Threshold
- Bounding Rect From Spec
- Extend Rect
- Count Results
- Match Result to Spec


## Anyline SDK 2.5.0 ##
Release Date 30.01.2014

### New ###
- Filter Contours Area Operation
- Adapt DataPoint for bounding rects in line
- CMYK Channel Operation
- HSV Channel Operation
- RGB Channel Operation
- Overlay Thresholding Operation
- own OCR Engine

### Improved ###
- TextDataPoints with character count
- BSThresholding with threshold factor
- parallelized Tesseract Operation
- descriptive patrameters for CodeParser
- refactored operations to use descriptive parameters and default values

### Fixed ###
- Tesseract OCR bugfix
- Resize Operation


## Anyline SDK 2.4.2 ##
Release Date 04.12.2014

### New ###
- Cut Thresholding Operation
- RGB Channel Extraction Operation
- HSV Channel Extraction Operation
- YMBC Channel Extraction Operation
- DataPoints for Line Operations
- Filter Contours in Area Operation

### Improved ###
- Parallelisation in OCR Detection
- BS Thresholding

### Fixed ###
- OCR bugfix


## Anyline SDK 2.4.1 ##
Release Date 04.12.2014

### New ###
- Watershed Operation
- Background Segmentation Thresholding
- Reflection Detection Operation
- QR / Barcode Scanning Operation
- Sort Contours Operation
- Init Image Operation for Parser
- Draw Border Operation
- Draw Bounding Rects in Image Operation
- Combine Images Operation
- Normalize Images Operation
- Set Color with Mask Operation
- Resolve Contours X Violation Operation

### Improved ###
- DataPoints with Grid Operation
- CodeParser Improvements
- Tesseract Operation works with Image Array
- More generic Thresholding Operation

### Fixed ###
- Values Stack bugfix
- Memory leaks with arm64
- rare occurring crashes fixed
- Digit DataPoint fixes


## Anyline SDK 2.4 ##
Release Date 28.10.2014

### New ###
- Mean Color in Rect Operation
- Color Distance Calculation Operation
- Validate Result Operation
- Clean Result Operation
- Find DataPoints Operation with configurable grid
- support for italic 7-segments

### Improved ###
- Interpreter refactored
- Improved Error & Exception handling

### Fixed ###
- fixed some possible Leaks
- fixed GPUImage bugs
- fixed minor issues with iOS 8


## Anyline SDK 2.3.5 ##
Release Date 01.10.2014

### Improved ###
- better OCR training capabilities

### Fixed ###
- Various Bugfixes


## Anyline SDK 2.3.4 ##
Release Date 23.09.2014

### New ###
- Square Angle Correction Operation
- Expand Square Operation
- Auto Rotate Image Operation
- Find Digit Position with Bounding Rects Operation
- Threshold Edge Detection GPU Operation

### Improved ###
- Find Square Operation
- ALSquare Bounding Rect Methods

### Fixed ###
- Various Bugfixes


## Anyline SDK 2.3.3 ##
Release Date 11.09.2014

### New ###
- Adaptive Luminance Thresholding Operation
- new Brightness in Rectangle Operation
- Luminance for Brightness Operation

### Improved ###
- Tesseract adaptive learning
- improved find data point
- find bounding squares with constraints
- better find data point area

### Fixed ###
- iOS8 color thresholding bug


## Anyline SDK 2.3.2 ##
Release Date 23.08.2014

### New ###
- Adapt Digit Position Operation with bounding rects
- Init Number Operation
- BS Thresholding Operation

### Improved ###
- Area constraint added for ApproxPolyDP Operation
- flipping values stack with none flipping support

### Fixed ###
- Observer removal bugfix
- reset equal count in values stack


## Anyline SDK 2.3.1 ##
Release Date 12.08.2014

### New ###
- new GetEqualCount Operation
- validation delegate for DisplayResult
- Init Operation for the ValuesStackFlipping

### Improved ###
- FindLargestSquareWithSizeRatio now works with Specs
- FindLargestSquareWithSizeRatio with ratio tolerance
- Transform Operation now works with Specs
- Flipping Values Stack now can accept single partial results
- DetectDigits with optional threshold parameter

### Fixed ###
- bugfixes for the ValuesStackFlipping
- bugfix for processing queue remove observer crash


## Anyline SDK 2.3 ##
Release Date 05.08.2014

### New ###
- modernised scripting language
- header part in scripting language
- support for encrypted command files
- support for reporting successful/unsuccessful scans
- specs are now defined with json
- validation delegate
- reporting KPIs

### Improved ###
- overall performance
- compatibility to android sdk

### Fixed ###
- various bugfixes


## Anyline SDK 2.2.1 ##
Release Date 29.07.2014

### Improved ###
- Exception handling
- Image processing clean ups

### Fixed ###
- Quality Calculation


## Anyline SDK 2.2 ##
Release Date 14.07.2014

### Improved ###
- better Image handling
- better thread managment
- better adapt digit positions
- overall performance improvements


## Anyline SDK 2.1 ##
Release Date 04.06.2014

### New ###
- iOS Interface Documentation added
- Anyline is now a fake dynamic library
- Anyline version & build number added
- Color Thresholding Operations
- Server Socket for Anylicious

### Fixed ###
- various bugfixes

### Improved ###
- better Image handling


## Anyline SDK 2.0 ##
Release Date 29.04.2014

### New ###
- Completely refactored the Anyline Framework.
- Anyline is now structured around Image Processing Operations.
- behaviour of Anyline is controlled over .alc command files.
- Simpler interface to communicate with Anyline.
- Overall faster performance.
- UI Stuff removed from Anyline binary.
- A lot of new Operations for the GPU.


## Anyline SDK 1.1 ##
Release Date 05.02.2014

### Improved ###
- Improved Display Overlay View with round corners.


## Anyline SDK 1.0 ##
Release Date 10.01.2014

### New ###
- Initial working version of Anyline for 7-segments.
