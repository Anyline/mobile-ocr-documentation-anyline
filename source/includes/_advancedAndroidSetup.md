# Android Advanced Setup

## How to Split APK into several smaller ones

It is possible to split your app into multiple APKs and upload them all to the playstore.
The playstore will pick the correct APK to download.
This is currently not recommended by Google unless the APK is bigger than 50MB. (see: [Google developer page on multiple APKs](http://developer.android.com/google/play/publishing/multiple-apks.html))
The following can be added in the android section of the build.gradle
to generate a separate APK for every supported CPU architecture (ABI).

```groovy
    //at the end of the android section
    productFlavors {
        x86 {
            flavorDimension "abi"
            ndkConfig.abiFilters "x86"
            // this is the flavor part of the version code.
            // It must be higher than the arm one for devices supporting
            // both, as x86 is preferred.
            versionCode = 4
        }
        armv7 {
            flavorDimension "abi"
            ndkConfig.abiFilters "armeabi-v7a"
            versionCode = 3
        }

        arm {
            flavorDimension "abi"
            ndkConfig.abiFilters "armeabi"
            versionCode = 2
        }

        mips {
            flavorDimension "abi"
            ndkConfig.abiFilters "mips"
            versionCode = 1
        }
        fat {
            flavorDimension "abi"
            // fat binary, lowest version code to be
            // the last option
            versionCode = 0
        }

        // make per-variant version code
        applicationVariants.all { variant ->
            // get the version code of each flavor
            def abiVersion = variant.productFlavors.get(0).versionCode

            // set the composite code
            variant.mergedFlavor.versionCode = defaultConfig.versionCode * 10 + abiVersion
        }
    }
```

This example will generate 5 APKs. 4 for every supported ABI and 1 big APK with everything.
The APKs need to have a different version, for the playstore to be able to deliver the correct one.
The playstore chooses the highest compatible version so *armeabi-v7a* must be higher than *armeabi*
and *x86* must be higher than *armeabi-v7a* (because x86 devices can usually also use *armeabi* libraries).


The versioning in this example is a bit different than the one suggested on different places like [this site from Intel](https://software.intel.com/en-us/android/articles/google-play-supports-cpu-architecture-filtering-for-multiple-APK).
Because it is not optimal that an old x86 version has a higher number than any newer other version,
this could be problematic if you change from multiple APKs to a single APK later on.

The example shows one possibility, you can also use 3 APKs: *x86*, *armeabi-v7a*, and one with *mips* + *armeabi*.

The splitting has advantages during development, because you can chose the product flavor you want to use
and that reduces the build and upload time a bit.
