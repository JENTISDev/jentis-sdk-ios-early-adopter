
# JentisSDK Documentation

## Overview
JentisSDK is a robust iOS SDK designed to facilitate app tracking to Jentis. This documentation provides details on installation, initialization, and usage of the SDK, including advanced configuration options.

---

## Table of Contents
1. [Installation](#installation)
2. [Initialization](#initialization)
3. [Usage](#usage)
   - [Setting User Consents](#setting-user-consents)
   - [Tracking Custom Events](#tracking-custom-events)
   - [Submitting Data](#submitting-data)
   - [Adding Enrichments](#adding-enrichments)
   - [Adding Custom Enrichments](#adding-custom-enrichments)
   - [Add to Cart](#add-to-cart)
4. [TrackConfig Details](#trackconfig-details)
5. [License](#license)

---

## Installation

### Swift Package Manager
Add `JentisSDK` as a dependency in your `Package.swift` file:
```swift
dependencies: [
    .package(url: "[https://github.com/your-repo/JentisSDK](https://github.com/JENTISDev/jentis-sdk-ios-early-adopter.git)", from: "1.0.0")
]
```

### Manual Integration
Download `JentisSDK.xcframework` and add it to your project dependencies.

---

## Initialization

Before using the SDK, configure it with a `TrackConfig` instance. This should be done in the app’s startup code, typically in `AppDelegate` or the SwiftUI `@main` struct.

### Example:
```swift
import JentisSDK

@main
struct MyApp: App {
    init() {
        let config = TrackConfig(
            trackDomain: "tracking.yourdomain.com",
            container: "your-container",
            environment: .stage, // live or stage
            version: "1",
            debugCode: "optional-debug-code",
            authorizationToken: "your-authorization-token",
            customProtocol: "https" // Optional custom protocol
        )
        JentisService.configure(with: config)
    }
}
```

---

## Usage

### Setting User Consents
Set user consents for different vendors:
```swift
TrackingService.shared.setConsents([
    "googleanalytics": .allow,
    "facebook": .deny
])
```

### Tracking Custom Events
Track custom events using `push`:
```swift
TrackingService.shared.push([
    "track": "pageview",
    "title": "Home Page",
    "url": "https://example.com"
])
```

### Submitting Data
Submit all stored events to the server:
```swift
try await TrackingService.shared.submit()
```

### Adding Enrichments
Add enrichments to tracking payloads:
```swift
TrackingService.shared.addEnrichment(
    pluginId: "userSession",
    arguments: [
        "sessionStart": "2024-11-21T10:00:00Z",
        "isPremiumUser": true
    ],
    variables: ["sessionId", "userType"]
)
```

### Adding Custom Enrichments
Add custom enrichments to tracking payloads:
```swift
TrackingService.shared.addCustomEnrichment(
   pluginId: "enrichment_xxxlprodfeed",
   arguments: [
       "account": "JENTIS TEST ACCOUNT",
       "page_title": "MY PAGE TITLE",
       "productId": ["123", "ABC", "3"],
       "baseProductId": ["1"]
   ],
   variables: ["enrichment_product_variant"]
)
```

### Add to Cart
Push an "Add to Cart" action and submit the data:
```swift
try await TrackingService.shared.addToCart([
    "product_id": "12345",
    "quantity": "2",
    "price": "49.99"
])
```

---

## TrackConfig Details
The `TrackConfig` class manages the configuration of the SDK. Below are its properties:

| Property              | Type            | Description                          | Default     |
|-----------------------|-----------------|--------------------------------------|-------------|
| `trackDomain`         | `String`        | The destination domain for requests | Required    |
| `container`           | `String`        | Container value from JENTIS DCP     | Required    |
| `environment`         | `.live/.stage`  | Tracking environment                | `.stage`    |
| `version`             | `String?`       | Optional version for debug          | `nil`       |
| `debugCode`           | `String?`       | Optional debug code                 | `nil`       |
| `authorizationToken`  | `String`        | Token for authorization             | `""`        |
| `customProtocol`      | `String?`       | Custom protocol (e.g., `http`)      | `"https"`   |

---

## License
This SDK is licensed under the MIT License.
