# Xamarin Setup Guide #

The Anyline SDK is available for both Xamarin.Android and Xamarin.iOS. Each mobile platform has their own Anyline Xamarin SDK and and can be used very similar to the regular Anyline bundles for Android and iOS. Please refer to our API Reference for platform-specific documentation.

## Xamarin.Android ##

The Anyline Xamarin SDK provides the functionality of our Android SDK in C# to make Android integration in Xamarin as easy as possible. This section helps you to setup your Xamarin.Android project and start scanning right away!

### Requirements ###

To use the Anyline SDK for Xamarin.Android, you'll need:

- Xamarin account (If you work with Visual Studio, you need at least a Xamarin business account. Check out the <a href="https://xamarin.com/">Xamarin website</a> for detailed information.)
- Android device with SDK >= 15
- Decent camera functionality (recommended: 720p and adequate auto focus)
- Xamarin Studio or Visual Studio
- An Anyline license which you can generate <a href="http://portal.anyline.io/">here</a> after purchasing our SDK in the <a href="https://www.anyline.io/store/">store</a>.

The Android bundle contains the following parts:

- **AnylineXamarinSDK.Droid.dll:** contains the Anyline SDK Library
- **AnylineXamarinApp.Droid** contains a simple app where an example for each module is implemented - it can be installed right away
- **LICENSE:** third party license agreements
- **README:** contains a quick start - setup and module description

### Quick Start - Setup ###

In this section we will go through the basic process of creating and configurating a simple Anyline scanning application in Xamarin.Android. If you are not yet familiar with Xamarin.Android, follow <a href="https://developer.xamarin.com/guides/android/getting_started/hello,android/hello,android_quickstart/">this</a> guide to develop an understanding of the fundamentals of Android application development with Xamarin.

#### 1. Create a new Xamarin.Android Project ####

If you are using Visual Studio, click `File` > `New Project...`, select `Visual C#` > `Android` and create a new Blank App.

![XamarinAndroidCreateVS](images/xamarinAndroidCreateVS.png)

If you are using Xamarin Studio, click `New Solution...` and create an Android App.

![XamarinAndroidCreateXS](images/xamarinAndroidCreateXS.png)

#### 2. Generate license key ####

