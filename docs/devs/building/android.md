Building on Android
===================

### Note: *Build works ONLY on Linux systems. Current documentation shows how to build application on Ubuntu 18.04*

Pre-requisites
--------------

You need to install `rename` (required for building libraries), `OpenJDK` 11+, `gradle` 5.1+ and `Android SDK`.

```
sudo apt-get install g++ rename openjdk-11-jdk gradle
```

- Android [SDK](https://developer.android.com/studio#downloads) (choose command line tools only)
- If your system provides gradle with version < 5.1, download it from [Gradle](https://gradle.org/install/) homepage

Dependencies
------------

Prepare Android SDK and install required packages

```bash
mkdir /tmp/android-sdk
cd /tmp/android-sdk
wget <latest SDK link>
unzip commandlinetools-linux-*_latest.zip
# install required tools
./cmdline-tools/bin/sdkmanager --sdk_root=/opt/android-sdk "build-tools;31.0.0" "cmake;3.18.1" "ndk;21.4.7075529"
```

Clone repository with submodules
```bash
git clone --recurse-submodules https://github.com/PurpleI2P/i2pd-android.git
```

Compile required libraries

```bash
export ANDROID_SDK_ROOT=/opt/android-sdk
export ANDROID_NDK_HOME=$ANDROID_SDK_ROOT/ndk/21.4.7075529

pushd app/jni
./build_boost.sh
./build_openssl.sh
./build_miniupnpc.sh
popd
```

Building Android application
--------

### Build application

- Create `local.properties` file with path to SDK and NDK
  ```
  sdk.dir=/opt/android-sdk
  ndk.dir=/opt/android-sdk/ndk/21.4.7075529
  ```
- Run `gradle clean assembleDebug`
- You will find an .apk file in `app/build/outputs/apk` folder

### Creating release .apk

In order to create release .apk you must obtain a Java keystore file(.jks). Either you have in already, or you can generate it yourself using keytool, or from one of you existing well-known certificates.
For example, i2pd release are signed with this [certificate](https://raw.githubusercontent.com/PurpleI2P/i2pd/9000b3df4edcbe7f2c8afd0e1e30609746311ace/contrib/certificates/router/orignal_at_mail.i2p.crt).

Change file `app\build.gradle` by replacing pre-defined values with your own

```
release {
    storeFile file("i2pdapk.jks")
    storePassword "android"
    keyAlias "i2pdapk"
    keyPassword "android"
}
```

Run `gradle clean assembleRelease`

Building executable binary
------------------------------

- Set environment variables:
  ```
  export ANDROID_SDK_ROOT=/opt/android-sdk
  export ANDROID_NDK_HOME=$ANDROID_SDK_ROOT/ndk/21.4.7075529
  ```
- Run `$ANDROID_NDK_HOME/ndk-build -j <threads> NDK_MODULE_PATH=$PWD` from `binary/jni` folder
- You will find an `i2pd` executable in `binary/libs/<architecture>` folder
