# iOS Setup Instructions (Required)

:information_source: Use `yarn` or `npm` as desired.

```
npm install @transistorsoft/capacitor-background-geolocation
npm install @transistorsoft/capacitor-background-fetch

npx cap sync
```

## Configure Background Capabilities

Open your App in **XCode** and select the root of your project.  Select **Capabilities** tab.  Enable **Background Modes** and enable the following modes:

- [x] Location updates
- [x] Background fetch
- [x] Audio (**optional for debug-mode sound FX**, see API docs [Config.debug](https://transistorsoft.github.io/capacitor-background-geolocation/interfaces/config.html#debug)

![](https://dl.dropboxusercontent.com/s/a4xieyd0h38xklu/Screenshot%202016-09-22%2008.12.51.png?dl=1)

## Configure Usage Strings in `Info.plist`

Edit **`Info.plist`**.  Add the following items (Set **Value** as desired):

| Key | Type | Value |
|-----|-------|-------------|
| *Privacy - Location Always and When in Use Usage Description* | `String` | *CHANGEME: Location required in background* |
| *Privacy - Location When in Use Usage Description* | `String` | *CHANGEME: Location required when app is in use* |
| *Privacy - Motion Usage Description* | `String` | *CHANGEME: Motion permission helps detect when device in in-motion* |

![](https://dl.dropboxusercontent.com/scl/fi/dh0sen3wxsgp1hox41le0/iOS-permissions-plist.png?rlkey=i3fipjdcpu7p1eez4mapukkpl&dl=1)

## Privacy Manifest

Apple now requires apps provide a [Privacy Manifest for "sensitive" APIs](https://developer.apple.com/documentation/bundleresources/privacy_manifest_files/describing_use_of_required_reason_api?language=objc) which could be abused for "fingerprinting" a user for malicious marketing activity.

If your app does not yet have a *Privacy Manifest* (__`PrivacyInfo.xcprivacy`__), create one now:

<details>
    <summary>ℹ️ Click here for detailed instructions...</summary>

- In XCode, __`File -> New -> File...`__:

![](https://dl.dropboxusercontent.com/scl/fi/n28028i3fbrxd67u491w2/file-new-PrivacyInfo.png?rlkey=sc7s1lyy8fli2c1hz2cfa4cpm&dl=1)

- Be sure to enable your `Targets: [x] YourApp`:

![](https://dl.dropboxusercontent.com/scl/fi/pmbfn5jypvns6r5pyhnui/file-new-PrivacyInfo-targets.png?rlkey=epvjffar23bxgyi9xax9ys40i&dl=1)


</details>


It's best to edit this file's XML manually.
- :open_file_folder: `ios/App/PrivacyInfo.xcprivacy`
- Add the following __4 blocks__ within the `NSPrivacyAccessedAPITypes` `<array>` container:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">

<plist version="1.0">
<dict>
    <key>NSPrivacyAccessedAPITypes</key>
    <array>
        <!-- [1] background_fetch: UserDefaults -->
        <dict>
            <key>NSPrivacyAccessedAPIType</key>
            <string>NSPrivacyAccessedAPICategoryUserDefaults</string>

            <key>NSPrivacyAccessedAPITypeReasons</key>
            <array>
                <string>CA92.1</string>
            </array>
        </dict>

        <!-- [2] background_geolocation: UserDefaults -->
        <dict>
            <key>NSPrivacyAccessedAPIType</key>
            <string>NSPrivacyAccessedAPICategoryUserDefaults</string>

            <key>NSPrivacyAccessedAPITypeReasons</key>
            <array>
                <string>CA92.1</string>
                <string>1C8F.1</string>
            </array>
        </dict>
        <!-- [3] background_geolocation (CocoaLumberjack): FileTimestamp -->
        <dict>
            <key>NSPrivacyAccessedAPIType</key>
            <string>NSPrivacyAccessedAPICategoryFileTimestamp</string>
            <key>NSPrivacyAccessedAPITypeReasons</key>
            <array>
                <string>C617.1</string>
                <string>0A2A.1</string>
            </array>
        </dict>
        <!-- [4] background_geolocation (CocoaLumberjack): DiskSpace -->
        <dict>
            <key>NSPrivacyAccessedAPIType</key>
            <string>NSPrivacyAccessedAPICategoryDiskSpace</string>
            <key>NSPrivacyAccessedAPITypeReasons</key>
            <array>
                <string>E174.1</string>
            </array>
        </dict>
    </array>
</dict>
</plist>
```

## [Configure `capacitor-background-fetch`](https://github.com/transistorsoft/capacitor-background-fetch/blob/master/help/INSTALL-IOS.md#configure-background-capabilities)

The BackgroundGeolocation SDK makes use internally on __`capacitor-background-fetch`__.  Regardless of whether you instend to implement the `BackgroundFetch` Javascript API in your app, you **must** perform the [Background Fetch iOS Setup](https://github.com/transistorsoft/capacitor-background-fetch/blob/master/help/INSTALL-IOS.md#configure-background-capabilities) at __`capacitor-background-fetch`__.

