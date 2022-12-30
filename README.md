# sibs-sdk-ios

## 1. Project configuration

Version 13.0 is the minimum requirement for the library to work properly with the iOS.

###### Adding dependencies

Library file (SibsSDK.xcframework) should be added to the project. In order to add them, perform the following:

    select „File → Add Files To” in Xcode
    select library file
    select option „Copy items if needed"
    select option „Create groups”
    in the field „Add to targets” select all the elements to which a library can be added

###### Preparation of project

Add the setting below to the configurational file Info.plist of the application:

    <key>NSAppTransportSecurity</key>
    <dict>
        <key>NSAllowsArbitraryLoadsInWebContent</key>
        <true/>
    </dict>

###### NOTE!

> The library contains anti-debug traps, so when using the library methods make sure the „Debug Executable” option is off.



###### SDK configuration!

Add the setting below to the AppDelegate:


    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        // Override point for customization after application launch.

        // baseURL, webURL, clientID and token shoulde be provided by SIBS
        SIBS.SDK.configure(withBaseURL: baseURL, webURL: webURL, clientID: clientID, token: token, language: .en)
        return true
    }

