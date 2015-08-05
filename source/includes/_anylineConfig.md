# Anyline Config

With the config file the views can be configured in regard of position and looks of the *cutout* used for scanning as well as the behavior of flash and feedback mechanisms for scanning. This section contains a detailed description of the configurable items as well as two distinct ways for each platform to set up and configure the view.

<a name="itemDescription"> </a>
## Item description

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

* [captureResolution] (#captureResolution)  *String*
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
* [beepOnResult] (#beepOnResult)  *boolean*
* [vibrateOnResult] (#vibrateOnResult)  *boolean*
* [blinkAnimationOnResult] (#blinkAnimationOnResult)  *boolean*
* [cancelOnResult] (#cancelOnResult)  *boolean*


### captureResolution

The resolution of the preview frame.

**Possible values:**

value | description
----- | -----------
1080p |	use 1920x1080 as preferred preview size
720p  | use 1280x720 as preferred preview size
480p  |	use 480x854 as preferred preview size


 - The p is optional. 
 - The defined capture resolution is just the preferred resolution. If this resolution is not available the resolution that best fits inside the desired resolution should be used.
 
<a name="captureResolution2"> </a>

<aside class="notice">
<b>Android specific</b>
<br/>
The capture resolution is more like a suggestion, because the available preview sizes vary from device to device. 
If the "suggested" resolution is available, it will be used, if not a resolution that fits best within the views size will be used.
</aside>
 
 
<a name="cutout"></a>
###cutout

This contains all the settings for the overlay/cutout.


<a name="cutout_width"></a>
#### width 

Specifies an exact desired pixel width (pixels in the frame, not on the display). If this is bigger than the width of the view, it will be limited to it. 

E.g. Android devices may have a 720p preview but only a 540p wide display. If a width of 600 is specified, the cutout will still only be 540. 

- **Type:** 		int
- **Default:** 		none

<a name="cutout_maxWidthPercent"></a>
#### maxWidthPercent

Specifies the maximum width of the cutout in percent.

This is the desired width if 'width' parameter is not used.

- **Type:**			String
- **Format:**		###%
- **Range:**		1% - 100%
- **Default:**		100%

<a name="cutout_maxHeightPercent"></a>
####maxHeightPercent
 
Specify the maximum height of the cutout in %.

- **Type:**			String
- **Format:**		###%
- **Range:**		1% - 100%
- **Default:**		100%

<aside class="notice">
<b>Why width and maxWidth and maxHeight but no height?</b>
<br/>
The form of the cutout can be defined by the ratio, so there is no direct need for width AND height parameter.
But if there is only one width parameter where % or pixel value is allowed, it is not possible to limit the height to a certain percentage.
That limits the usability in landscape mode quite a bit.
</aside>

<a name="cutout_ratioFromSize"></a>
####ratioFromSize
The ratio of the cutout can be defined by just setting this parameters width and height to the size of the real object that should be
scanned.

E.g. width = 100 height = 50 is the same as width = 200 height = 100, because only the ratio matters here.

- **Type of width and height:** int
- **Default ratio:** 			1 (width = 1, height = 1)

<a name="cutout_alignment"></a>	
####alignment
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
####offset
Move the cutout in x and y direction by the specified pixel value. 

Put negative values to move left or up, positive values to move right or down.

The offset is limited to move the cutout to the start/end of the view.
 
- **Type:** int (pixel value = pixels in the frame, not on the display)

<a name="cutout_outerColor"></a>
####outerColor
The color of the area outside the cutout.

- **Type:** Hex-String 
- **Format:** RRGGBB

<a name="cutout_outerAlpha"></a>
####outerAlpha
The alpha value for the color outside of the cutout.

- **Type:** float
- **Range:** 0-1 
   - 0 = view is completely transparent
   - 1 = view is completely opaque

<a name="cutout_style"></a>
####style
The type of the cutout.

style | description | related parameters
----- | ----------- | -------------------
rect | a rectangle is drawn around the cutout-area | strokeWidth, strokeColor, cornerRadius
image | a given image is drawn inside the cutout-area (resized to fit) | image

<a name="cutout_strokeColor"> </a>
####strokeColor
The color of the stroke (used by rect or vector style).

- **Type:** Hex-String 
- **Format:** RRGGBB

<a name="cutout_strokeWidth"> </a>
####strokeWidth
The thickness of the stroke in a display independent unit 

- **Type:** int (density-independent pixel dp)

<a name="cutout_cornerRadius"> </a>
####cornerRadius
The radius to round the corners of the rect (only used if type is rect).

 - **Type:** int (density-independent pixel dp)

<a name="cutout_image"> </a>
####image
The name of the image resource (only used if type is image).

- **Type:** String

<a name="cutout_cropPadding"> </a>
####cropPadding
Adapts the size of the cropped area from the frame. It changes the visible cutout width and height by the x or y padding.

Positive values mean that the crop will be smaller than the cutout.  Negative values mean that the crop will be bigger than the cutout.

The padding is all around the cutout so cropWidth = cutoutWidth - 2 * xPadding.

This should only be used in combination with a fixed width (which also fits on all supported devices), because this is a fixed value that will not be adjusted to anything.

- **Type:** int (pixel value)

<a name="cutout_cropOffset"> </a>
####cropOffset
Move the part that is cropped out of the frame away from the visible cutout by the x and y.

Negative values to move left or up, positive values to move right or down.

This should only be used in combination with a fixed width (which also fits on all supported devices), because this is a fixed value that will not be adjusted to anything.

- **Type:** int (pixel value)

<a name="flash"> </a>
###flash
Settings for a simple view that provides flash functionality.

<a name="flash_mode"> </a>
####mode

Default images for flash on/off/auto are provided.

**Possible values:** 

mode | description 
---- | ------------
none | flash view is not used and not visible
manual | flash can be toggled manually  (default is off)
auto | flash view also has an automatic option. But it is still required to tell the view when it should turn on the flash in auto mode. (Except for use-cases where additional abstraction is provided like the Energy-Module)

<a name="flash_alignment"> </a>
####alignment
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
####imageOn
The name of the custom image resource to show when the flash is on.

The image will not be resized and should be provided in an appropriate size.

- **Type:** String

<a name="flash_imageOff"> </a>
####imageOff
The name of the custom image resource to show when the flash is off.

The image will not be resized and should be provided in an appropriate size.

- **Type:** String


<a name="flash_imageAuto"> </a>
####imageAuto
The name of the custom image resource to show when the flash is in auto mode.

The image will not be resized and should be provided in an appropriate size.

- **Type:** String

###Special options for Modules

Modules can use these additional options to configure some things that should happen when a result was found.

<a name="beepOnResult"> </a>
####beepOnResult
True, if there should be a beep when a result is found.

- **Type:** boolean

<a name="vibrateOnResult"> </a>
####vibrateOnResult
True, if there should be a vibration alarm when a result is found.

- **Type:** boolean

<a name="blinkAnimationOnResult"> </a>
####blinkAnimationOnResult
True, if the view should display a short flash of white when a result is found.

- **Type:** boolean

<a name="cancelOnResult"> </a>
####cancelOnResult
True, if the scanning process should be canceled when a result is found (=default setting).

Set to false if multiple things should be scanned immediately after each other.
If another scan may be required later, leave this value *true* and just restart the scanning process later

- **Type:** boolean
- **Default:** true (cancel scanning when result is found)

<a name="androidSpecifics"> </a>
## Android Config

There are two ways to configure the look and behavior of the scan view in Android.

### 1. Configuration via json

The view can be configured using a json config file like seen above in the [item description] (#itemDescription). This file must be located in the assets folder of your Android project.

<a name="configureViaXML"> </a>
### 2. Configuration via XML

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
#####Differences to json
- tree is "flattened" and snake_case
- captureResolution is defined by preferred_preview_width and preferred_preview_height
- colors contain alpha value, so there is no extra option for alpha

<a name="iosConfig"> </a>
## iOS Config 

There are two methods to add the view for the models available.

<a name="iosConfigModuleViewCode"> </a>
### 1. Configure in Code

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

##### flashButtonAlignment
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
### 2. Configure with Storyboards
1. Select *View* from the object library
2. Drag it onto the view of your view controller
3. Change the class to the name of the module you want to use (AnylineBarcodeModuleView, AnylineMRZModuleView, AnylineEnergyModuleView)

![AddView] (images/docAddView.jpg)


##### Changing the appearance
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

## Cordova Plugin Config 
The configuration for the anyline-sdk plugin is part of the AnylineSDK call. The config is just passed as a json-array, see also the [example in the Quick Start] (#cordova-example).