# Sibs SDK iOS
<br/>

## Example app

Check our app with example of SDK implementation: [Example app](https://github.com/payone-pl/sibs-sdk-example-ios)
<br/><br/>
## 1. Project configuration

Minimum deployment target is **iOS 13.0**

###### Adding dependencies

Library file (**SibsSDK.xcframework**) should be added to the project. In order to add them, perform the following:

    select „File → Add Files To” in Xcode
    select library file
    select option „Copy items if needed"
    select option „Create groups”
    in the field „Add to targets” select all the elements to which a library can be added

###### Preparation of project

###### NOTE!

> When using the library methods make sure the „Debug Executable” option is off.



###### SDK initialization and configuration!

Add the code below to your view model/service or other place where you want to use SDK:

``` swift
let sdk: SDK = SDK(
        SDKConfig(
            baseURL: "baseURL",
            webURL: "webURL",
            clientID: "clientID",
            token: "token",
            language: .pl
    ))
```
<br/>

## 2. Transaction call

In order to call the transaction, the following parameters must be set using the builder for **TransactionParams**:

``` swift
do {
    // Required for card tokenization
    let tokenizeParams = TokenizationParams.Builder()
        .tokenizeCard(true)

    // If previous tokens are available
    if let token = token {
        _ = tokenizeParams.cardTokens([token])
    }

    let data = try TransactionParams.Builder()
        .currency(.pln)
        .amount(50.00)
        .paymentMethods([.blik])
        .terminalID(182)
        .transactionID("dHJhbnNhY3Rpb25JRA")
        .transactionDescription("Transaction description")
        .client("Rutger Power")
        .email("rutger.power@example.com")
        .tokenizationParams(tokenizeParams.build())
        .build()
} catch {
    print(error)
}
 ```
 
or

``` swift
// Required for card tokenization
let tokenizeParams = TokenizationParams.Builder()
    .tokenizeCard(true)

// If previous tokens are available
if let token = token {
    _ = tokenizeParams.cardTokens([token])
}

let data = try? TransactionParams.Builder()
    .currency(.pln)
    .amount(50.00)
    .paymentMethods([.blik])
    .terminalID(182)
    .transactionID("dHJhbnNhY3Rpb25JRA")
    .transactionDescription("Transaction description")
    .client("Rutger Power")
    .email("rutger.power@example.com")
    .tokenizationParams(tokenizeParams.build())
    .build()
 ```


> Optionaly you can build additional parameters like **shippingAddress** and **billingAddress** by using dedicated builder:


``` swift
let address = try Address.Builder()
    .street1("Rua 123")
    .street2("porta 2")
    .city("Lisboa")
    .postcode("1200-999")
    .country("PT")
    .build()
```

###### Start payment flow:

``` swift
sdk.startPayment(from: self, with: data) { result in
    switch result {
    case .success(let data):
        print(data)
    case .failure(let error):
        print(error)
    }
}
```
<br/>

## 3. Check transaction status

You can always check transaction status independently, for example in case of standard payment flow was interrupted or when you need to check transaction status apart from that flow:

``` swift
sdk.check(transactionID: "transactionID") { result in
    switch result {
    case .success(let data):
        print(data)
    case .failure(let error):
        print(error)
    }
}
```
