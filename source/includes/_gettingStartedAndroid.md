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
