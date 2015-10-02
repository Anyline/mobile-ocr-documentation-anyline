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

Bitcode needs to be disabled. Just search for "*Bit*" and set *Enable Bitcode* to *No*.

> Bitcode is an intermediate representation of a compiled program. Apps you upload to iTunes Connect that contain bitcode will be compiled and linked on the App Store. Including bitcode will allow Apple to re-optimize your app binary in the future without the need to submit a new version of your app to the store.

![Linker Flags](images/iOS_build_bitcode.png)

[Apple Documentation on Bitcode](https://developer.apple.com/library/prerelease/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AppThinning/AppThinning.html)

####4. Init an AnylineModuleView in your ViewController or Storyboard

There are module specific options - take a look at the description of the desired module to get more detailed information.

####5. Enjoy scanning and have fun :)
