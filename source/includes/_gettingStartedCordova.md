
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

###### 1. Add the anylinesdk-plugin to your existing cordova project
```plaintext
cordova plugin add PATH_TO_DIR/anylinesdk-plugin/
```

Copy the <i>anylinesdk-plugin</i> folder and add the plugin to your Cordova project. Make sure you have added at least on of the platforms (Android, iOS) to your project.

###### 2. Modify the config.xml
```xml
<feature name="AnylineSDK">
    <param name="android-package" value="io.anyline.cordova.AnylinePlugin" />
    <param name="onload" value="true" />
    <param name="ios-package" value="AnylineSDKPlugin" />
    <param name="id" value="io.anyline.cordova" />
</feature>
```
Add the following code to your (auto-generated) <i>config.xml</i> of your Cordova project, declaring the feature "AnylineSDK".


###### 3. Copy the js-files or create custom js-file

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
```


###### 4. Run your cordova project: Enjoy scanning and have fun :)
