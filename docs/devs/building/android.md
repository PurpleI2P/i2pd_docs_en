Building on Android
===================

There are two versions: with QT and without QT.

Pre-requisites
--------------

You need to install Android SDK and NDK. For QT version, you also need QT with android support.

- [SDK](https://developer.android.com/studio/index.html) (choose command line tools only, and make sure you installed "Android SDK Build-Tools")
- [NDK](https://developer.android.com/ndk/downloads/index.html)
- [QT](https://www.qt.io/download-open-source/)(for QT only).
  Choose one for your platform for android. For example QT 5.6 under Linux would be [this file](http://download.qt.io/official_releases/qt/5.6/5.6.1-1/qt-opensource-linux-x64-android-5.6.1-1.run)

You also need Java JDK (prefer Oracle Java 8), Ant and [latest Gradle](https://gradle.org/releases/) (min version 3.4).

QT-Creator (for QT only)
------------------------

Open QT-creator that should be installed with QT.
Go to Settings/Android and specify correct paths to SDK and NDK.
If everything is correct you will see two set available:
Android for armeabi-v7a (gcc, qt) and Android for x86 (gcc, qt).

Dependencies
------------

Take following pre-compiled binaries from PurpleI2P's repositories.

	git clone https://github.com/PurpleI2P/Boost-for-Android-Prebuilt.git -b boost-1_72_0
	git clone https://github.com/PurpleI2P/OpenSSL-for-Android-Prebuilt.git
	git clone https://github.com/PurpleI2P/MiniUPnP-for-Android-Prebuilt.git
	git clone https://github.com/PurpleI2P/android-ifaddrs.git

Prepare Android SDK and install required packages

	mkdir android-sdk
	cd android-sdk
	wget -t0 <link to latest SDK from Android site>
	unzip commandlinetools-XXXXXX-XXXXXX.zip -d cmdline-tools
	./cmdline-tools/tools/bin/sdkmanager "build-tools;25.0.3" "platforms;android-14" "platforms;android-25" "platform-tools"

Building the app with QT
------------------------

- Open `qt/i2pd_qt/i2pd_qt.pro` in the QT-creator
- Change line `MAIN_PATH = /path/to/libraries` to an actual path where you put the dependencies to
- Select appropriate project (usually armeabi-v7a) and build
- You will find an .apk file in `android-build/bin` folder

Building the app without QT
---------------------------

- Change line `I2PD_LIBS_PATH` in `android/jni/Application.mk` to an actual path where you put the dependencies to
- Create or edit file 'local.properties'. Place 'sdk.dir=`<path to SDK>`' and 'ndk.dir=`<path to NDK>`'
- Run `gradle clean cleanBuildCache assembleDebug` from `android` folder
- You will find an .apk file in `android/build/outputs/apk` folder

Creating release .apk
---------------------

In order to create release .apk you must obtain a Java keystore file(.jks). Either you have in already, or you can generate it yourself using keytool, or from one of you existing well-know certificates.
For example, i2pd release are signed with this [certificate](https://github.com/PurpleI2P/i2pd/blob/openssl/contrib/certificates/router/orignal_at_mail.i2p.crt).

Change file 'build.gradle':

```patch
--- a/android/build.gradle
+++ b/android/build.gradle
@@ -46,11 +46,17 @@ android {
             keyAlias "i2pdapk"
             keyPassword "android"
         }
+        release {
+            storeFile file("path to .jks")
+            storePassword "Store Password"
+            keyAlias "alias"
+            keyPassword "Key Passwordq"
+        }
     }
     buildTypes {
         release {
             minifyEnabled true
-            signingConfig signingConfigs.orignal
+            signingConfig signingConfigs.release
```

Run `gradle clean cleanBuildCache assembleRelease`

Building executable android binary
------------------------------

- Change line `I2PD_LIBS_PATH` in `android_binary_only/jni/Application.mk` to an actual path where you put the dependencies to
- Run `ndk-build -j <threads>` from `android_binary_only` folder
- You will find an `i2pd` executable in `android_binary_only/libs/armeabi-v7a` folder
