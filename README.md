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

``` xml
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoadsInWebContent</key>
    <true/>
</dict>
```

###### NOTE!

> When using the library methods make sure the „Debug Executable” option is off.



###### SDK configuration!

Add the setting below to the AppDelegate:

``` swift
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
    // Override point for customization after application launch.

    // baseURL, webURL, clientID and token shoulde be provided by SIBS
    SIBS.SDK.configure(withBaseURL: baseURL, webURL: webURL, clientID: clientID, token: token, language: .en)
    return true
}
```

## 2. Transaction call

In order to call the transaction, the following parameters must be set using the builder for **TransactionParams**:

``` swift
do {
    let data = try SIBS.TransactionParams.Builder()
        .currency(.pln)
        .amount(50.00)
        .paymentMethods([.blik])
        .terminalID(182)
        .transactionID("dHJhbnNhY3Rpb25JRA")
        .transactionDescription("Transaction description")
        .client("Rutger Power")
        .email("rutger.power@example.com")
        .build()
} catch {
    print(error)
}
 ```
 
or

``` swift
let data = try? SIBS.TransactionParams.Builder()
    .currency(.pln)
    .amount(50.00)
    .paymentMethods([.blik])
    .terminalID(182)
    .transactionID("dHJhbnNhY3Rpb25JRA")
    .transactionDescription("Transaction description")
    .client("Rutger Power")
    .email("rutger.power@example.com")
    .build()
 ```


> Optionaly you can build additional parameters like **shippingAddress** and **billingAddress** by using dedicated builder:


``` swift
let address = try SIBS.Address.Builder()
    .street1("Rua 123")
    .street2("porta 2")
    .city("Lisboa")
    .postcode("1200-999")
    .country("PT")
    .build()
```

###### Start payment flow:

``` swift
SIBS.SDK.shared.startPayment(from: self, with: data) { result in
    switch result {
    case .success(let data):
        print(data)
    case .failure(let error):
        print(error)
    }
}
```

## 3. Check transaction status

You can always check transaction status independently, for example in case of standard payment flow was interrupted or when you need to check transaction status apart from that flow:

``` swift
SIBS.SDK.shared.check(transactionID: "transactionID") { result in
    switch result {
    case .success(let data):
        print(data)
    case .failure(let error):
        print(error)
    }
}
```
