# Mobile App Notes #


http://christopher5106.github.io/mobile/2015/04/09/create-mobile-app-with-html-css-cordova.html



## Preferred Cordova Plugins ##

https://github.com/EddyVerbruggen/Custom-URL-scheme

```
cordova plugin add cordova-plugin-customurlscheme --variable URL_SCHEME=myfleet
```



## Obtain iOS Development certificate ##

- Go to: https://developer.apple.com/account/
- Create/Verify if needed
- Once logged in, go to the bottom "Join the Apple Developer Program"
- Redirected to: https://developer.apple.com/programs/  click "Enroll"
- https://developer.apple.com/programs/enroll/  "Start Your Enrollment"



## Documentation/Guides ##

Maintaining Your Signing Identities and Certificates
https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/MaintainingCertificates/MaintainingCertificates.html

Hiding Status Bar
https://github.com/apache/cordova-plugin-statusbar#hiding-at-startup



## Install App from Web App ##

```html
<a href="itms-services://?action=download-manifest&url=https://path.to.host/manifest.plist">Install the App!</a>
```

*plist file contents*
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>items</key>
    <array>
        <dict>
            <key>assets</key>
            <array>
                <dict>
                    <key>kind</key>
                    <string>software-package</string>
                    <key>url</key>
                    <string>https://path.to.host/AppFileName.IPA</string>
                </dict>
            </array>
            <key>metadata</key>
            <dict>
                <key>bundle-identifier</key>
                <string>com.domain.myappname</string>
                <key>bundle-version</key>
                <string>0.1</string>
                <key>kind</key>
                <string>software</string>
                <key>title</key>
                <string>My App Title</string>
            </dict>
        </dict>
    </array>
</dict>
</plist>
```


Verify/Trust the developer of the app:
```
Settings -> General -> Device Management
```


### Hiding the Status Bar ###

```xml
<key>UIStatusBarHidden</key>
<true/>
<key>UIViewControllerBasedStatusBarAppearance</key>
<false/>
```


### Create Downloadable/Install App (.ipa) ###

1. Build (cmd+B)
2. Product > Archive > Export...



## Troubleshooting ##

- Xcode does not support opening folders without a project or workspace.

Solution: Select the folder with the .xproj file in it.