Obtaining a license key is already described [here](#obtaining-an-anyline-sdk-license-key). To generate a license key for your application, refer to the `package name` of your Xamarin.Android project. 

- Visual Studio

![XamarinAndroidPackageVS](images/xamarinAndroidPackageVS.png)

- Xamarin Studio

![XamarinAndroidPackageXS](images/xamarinAndroidPackageXS.png)

#### 3. Add AnylineXamarinSDK.Droid as reference ####

To access the functionality of our SDK, simply add the `AnylineXamarinSDK.Droid.dll` file as a reference. 

Visual Studio:

- right-click the References node in your project tree
- click "Add reference..."
- click "Browse..." and locate the AnylineXamarinSDK.Droid.dll file

Xamarin Studio:

- right-click the References node in your project tree
- click "Edit references..."
- navigate to the ".Net Assembly" tab
- click "Browse..." to locate the AnylineXamarinSDK.Droid.dll file

The SDK should now be visible as a reference and your project tree should somewhat look like this:

![XamarinAndroidReference](images/xamarinAndroidReference.png)

#### 4. Set minimum Android target ####

In the `Application` settings of your project, set the minimum version of your Android target to `API level 17`.

![XamarinAndroidTarget](images/xamarinAndroidTarget.png)

#### 5. Add necessary permissions and features ####

The scanning activity needs the following permissions and features

- Permissions
	- CAMERA
	- VIBRATE
	- WRITE_EXTERNAL_STORAGE
	- READ_EXTERNAL_STORAGE
- Features
	- android.hardware.camera
	- android.hardware.camera.flash
	- android.hardware.camera.autofocus

So let's add them in our AndroidManifest.xml in our Properties node.

> in AndroidManifest.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
  package="AT.Nineyards.AnylineXamarinExample"
  android:versionCode="1" android:versionName="1.0">
	<uses-sdk />
	<application android:label="AnylineExampleAndroid" android:icon="@drawable/Icon"></application>
  <uses-permission android:name="android.permission.CAMERA" />
  <uses-permission android:name="android.permission.VIBRATE" />
  <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" android:maxSdkVersion="18" />
  <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" android:maxSdkVersion="18" />
  <uses-feature android:name="android.hardware.camera" android:required="false" />
  <uses-feature android:name="android.hardware.camera.flash" />
  <uses-feature android:name="android.hardware.camera.autofocus" android:required="false" />
</manifest>
```
###### &NewLine;

#### 6. Add a Scan View ####

- Open the Main.axml file in your Resources/Layout node.
- Switch from the `Design` tab to the `Source` tab.
- Edit the XML Code as shown in the code example

> The XML code should look somewhat like this

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent">
    <Button
        android:id="@+id/MyButton"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:text="@string/Hello" />
</LinearLayout>
```
###### &NewLine;

> Remove the Button and add a scan view instead. Edit the XML code as follows

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent">
    <at.nineyards.anyline.modules.barcode.BarcodeScanView
        android:id="@+id/AnylineScanView"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />
</LinearLayout>
```
###### &NewLine;

If you switch back from to the `Design` tab, the user interface should look like this:

![XamarinAndroidDesign](images/xamarinAndroidDesign.png)

You now have added a Barcode scan view to your activity. However, depending on which scanning type you want to implement, choose one of these module-specific views:

- Barcode scanning
	- at.nineyards.anyline.modules.barcode.BarcodeScanView
- Energy meter scanning
	- at.nineyards.anyline.modules.energy.EnergyScanView
- MRZ and passport scanning
	- at.nineyards.anyline.modules.mrz.MrzScanView

Because we named the element "AnylineScanview" (`android:id="@+id/AnylineScanView"` in the XML code), this element is added automatically to the Resource.Designer.cs class when the project is built. 

#### 7. Provide a configuration to the scan view ####

You can adapt your scan view easily by either adding a .json file or using XML-attributes in the layout file (e.g. Main.axml).
In this example, let's create a .json file and fill it with properties:

Visual Studio:

- In your Assets treenode, right-click on the folder and select `Add` > `New Item..`
- Choose `Visual C#` > `Text File` and name it "scanConfig.json"

Xamarin Studio:

- In your Assets treenode, right-click on the folder and select `Add` > `New File...`
- Choose `Misc` > `Empty Text File` and name it "scanConfig.json"

Make sure that the build action for this file is `AndroidAsset`.

Now open the file and paste the code from the following example

```json
{
  "captureResolution":"720p",

  "cutout": {
    "style": "rect",
    "maxWidthPercent": "80%",
    "alignment": "top_half",
    "ratioFromSize": {
      "width": 100,
      "height": 80
    },
    "strokeWidth": 4,
    "cornerRadius": 10,
    "strokeColor": "00AFFE",
    "outerColor": "000000",
    "outerAlpha": 0.3
  },
  "flash": {
    "mode": "manual",
    "alignment": "bottom_right"
  },
  "beepOnResult": false,
  "vibrateOnResult": true,
  "blinkAnimationOnResult": true,
  "cancelOnResult": false
}
```
###### &NewLine;

Follow [this guide](#anyline-config) for detailed information about the configuration.

#### 8. Implement the scan view ####

Now we are ready to implement the scan view in our activity. Add a `using` statement for our module-specific scan view in our `MainActivity.cs`.

```csharp
// for barcode scanning
using AT.Nineyards.Anyline.Modules.Barcode;

// for energy meter scanning
using AT.Nineyards.Anyline.Modules.Energy;

// for MRZ scanning
using AT.Nineyards.Anyline.Modules.MRZ;
```
###### &NewLine;

Have a look at the code outline of our activity in the following code example:

> Activity outline

```csharp
namespace AnylineExampleAndroid
{
    [Activity(Label = "AnylineExampleAndroid", MainLauncher = true, Icon = "@drawable/icon")]
    public class MainActivity : Activity
    {
		// A member variable that is our scan view
        BarcodeScanView scanView;
		
		// Setup our scan view here
        protected override void OnCreate(Bundle bundle) { ... }
        		
		// If the application state passes from "Resumed" to "Paused", this method is invoked
		// scanView.CancelScanning(); and scanView.ReleaseCameraInBackground(); should be called here
        protected override void OnPause() { ... }
		
		// This method is invoked if the application state changes from "Paused" or "Started" to "Resumed"
		// scanView.StartScanning(); should be called here
        protected override void OnResume() { ... }
		
		// If the back-button is pressed, invoke this method
		// To close your application, call the Finish(); method here
        public override void OnBackPressed() { ... }
    }
}
```
###### &NewLine;

The following methods have to be implemented in order to use the scan view:

- in `OnCreate` we'll set up our scan view, load a configuration, initialize it with the license key that we generated earlier.

> OnCreate

```csharp
// Setup our scan view here
protected override void OnCreate(Bundle bundle)
{
	base.OnCreate(bundle);

	// Set our view from the "main" layout resource
	SetContentView(Resource.Layout.Main);
				
	// allocate the element we defined earlier in our Main.axml
	scanView = FindViewById<BarcodeScanView>(Resource.Id.AnylineScanView);

	// Load config from the .json file
	// We don't need this, if we define our configuration in the XML code
	scanView.SetConfigFromAsset("scanConfig.json");

	// Initialize with our license key and our result listener
	// Important: use the license key for your app package
	scanView.InitAnyline(<INSERT YOUR LICENSE KEY HERE>, this);
	
	// Don't stop scanning when a result is found
	scanView.SetCancelOnResult(false);

	// Register event that shows if the camera is initialized
	scanView.CameraOpened += (s, e) => { 
		Log.Debug("Camera", "Camera opened successfully. Frame resolution " + e.Width + " x " + e.Height);
	};
	
	// Register event that shows if the camera returns an error
	scanView.CameraError += (s, e) => {
		Log.Error("Camera", "OnCameraError: " + e.Event.Message);
	};
}
```
###### &NewLine;

<aside class="notice">
<b>InitAnyline</b> takes the license key and a module-specific result listener as arguments. A convenient way is to store the license key in a <b>const String</b> and let the activity implement the interface of the result listener, then passing <b>this</b> as second argument.
</aside>
###### &NewLine;

- In `OnResume`, we finally start scanning.

> OnResume

```csharp
// This method is invoked if the application state changes from "Paused" or "Started" to "Resumed"
protected override void OnResume()
{
	base.OnResume();
	scanView.StartScanning();
}
```
###### &NewLine;

#### 9. Handle Results ####

Each of the anyline modules uses a module-specific scan view that uses the camera of your mobile device and provides real-time scan results via a module-specific listener.

The easiest way to make use of that listener is to implement the interface of the listener in your scan activity like this:

> Implement a Result Listener depending on the module (IBarcodeResultListener, IEnergyResultListener or IMrzResultListener)

```csharp
[Activity(Label = "AnylineExampleAndroid", MainLauncher = true, Icon = "@drawable/icon")]
public class MainActivity : Activity, IBarcodeResultListener // or any other module-specific listener, depending on your scan module
{
	...
}
```
###### &NewLine;  

The interface can be implemented by right-clicking on the ResultListener Interface and selecting `Implement Interface`. A method will be generated that is called each time our scan view is scanning and reporting a result.

> OnResult of the Barcode ResultListener

```csharp
// Each time, a result is found, this method is invoked
public void OnResult(string result, AT.Nineyards.Anyline.Models.AnylineImage resultImage)
{
	// Show a short Toast message if a result is found
	Toast.MakeText(this, result, ToastLength.Short).Show();
}
```
###### &NewLine;

&nbsp;

#### 10. Test your app on an Android device ####

Build your project and deploy your application on your Android device. For module-specific documentation, please check [this section](#modules) for more information. Enjoy scanning and have fun :)

## Xamarin.iOS ##

The Anyline Xamarin SDK provides the functionality of our iOS SDK in C# to make iOS integration in Xamarin as easy as possible. This section provides a step-by-step guide to set up a Xamarin.iOS project in Visual Studio and start scanning right away!

### Requirements ###

To use the Anyline SDK for Xamarin.iOS, you'll need:

- Xamarin account (For Visual Studio, you need at least a Xamarin business account. Check out the <a href="https://xamarin.com/">Xamarin website</a> for detailed information.)
- An Anyline license which you can generate <a href="http://portal.anyline.io/">here</a> after purchasing our SDK in the <a href="https://www.anyline.io/store/">store</a>.
- An iOS device
	- Minimum iOS 7.0
	- Minimum iPhone4s
- A Mac Build Host (For more information, please check out <a href="http://developer.xamarin.com/guides/ios/getting_started/installation/windows/">this guide</a> on how to install Xamarin.iOS on Windows.
- An Apple developer account and your device set up for development: Please check out <a href="http://developer.xamarin.com/guides/ios/getting_started/installation/device_provisioning/">this guide</a> for detailed information.

The iOS bundle contains the following parts:

- **AnylineXamarinSDK.iOS.dll:** contains the Anyline SDK Library
- **AnylineResources.bundle** contains the necessary resources for your project
- **AnylineXamarinApp.iOS:** contains a simple app where each module is implemented - it can be installed right away
- **LICENSE:** third party license agreements
- **README:** contains a quick start - setup

### Quick Start - Setup ###

In this section we will go through the basic process of creating and configurating a simple Anyline scanning application in Xamarin.iOS. If you are not yet familiar with Xamarin.iOS, follow <a href="https://developer.xamarin.com/guides/ios/getting_started/">this</a> guide to develop an understanding of the fundamentals of iOS application development with Xamarin.


#### 1. Create a new Xamarin.iOS Project ####

In Visual Studio, click `File` > `New Project...`, select `Visual C#` > `iOS` > `iPhone` and create a new Single View App for iPhone.

![XamariniOSCreate](images/xamariniOSCreate.png)

Make sure your build host is up and running and pair Visual Studio with the build host. If you have trouble with this step, please refer to <a href="https://developer.xamarin.com/guides/ios/getting_started/installation/windows/troubleshooting/">this guide</a>.

###### &NewLine;

#### 2. Generate license key ####

To generate a license key for your application, refer to the `bundle identifier` of your Xamarin.iOS project. Further information about getting a license key is described [here](#obtaining-an-anyline-sdk-license-key). 

![XamariniOSBundle](images/xamariniOSBundle.png)

#### 3. Add Anyline SDK ####

In the project view, right-click on the `References` node, select `Add Reference...` and locate the AnylineXamarinSDK.iOS.dll.

When added, the SDK is visible in the solution explorer as follows:

![XamariniOSSDK](images/xamariniOSSDK.png)

###### &NewLine;

#### 4. Add AnylineResources.bundle ####

Simply drag and drop the AnylineResources.bundle folder into the `Resources` node of your project. The project structure should now look like this:

![XamariniOSResource](images/xamariniOSResource.png)

###### &NewLine;
###### &NewLine;

#### 5. Implement the Anyline Scan View ####

Because we created a Single View App, Visual Studio automatically created a Storyboard and a View Controller for us. By default, the Storyboard is named `MainStoryboard.storyboard` and the View Controller is named `RootViewController`.

Let's have a look at the code structure of the `RootViewController`.

- In *ViewDidLoad*, we'll set up the Scan View and add it to the View of the ViewController.
- In *ViewDidAppear*, we'll start scanning for results.
- In *ViewWillDissapear*, we'll stop scanning.
- In *ViewDidDissapear*, we'll unregister any event handlers and dispose/remove our Scan View so there are no circular dependencies left.

> RootViewController.cs

```csharp
using System;
using System.Drawing;

using Foundation;
using UIKit;

namespace AnylineExampleiOS
{
    public partial class RootViewController : UIViewController
    {
        public RootViewController(IntPtr handle) : base(handle) { ... }
        
        public override void DidReceiveMemoryWarning() { ... }
        
        public override void ViewDidLoad() { ... }
        
        public override void ViewWillAppear(bool animated) { ... }
        
        public override void ViewDidAppear(bool animated) { ... }
        
        public override void ViewWillDisappear(bool animated) { ... }
        
        public override void ViewDidDisappear(bool animated) { ... }
        
    }
}
```
###### &NewLine;

To access the classes and methods of the Anyline SDK, simply add the following `using` statement.

```csharp
using AnylineXamarinSDK.iOS;
```
###### &NewLine;
###### &NewLine;

##### Create and set up a Scan View #####

There are three module-specific Scan Views available:

- **AnylineBarcodeModuleView** for barcode scanning
- **AnylineEnergyModuleView** for power meter and gas meter scanning
- **AnylineMRZModuleView** for MRZ and passport scanning

In this example, we'll implement a *Barcode* scan.

Create a member variable `scanView` of the type of the module-specific view that you want to implement. 
In this case: `AnylineBarcodeModuleView scanView;`

Have a look at the `ViewDidLoad` method on how to set up the Scan View.

Basically, a new instance of our module-specific view (in this case: *AnylineBarcodeModuleView*) is created and a frame in which the view is rendered is delivered.

`SetupWithLicenseKey` is called with the following parameters:

- The license key of **your** personal bundle identifier
- The delegate that will listen to the results (in this case: the `RootViewController`, because we'll let it implement the delegate of our module-specific view)
- An error which is filled with an error description if the setup fails (for example, when the license key is wrong).

If anything goes wrong in the setup, the error description is presented to the user in form of an alert view.

Finally, the scan view is added as a subview of our `RootViewController.View`.

```csharp
public override void ViewDidLoad()
{
	base.ViewDidLoad();

	//initialize the scan view with a frame
	scanView = new AnylineBarcodeModuleView(UIScreen.MainScreen.Bounds);

	//setup the scan view with your license key, the delegate (this) that handles the results and store any possible error in a variable
	NSError error;
	bool success = scanView.SetupWithLicenseKey("YOUR LICENSE KEY HERE", this, out error);

	//show an alert if something went wrong
	if (!success)
	{
		new UIAlertView("Error", error.DebugDescription, null, "OK", null).Show();
	}

	// After the setup is done we add the module to the view of this view controller
	View.AddSubview(scanView);
}
```
###### &NewLine;

In the `ViewDidAppear` method, we'll tell the SDK to start scanning. The 

```csharp
public override void ViewDidAppear(bool animated)
{
	base.ViewDidAppear(animated);
	/*
	 This is the place where we tell Anyline to start receiving and displaying images from the camera.
	 Success/error tells us if everything went fine.
	 */
	NSError error;
	bool success = scanView.StartScanningAndReturnError(out error);
	
	if (!success)
	{
		// Something went wrong. The error object contains the error description
		new UIAlertView(@"Start Scanning Error", error.DebugDescription, null, "OK", null).Show();
	}
}
```
###### &NewLine;

#### 6. Handle Scan Results ####

To handle scan results, our `RootViewController` has to implement the interface of a module-specific view delegate. Simply modify the class header signature as shown in the code.

```csharp
// for barcode scans, implement IAnylineBarcodeModuleDelegate
public partial class RootViewController : UIViewController, IAnylineBarcodeModuleDelegate
{
	...
}

// for energy meter scans, implement IAnylineEnergyModuleDelegate
public partial class RootViewController : UIViewController, IAnylineEnergyModuleDelegate
{
	...
}

// for MRZ and passport scans, implement IAnylineMRZModuleDelegate
public partial class RootViewController : UIViewController, IAnylineMRZModuleDelegate
{
	...
}
```
###### &NewLine;

Right-click on the Interface and choose `Implement Interface` > `Implement Interface`.

![XamariniOSInterface](images/xamariniOSInterface.png)

Visual Studio automatically generates the `DidFindScanResult` method of the module-specific delegate. This method is called every time a result is found.

The following code shows an implementation where the result is presented in an `UIAlertView`.

<aside class="notice">
Because barcode scanning is too fast, the <b>DidFindScanResult</b> event is not fired when the same barcode is scanned again, even if <b>scanView.CancelOnResult</b> is set to false.
</aside>

```csharp
public void DidFindScanResult(AnylineBarcodeModuleView anylineBarcodeModuleView, string scanResult, UIImage image)
{
	new UIAlertView(@"Result", scanResult, null, "OK", null).Show();	
}
```
###### &NewLine;

#### 7. A few more things ####

* Make sure that scanning is stopped before the view appears.

* If you use a **community edition license** and any visual element is drawn over the Anyline watermark while scanning, an exception is thrown!

* Xamarins Garbage Collecter has a few problems, which means that there might be memory leaks if there are any dependencies left in the `ViewController`. Therefore, make sure that all registered events are unregistered and any subview is removed from the ViewController view and disposed.

> ViewWillDisappear

```csharp
public override void ViewWillDisappear(bool animated)
{
	base.ViewWillDisappear(animated);

	// We have to stop scanning before the view dissapears
	error = null;
	if (!scanResult.CancelScanningAndReturnError(out error))
	{
		(alert = new UIAlertView("Error", error.DebugDescription, null, "OK", null)).Show();
	}
}
```

> ViewDidDisappear

```csharp
public override void ViewDidDisappear(bool animated)
{
	base.ViewDidDisappear(animated);

	//un-register any event handlers here, if you have any
	
	//we have to erase the scan view so that there are no dependencies for the viewcontroller left.
	scanResult.RemoveFromSuperview();
	scanResult.Dispose();
	scanResult = null;

	base.Dispose();
}
```
###### &NewLine;

#### 10. Test your app on an iOS device ####

Build your project and deploy your application on your iOS device. Make sure you choose your storyboard and an adequate deployment target in your Info.plist (>= 7.1, but not higher than your device target).

For module-specific documentation, please check [this section](#modules) for more information. Enjoy scanning and have fun :)

## Xamarin.WinPhone ##

Anyline will be available Windows Phone by Q4 2015.