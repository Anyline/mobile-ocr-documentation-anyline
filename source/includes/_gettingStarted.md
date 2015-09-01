# Getting Started

This section helps you to get started scanning using the Anyline SDK and a brief description of the files included in SDK bundle as well as a requirement description and a Quick Start Guide for all available platforms.

## Obtaining an Anyline SDK Licence Key

The best way to use Anyline is to purchase your personal licence in our [Anyline Store](https://www.anyline.io/store/). Each key is bound to a specific application by the Bundle Identifier (iOS) or the ApplicationId (Android).

### iOS Bundle Identifier
The iOS Bundle Identifier is best found in Xcode in the Project Overview / General.

![BundleIDiOS](images/bundleIDiOS.png)

### Android ApplicationId
The Android ApplicationId is found in the build.gradle file as android / defaultConfig / applicationId.

![BundleIDAndroid](images/bundleIDAndroid.png)

If you do not use the ApplicationId in your build.gradle file you may use the package name shown in the AndroidManifest.xml.

![BundleIdAndroid2](images/bundleIDAndroid2.png)


## Android Getting Started ##

The Android bundle contains the following parts: 

- **AnylineSDK:**      contains the anyline-android-release.aar library
- **Documentation:**    contains the java doc for the anyline-sdk
- **AnylineSDKExamples:**      contains a simple app where an example for each module is implemented - it can be installed right away
- **LICENSE:** 			third party license agreements
- **README:**			contains a quick start - setup and module description


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

##### 1. Copy AnylineSDK.aar to your apps "libs" directory and modify the build.gradle as shown in the example.

###### &NewLine;  

##### 2. Provide a config file (json or xml)

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

The config file enables a quick and easy adaption of the "Scan-View". You can either provide a json-file or use the XML-attributes in the layout file itself.
A detailed description of all available attributes can be found in [Anyline Config] (#anyline-config)

###### JSON

This config file must be located in the **assets** folder of your Android project.

Some of the most important config options may be:

Parameter | Description 
-------- | -----------
captureResolution | the preferred camera preview size
cutout | defining which area of the preview will be "cutout" (analyzed to find bar/QR code)
flash | defines the flash mode, where to place the flash symbol, etc.
beepOnResult | enables sound on successful scanning process (for modules only)
vibrateOnResult | provides haptic feedback for a successful scanning process (for modules only)
blinkOnResult |  visual feedback for a successful scanning process (for modules only)
cancelOnResult | if true, the scanning process will be stopped after one result and needs to be restarted manually (for modules only)

###### XML


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

Alternatively to a json config, it is also possible to configure the view (EnergyScanView, MrzScanView, BarcodeScanView) using XML-attributes in the layout-file.

###### &NewLine;  

####3. Init Anyline in your Activity
There are module specific options - take a look at the description of the desired module to get more detailed information.

####4. Enjoy scanning and have fun :)


##iOS Getting Started##

- **Framework:**        contains the Anyline.framework & AnylineResources.bundle
- **Documentation:**    contains a html & docset version of an appledoc style interface documentation
- **AnylineExamples:**  contains a simple app where each module is implemented - it can be installed right away
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


In the import screen select *Copy items if needed* and *Create groups* and add the files to your target.
![Copy Frameworks](images/CopyFramework.jpg)

#### 2. Linking Frameworks

After the framework and bundle got imported go to your project inspector. In the *Build Phases* tab, add the following libraries:

- libc++.dylib
- libstdc++.6.0.9.dylib
- libiconv.dylib
- AssetsLibrary.framework	

After adding the libraries, it should look like this (notice the AnylineResources bundle, if its not in *Copy Bundle Resources* add it):
![Link Frameworks](images/LinkFrameworks.jpg)

#### 3. Linker Flags 

In your project inspector switch to the *Build Settings* tab  and search for *Other Linker Flags*. Select
*Other - Other Linker Flags* and add *-&nbsp;ObjC*. 
This flag causes the linker to load every object file in the library that defines an Objective-C class or category.

![Linker Flags](images/LinkerFlags.jpg)


####4. Init an AnylineModuleView in your ViewController or Storyboard

There are module specific options - take a look at the description of the desired module to get more detailed information.

####5. Enjoy scanning and have fun :)

## Cordova Plugin ##

The Cordova bundle contains the following parts: 


- **anylinesdk-plugin:**      contains the anylinesdk cordova-plugin that can be added to your project
- **example:**    a simple cordova example project, demonstrating how the anylinesdk cordova-plugin can be used
- **PLUGIN_USAGE:**     quick start guide

### Requirements

#### Android
- Android device with SDK >= 15
- decent camera functionality (recommended: 720p and adequate auto focus)

#### iOS
- minimum iOS 7.0
- minimum iPhone4s


### Quick Start - Setup
This is just a simple setup guide to integrate the anylinesdk-plugin in an existing Cordova project.<br/>
For more information about Cordova, how to use plugins, etc. see <a target="_blank" href="https://cordova.apache.org/">https://cordova.apache.org/</a>.

######1. Add the anylinesdk-plugin to your existing cordova project
```plaintext
cordova plugin add PATH_TO_DIR/anylinesdk-plugin/
```

Copy the <i>anylinesdk-plugin</i> folder and add the plugin to your Cordova project. Make sure you have added at least on of the platforms (Android, iOS) to your project.

######2. Modify the config.xml
```xml
<feature name="AnylineSDK">
    <param name="android-package" value="io.anyline.cordova.AnylinePlugin" />
    <param name="onload" value="true" />
    <param name="ios-package" value="AnylineSDKPlugin" />
    <param name="id" value="io.anyline.cordova" />
</feature>
```
Add the following code to your (auto-generated) <i>config.xml</i> of your Cordova project, declaring the feature "AnylineSDK".


######3. Copy the js-files or create custom js-file

The <i>example</i> project provides sample scripts which are optimized for each module. The js-files can be found in <i>example/www/js/</i>.<p/>
You can either reuse those sample scripts or create your own and adapt them to your needs. <br/>
Just make sure the script is called in the <i>index.html</i> of the Cordova project.<p/>

```html
cordova.exec(onResult, onError, "AnylineSDK", scanMode, config);
```
<p/>
<a name="cordova-example"></a>
Basically, there is one simple exec-call to the AnylineSDK with the following parameters:

- <b>onResult</b>: a function that is called on a scan result
- <b>onError</b>: a function that is called on error or when the user canceled the scanning
- <b>AnylineSDK</b>: add this *string* to make sure the anyline-sdk plugin is called
- <b>scanMode</b>: one of "<i>scanMRZ</i>", "<i>scanBarcode</i>", "<i>scanElectricMeter</i>", "<i>scanGasMeter</i>"
- <b>config</b>: an array
    * <b>config[0]</b>: the license key
    * <b>config[1]</b>: the [json config] (#anyline-config) for the view, passed as json-array
	

> Example for **config** from MRZ:

```json
	["YOUR_LICENSE_KEY",  
	{ "captureResolution": "1080p", 
		"cutout": { 
				"style": "rect", 
				"maxWidthPercent": "90%", 
				"maxHeightPercent": "90%", 
				"alignment": "top_half", 
				"strokeWidth": 2, 
				"cornerRadius": 4, 
				"strokeColor": "FFFFFF", 
				"outerColor": "000000", 
				"outerAlpha": 0.3 }, 
		"flash": { 
				"mode": "manual", 
				"alignment": "bottom_right" }, 
		"beepOnResult": true, 
		"vibrateOnResult": true, 
		"blinkAnimationOnResult": true, 
		"cancelOnResult": true 
	}]
```


######4. Run your cordova project: Enjoy scanning and have fun :)


