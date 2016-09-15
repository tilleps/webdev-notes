# Cordova Notes #


## Create Project ##

```
cordova create myexampleapp com.yourname.myexampleapp MyExampleApp
cd myexampleapp/
cordova platforms add ios
cordova platforms add android
cordova plugin add cordova-plugin-device
cordova plugin add cordova-plugin-console
cordova plugins add cordova-plugin-whitelist
cordova plugin add https://github.com/EddyVerbruggen/LaunchMyApp-PhoneGap-Plugin.git --variable URL_SCHEME=myexampleapp
cordova plugin add https://github.com/phonegap-build/PushPlugin.git
```


### Recommended Plugins ###

[Status bar])(https://github.com/apache/cordova-plugin-statusbar)
```
cordova plugin add cordova-plugin-statusbar
```

```
<platform name="ios">
  <preference name="StatusBarOverlaysWebView" value="false" />
</platform>
```


https://github.com/mesmotronic/cordova-plugin-fullscreen
```
cordova plugin add cordova-plugin-fullscreen
```


### iOS ###

```
cordova build ios
cordova emulate ios
```

```
npm install -g ios-deploy
```


### Android ###


#### Install JDK 8.0 (or whatever is required) ####

JDK 8.0
http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html

JDK 7.0
http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html


##### Install Necessary Dependencies #####

~/Library/Android/sdk/tools/android

Install the following:
1. "SDK Platform" for android-23
2. "Android SDK Platform-tools (latest)
3. "Android SDK Build-tools" (latest)
4. Intel x86 Emulator Accelerator (HAXM installer)

~/Library/Android/sdk/tools/android sdk
~/Library/Android/sdk/tools/android avd
~/Library/Android/sdk/platform-tools/adb devices

```
cordova build android
cordova emulate android
cordova run android --device
```

platforms/android/build/outputs/apk/android-debug.apk


###### Setup Android Device for Testing ######

Android > Security
[x] Unknown Sources


Developer Mode
About Device > Build Number (click many times)

Developer Options
[x] USB debugging


###### ADB Commands ######

adb devices
adb shell
adb install
adb remount
adb push
adb pull
adb logcat

