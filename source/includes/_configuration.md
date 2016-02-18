<a name="configuration"> </a>
# Configuration

This section describes how to configure the scan view using [AnylineConfig] (#anyline-config) files as well as how to [add views] (#add-views) without overlapping the scan cutout. 

<a name="anyline-config"> </a>
## Anyline Config 

With the config file the views can be configured in regard of position and looks of the *cutout* used for scanning, as well as the behavior of flash and feedback mechanisms for scanning. This section contains a detailed description of the configurable items, as well as two distinct ways for each platform to set up and configure the view.

<a name="itemDescription"> </a>
### Item description

```json
{
      "captureResolution": "720",
      "pictureResolution": "1080",
      "cutout": {
          "width": 540,
          "maxWidthPercent": "80%",
          "maxHeightPercent": "80%",
          "ratioFromSize": {
              "width": 125,
              "height": 85
          },
          "alignment": "center",  
          "offset": {
              "x": 0,
              "y": 0
          },
          "outerColor": "000000",
          "outerAlpha": 0.3,
		  "style": "rect",
          "strokeColor": "FFFFFF",
		  "strokeWidth": 2,
          "cornerRadius": 4,
		  "image":"Overlay",
          "cropPadding": {
              "x": 0,
              "y": 0
          },
          "cropOffset": {
              "x": 0,
              "y": 0
          }
      },
      "flash": {
          "mode": "manual",
          "alignment": "bottom_right",
          "imageOn": "ic_flash_on",
          "imageOff": "ic_flash_off",
          "imageAuto": "ic_flash_auto",
		  "offset": {
		        "x": 10,
		        "y": -10
		  }
      },
      "beepOnResult": false,
      "vibrateOnResult": true,
      "blinkAnimationOnResult": true,
      "cancelOnResult": true
}
```

* [captureResolution] (#captureResolution)  *String*
* [pictureResolution] (#pictureResolution)  *String*
* [cutout] (#cutout) 
  * [width] (#cutout_width) *int*
  * [maxWidthPercent] (#cutout_maxWidthPercent)  *String*
  * [maxHeightPercent] (#cutout_maxHeightPercent)  *String*
  * [ratioFromSize] (#cutout_ratioFromSize) 
     * [width] (#cutout_ratioFromSize) *int*
     * [height] (#cutout_ratioFromSize)  *int*   
  * [alignment] (#cutout_alignment)  *String*
  * [offset] (#cutout_offset) 
     * [x] (#cutout_offset)  *int*
     * [y] (#cutout_offset) *int*
  * [outerColor] (#cutout_outerColor)  *Hex-String*  - format: RRGGBB
  * [outerAlpha] (#cutout_outerAlpha)  *float*
  * [style] (#cutout_style)  *String* 
  * [strokeColor] (#cutout_strokeColor)  *Hex-String* - format: RRGGBB
  * [strokeWidth] (#cutout_strokeWidth)  *int*
  * [cornerRadius] (#cutout_cornerRadius)  *int*
  * [image] (#cutout_image)  *String*
  * [cropPadding] (#cutout_cropPadding)  
     * [x] (#cutout_cropPadding)  *int*
     * [y] (#cutout_cropPadding) *int*
  * [cropOffset] (#cutout_cropOffset)  
     * [x] (#cutout_cropOffset)  *int*
     * [y] (#cutout_cropOffset) *int*
* [flash] (#flash) 
  * [mode] (#flash_mode)  *String*
  * [alignment] (#flash_alignment)  *String*
  * [imageOn] (#flash_imageOn)  *String*
  * [imageOff] (#flash_imageOff)  *String*
  * [imageAuto] (#flash_imageAuto)  *String*
  * [offset] (#flash_offset)
     * [x] (#cutout_cropOffset)  *int*
     * [y] (#cutout_cropOffset) *int*
* [beepOnResult] (#beepOnResult)  *boolean*
* [vibrateOnResult] (#vibrateOnResult)  *boolean*
* [blinkAnimationOnResult] (#blinkAnimationOnResult)  *boolean*
* [cancelOnResult] (#cancelOnResult)  *boolean*


<a name="captureResolution"> </a>
##### captureResolution

The resolution of the preview frame.

**Possible values:**

value | description
----- | -----------
1080p |	use 1920x1080 as preferred preview size
720p  | use 1280x720 as preferred preview size
480p  |	use 480x854 as preferred preview size


 - The p is optional. 
 - The defined capture resolution is just the preferred resolution. If this resolution is not available the resolution that best fits inside the desired resolution should be used.
 
<aside class="notice">
<b>Android specific</b>
<br/>
The capture resolution is more like a suggestion, because the available preview sizes vary from device to device. 
If the "suggested" resolution is available, it will be used, if not a resolution that fits best within the views size will be used.
</aside>


<a name="pictureResolution"> </a>
##### pictureResolution

The resolution of the full picture taken from the camera (optional). This parameter is only of interest when using the [Document] (#document) module.

**Possible values:**

value | description
----- | -----------
1280 | use 1280 as the minimum picture resolution
1080 |	use 1080 as the minumum picture resolution
...  | and so on

 - The p is optional (and only accepted for convenience). 
 - The defined picture resolution is just the preferred minimal picture resolution.  

<aside class="notice">
<b>Android specific</b>
<br/>
The picture resolution is more like a suggestion, because the available preview sizes vary from device to device. 
If the "suggested" resolution is available, it will be used, if not, the smallest available resolution bigger than the suggested resolution will be used.
</aside>

<aside class="notice">
<b>iOS specific</b>
<br/>
The highest possible resolution for iOS is 1080. However if you specify a higher value for picture resolution we will return a high resolution photo of the found document as result. Therefore it is also possible to specify "photo" as value if you want to get a high resolution result.
</aside>


<a name="cutout"></a>
##### cutout

This section contains all the settings for the overlay/cutout.


<a name="cutout_width"></a>
##### width 

Specifies an exact desired pixel width (pixels in the frame, not on the display). If this is bigger than the width of the view, it will be limited to the view's width. 

E.g. Android devices may have a 720p preview but only a 540p wide display. If a width of 600 is specified, the cutout will still only be 540. 

- **Type:** 		int
- **Default:** 		none

<a name="cutout_maxWidthPercent"></a>
##### maxWidthPercent

Specifies the maximum width of the cutout in percent.

This is the desired width if the 'width' parameter is not used.

- **Type:**			String
- **Format:**		###%
- **Range:**		1% - 100%
- **Default:**		100%

<a name="cutout_maxHeightPercent"></a>
#####maxHeightPercent
 
Specify the maximum height of the cutout in %.

- **Type:**			String
- **Format:**		###%
- **Range:**		1% - 100%
- **Default:**		100%

<aside class="notice">
<b>Why width, maxWidth and maxHeight but no height?</b>
<br/>
The form of the cutout can be defined by the ratio, so there is no direct need for width AND height parameter.
But if there is only one width parameter where % or pixel value is allowed, it is not possible to limit the height to a certain percentage.
That limits the usability in landscape mode quite a bit.
</aside>

<a name="cutout_ratioFromSize"></a>
#####ratioFromSize
The ratio of the cutout can be defined by just setting this parameters width and height to the size of the real object that should be
scanned.

E.g. width = 100 height = 50 is the same as width = 200 height = 100, because only the ratio matters here.

- **Type of width and height:** int
- **Default ratio:** 			1 (width = 1, height = 1)

<a name="cutout_alignment"></a>	
#####alignment
The alignment of the cutout.

**Possible values:**

value | description
----- | -----------
top | upper border of the cutout is at the upper border of the view
top_half | offsetY = (viewHeight - cutoutHeight) / 3
center | cutout is centered in the view vertically and horizontally
bottom | lower border of the cutout is at the lower border of the view
bottom_half | the same as top half just from below: offsetY = (viewHeight - cutoutHeight) / 3 * 2

<a name="cutout_offset"></a>
#####offset
Move the cutout in x and y direction by the specified pixel value. 

Put negative values to move left or up, positive values to move right or down.

The offset is limited to move the cutout to the start/end of the view.
 
- **Type:** int (pixel value = pixels in the frame, not on the display)

<a name="cutout_outerColor"></a>
#####outerColor
The color of the area outside the cutout.

- **Type:** Hex-String 
- **Format:** RRGGBB

<a name="cutout_outerAlpha"></a>
#####outerAlpha
The alpha value for the color outside of the cutout.

- **Type:** float
- **Range:** 0-1 
   - 0 = view is completely transparent
   - 1 = view is completely opaque

<a name="cutout_style"></a>
#####style
The type of the cutout.

style | description | related parameters
----- | ----------- | -------------------
rect | a rectangle is drawn around the cutout-area | strokeWidth, strokeColor, cornerRadius
image | a given image is drawn inside the cutout-area (resized to fit) | image

<a name="cutout_strokeColor"> </a>
#####strokeColor
The color of the stroke (used by rect or vector style).

- **Type:** Hex-String 
- **Format:** RRGGBB

<a name="cutout_strokeWidth"> </a>
#####strokeWidth
The thickness of the stroke in a display independent unit 

- **Type:** int (density-independent pixel dp)

<a name="cutout_cornerRadius"> </a>
#####cornerRadius
The radius to round the corners of the rect (only used if type is rect).

 - **Type:** int (density-independent pixel dp)

<a name="cutout_image"> </a>
#####image
The name of the image resource (only used if type is image).

- **Type:** String

<a name="cutout_cropPadding"> </a>
#####cropPadding
Adapts the size of the cropped area from the frame. It changes the visible cutout width and height by the x or y padding.

Positive values mean that the crop will be smaller than the cutout.  Negative values mean that the crop will be bigger than the cutout.

The padding is all around the cutout so cropWidth = cutoutWidth - 2 * xPadding.

This should only be used in combination with a fixed width (which also fits on all supported devices), because this is a fixed value that will not be adjusted to anything.

- **Type:** int (pixel value)

<a name="cutout_cropOffset"> </a>
#####cropOffset
Move the part that is cropped out of the frame away from the visible cutout by the x and y.

Negative values to move left or up, positive values to move right or down.

This should only be used in combination with a fixed width (which also fits on all supported devices), because this is a fixed value that will not be adjusted to anything.

- **Type:** int (pixel value)

<aside class="notice">
<b>Cutout and Focus</b>
<br/>
Please keep in mind that there is a relationship between the size of the cutout, and how close/far the user will hold the phone from the object. Choosing a cutout that is too large for a small object, will intuitively force the user to move closer with the phone to the object. This may result in an incorrectly set focus on some devices, as the minimal distance between the object and the camera needs to be up to ~12cm in auto-focus mode in some cases, and ~7.5cm in macro- or continuous focus mode.
</aside>

<a name="flash"> </a>
####flash
Settings for a simple view that provides flash functionality. Default images for flash on/off/auto are provided, however you can use custom images as well. 

<aside class="notice">
<b>Epson specific</b>
<br/>
The Epson BT2000 has no flash integrated. Hence, all flash config settings will be ignored
</aside>

<a name="flash_mode"> </a>
#####mode

The following three possible flash modes are available:

**Possible values:** 

mode | description 
---- | ------------
none | flash view is not used and not visible
manual | flash can be toggled manually  (default is off)
auto | flash view also has an automatic option. But it is still required to tell the view when it should turn on the flash in auto mode. (Except for use-cases where additional abstraction is provided like the Energy-Module)

<a name="flash_alignment"> </a>
#####alignment
The alignment of the flash view.

alignment | meaning
--------- | -------
top |at the top centered horizontally
top_left | at the top left corner
top_right |at the top right corner
bottom | at the bottom centered horizontally
bottom_left | at the bottom left corner
bottom_right | at the bottom right corner

<a name="flash_imageOn"> </a>
#####imageOn
The name of the custom image resource to show when the flash is on.

The image will not be resized and should be provided in an appropriate size.

- **Type:** String

<a name="flash_imageOff"> </a>
#####imageOff
The name of the custom image resource to show when the flash is off.

The image will not be resized and should be provided in an appropriate size.

- **Type:** String


<a name="flash_imageAuto"> </a>
#####imageAuto
The name of the custom image resource to show when the flash is in auto mode.

The image will not be resized and should be provided in an appropriate size.

- **Type:** String

<a name="flash_offset"></a>
#####offset
Move the flash button in x and y direction by the specified pixel value. 

Positive values in x direction move the button to the right, negative values move the button to the left.
In y direction positive values move the flash button down, negative values move it up.
 
- **Type:** int (pixel value = densitiy pixels on the display)

####Special options for Modules

Modules can use these additional settings to configure some feedback options which should happen when a result was found.

<a name="beepOnResult"> </a>
#####beepOnResult
True, if there should be a beep when a result is found.

<aside class="notice">
<b>Epson specific</b>
<br/>
The Epson BT2000 has no built-in speakers, therefore the beep will only be played when the headset is connected.
</aside>


- **Type:** boolean

<a name="vibrateOnResult"> </a>
#####vibrateOnResult
True, if there should be a vibration alarm when a result is found.

<aside class="notice">
<b>Epson specific</b>
<br/>
The Epson BT2000 has no vibration alarm integrated, therefore the 'vibrateOnResult' setting will be ignored
</aside>

- **Type:** boolean

<a name="blinkAnimationOnResult"> </a>
#####blinkAnimationOnResult
True, if the view should display a short flash of white when a result is found.

- **Type:** boolean

<a name="cancelOnResult"> </a>
#####cancelOnResult
True, if the scanning process should be canceled when a result is found (=default setting).

Set to false if multiple things should be scanned immediately after each other.
If another scan may be required later, leave this value *true* and just restart the scanning process later

- **Type:** boolean
- **Default:** true (cancel scanning when result is found)

<a name="androidSpecifics"> </a>
### Android Config

There are two ways to configure the look and behavior of the scan view in Android.

#### 1. Configuration via json

The view can be configured using a json config file like seen above in the [item description] (#itemDescription). This file must be located in the assets folder of your Android project.

<a name="configureViaXML"> </a>
#### 2. Configuration via XML

> Where app is xmlns:app="http://schemas.android.com/apk/res-auto"

```xml
app:preferred_preview_width="720"
app:preferred_preview_height="1280"
app:cutout_alignment="top"
app:cutout_style="rect"
app:cutout_outside_color="#55000000"
app:cutout_offset_x="0"
app:cutout_offset_y="160"
app:cutout_crop_padding_x="0"
app:cutout_crop_padding_y="0"
app:cutout_crop_offset_x="0"
app:cutout_crop_offset_y="0"
app:cutout_width="540"
app:cutout_max_width_percent="90"
app:cutout_max_height_percent="90"
app:cutout_ratio_from_size_width="540"
app:cutout_ratio_from_size_height="98"
app:cutout_rect_corner_radius_in_dp="4"
app:cutout_stroke_width_in_dp="2"
app:cutout_stroke_color="#FFFFFF"
app:flash_mode="manual"
app:flash_alignment="bottom_right"
app:flash_image_on="@drawable/flash_icon"
app:flash_image_off="@drawable/flash_icon_off"
app:flash_image_auto="@drawable/flash_icon_auto"
app:beep_on_result="true"
app:vibrate_on_result="true"
app:blink_animation_on_result="true"
app:cancel_on_result="true"
```

The configuration can also be done via xml instead of json.

A list of of all available xml options can be found in the example.

<a name="differencesToJson"> </a>
##### Differences to json
- tree is "flattened" and snake_case
- captureResolution is defined by preferred_preview_width and preferred_preview_height
- pictureResolution is defined by preferred_picture_width and preferred_picture_height
- colors contain alpha value, so there is no extra option for alpha

<a name="iosConfig"> </a>
### iOS Config 

There are two methods to add the view for the models available.

<a name="iosConfigModuleViewCode"> </a>
#### 1. Configure in Code

The view exposes the following properties:
 
 
value | description
----- | ----------
strokeWidth	| sets the width of the views border
strokeColor | sets the color of the views border
cornerRadius | sets the corner radius of the views border
outerColor | sets the color of the space surrounding the view
outerAlpha | sets the alpha of the space surrounding the view
flashImage	| sets image used to toggle the flash
flashButtonAlignment | sets the alignment of the flash button
flashStatus	| flag to check the status of the flash
cancelOnResult | true, if Anyline should stop scanning once a result was found
beepOnResult | true, if there should be a beep when a result was found
blinkOnResult | true, if there should be a blinking alarm when a result was found
vibrateOnResult	| true, if there should be a vibration alarm when a result was found

###### flashButtonAlignment
specifies the alignment of the flash button

**Possible values:**

alignment | description 
--------- | -----------
ALFlashAlignmentTop | aligned at top, centered horizontally
ALFlashAlignmentTopLeft | aligned at top left corner
ALFlashAlignmentTopRight | aligned at top right corner
ALFlashAlignmentBottom | aligned at bottom, centered horizontally
ALFlashAlignmentBottomLeft | aligned at bottom left corner
ALFlashAlignmentBottomRight | aligned at bottom right corner

<a name="iosConfigModuleViewStoryboard"> </a>
#### 2. Configure with Storyboards
1. Select *View* from the object library
2. Drag it onto the view of your view controller
3. Change the class to the name of the module you want to use (AnylineBarcodeModuleView, AnylineMRZModuleView, AnylineEnergyModuleView)

![AddView] (images/docAddView.jpg)


###### Changing the appearance
The appearance of the view can be changed in the attributes inspector.

![ConfigView] (images/docConfigView.jpg)

###### Creating the property

> Module Outlet

```objective_c
@interface MainViewController : UIViewController
 
@property (weak, nonatomic) IBOutlet AnylineBarcodeModuleView *moduleView;
 
@end
```

Define the outlet of the view in the interface of the view controller.

###### Connecting the module view to your view controller
Drag the connector of you property to the newly created view. 

![Connect] (images/docConnect.jpg)

### Cordova Plugin Config 
The configuration for the anyline-sdk plugin is part of the AnylineSDK call. The config is just passed as a json-array, see also the [example] (#cordova-example) in the Quick Start.

<a name="add-views"> </a>
## Add Views

The SDK provides a function to determine the position and outline of the cutout view. This function should be used when placing views on top of the module ScanView in order to avoid an overlap with the cutout.
In the Community Edition of the SDK a watermark is displayed. Depending on the cutout size, this watermark will either be displayed within the cutout, or with respect to the cutout position, directly underneath or above the cutout. In the second case, the watermark view has to be considered when adding other views, since an overlap of the watermark will result in an exception. 


### Android


```java
RelativeLayout rl =  (RelativeLayout) findViewById(R.id.layout);

// get the cutout rect from the scan view
Rect cutoutRect = scanView.getCutoutRect();

// create a view
TextView textView = new TextView(context, null, 0);
textView.setText("Result");

// set layout parameters
int height = (int) TypedValue.applyDimension(TypedValue.COMPLEX_UNIT_SP, 28, getResources().getDisplayMetrics());
LayoutParams params = new LayoutParams(LayoutParams.WRAP_CONTENT, height);
params.leftMargin = cutoutRect.left;

// Converting dps to pixels
float dp = 40f;
float scale = getContext().getResources().getDisplayMetrics().density;
int pixels = Math.round(dp * scale);
params.topMargin = cutoutRect.top-pixels;

// add view to layout
rl.addView(textView, params);
```

This example shows how to add a view above the cutout. With the function getCutoutRect you get a Rect representing the four corners (left, top, right, bottom) of the cutout. 

We add a TextView where the left margin of the view will be the same as the left margin of the cutout. The top border of the TextView will be 40 pixels above the cutout. It is important to consider the height of the TextView and reduce this height value from the top value of the cutout rect. Furthermore it is recommended to work with density independent pixels to avoid large differences when displayed on devices with various display densities. 


######Consider the Watermark in Community Edition

```java
Rect watermarkRect = scanView.getWatermarkRect();

// create a view
TextView textView = new TextView(context, null, 0);
textView.setText("Result");

// set layout parameters
LayoutParams params = new LayoutParams(LayoutParams.WRAP_CONTENT, LayoutParams.WRAP_CONTENT);

// Converting dps to pixels
float dp = 8f;
float scale = getContext().getResources().getDisplayMetrics().density;
int marginPx = Math.round(dp * scale);

params.topMargin = watermarkRect.bottom + marginPx;
params.gravity = Gravity.CENTER_HORIZONTAL;

// add view to layout
rl.addView(textView, params);
```

When working with the community edition and small cutouts the watermark will usually be placed underneath the cutout and not included in the cutout view itself. In this case you have to consider the watermark as well when placing a view under the cutout. In case the cutout is placed on the bottom of the view, the watermark will be displayed above the the cutout and you have to be careful when placing views above the cutout. 


The example shows how to use the getWatermarkRect function to add a TextView with a top margin of 8dp under the watermark view

<aside class="notice">
<b>Use functions postDelayed</b>
<br/>
Both functions can only be used after the ScanView is drawn, therefore use postDelayed otherwise the Rect will be null.
</aside>


### iOS

```objective_c
// get the cutout rect from the scan view
CGRect cutOutRect = [self.moduleView cutoutRect];
CGFloat lowerBorder = CGRectGetMaxY(cutOutRect);
        
// create a view
UILabel * label = [[UILabel alloc] initWithFrame:CGRectMake(cutOutRect.origin.x,
                                                            lowerBorder,
                                                            cutOutRect.size.width,
                                                            cutOutRect.size.height)];
label.backgroundColor = [UIColor grayColor];
                                                            
// add view to layout
[self.moduleView addSubview:label];
```

This example shows how to add a view below the cutout view. With the function `cutoutRect` you get a CGRect representing the cutout. `CGRectGetMaxY` adds its y origin and its height to get the lower border of the cutout.

######Consider the Watermark in Community Edition

```objective_c
// get the cutout rect from the scan view
CGRect cutOutRect = [self.ocrModuleView cutoutRect];
CGRect watermarkRect = [self.ocrModuleView watermarkRect];    

CGFloat lowerBorder = MAX(CGRectGetMaxY(cutOutRect), CGRectGetMaxY(watermarkRect));

// create a view
UILabel * label = [[UILabel alloc] initWithFrame:CGRectMake(cutOutRect.origin.x,
                                                            lowerBorder,
                                                            cutOutRect.size.width,
                                                            cutOutRect.size.height)];
label.backgroundColor = [UIColor grayColor];

// add view to layout
[self.ocrModuleView addSubview:label];
```

Notice that you will get an exception when you overlay the watermark. Since the watermark will be displayed outside the cutout when the cutout size is to small, you have to consider both views when placing custom views around the cutout. 
In this example we add a view below the watermark. Therefore, we have to check which view has the largest y coordinate to avoid an overlap. With the function `cutoutRect` you get a CGRect representing the cutout. The function `watermarkRect` returns the bounding rect of the watermark. `CGRectGetMaxY` determines the y origin plus height to get the lower border of the view. With `MAX` you then get the maximum y coordinate of both rects and can use this value to place your view below both views. In case a small cutout is placed at the bottom edge of the display the watermark will be placed above the cutout. Please also consider this edge case when placing your views around the cutout.