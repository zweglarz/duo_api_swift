# DuoAPISwift

[![Build Status](https://travis-ci.org/duosecurity/duo_api_swift.svg?branch=master)](https://travis-ci.org/duosecurity/duo_api_swift)
[![CocoaPods Compatible](https://img.shields.io/cocoapods/v/DuoAPISwift.svg)](https://img.shields.io/cocoapods/v/DuoAPISwift.svg)
[![Carthage Compatible](https://img.shields.io/badge/Carthage-compatible-4BC51D.svg?style=flat)](https://github.com/Carthage/Carthage)
[![GitHub license](https://img.shields.io/badge/license-BSD-blue.svg)](https://raw.githubusercontent.com/duosecurity/duo_api_swift/master/LICENSE)

DuoAPISwift is an API client to call Duo API methods with Swift.

## Duo Auth API

The [Duo Auth API][1] provides a low-level API for adding strong two-factor authentication to applications that cannot directly display rich web content.

## Installation

### CocoaPods

To install DuoAPISwift with [CocoaPods][2], add the following line to your `Podfile`.

```
pod 'DuoAPISwift', '~> 2.0'
```

Then run `pod install` to add DuoAPISwift to your project.

### Carthage

To install DuoAPISwift with Carthage, add the following to your `Cartfile`.

```
github "duosecurity/duo_api_swift" ~> 2.0
```

Then run `carthage update` to build the framework. When finished, drag `DuoAPISwift.framework` to your Xcode project.

## Usage

### Creating the Auth Object

```
import DuoAPISwift

let auth = Auth(
    ikey: "<IKEY>",
    skey: "<SKEY (Keep this secret!)>",
    host: "api-xxxxxxxx.duosecurity.com")
```

### Verify that Duo Is Up and Running

```
auth.ping({response in
    print(response)
})
```

### Verify IKEY, SKEY, and Signature

```
auth.check({response in
    print(response)
})
```

### Retrieve Stored Logo

```
auth.logo({ response in
    if let data = response as? NSData {
        if let image = NSImage(data: data as Data) {
            self.logoImageView.image = image
        }
    }
})
```

### Send a Duo Push Authentication Request

```
auth.auth("push",
          username: "<USERNAME>",
          device: "auto",
          completion: { response in
    var allowed = false
    if let r = response["response"] as? [String : Any],
            let result = r["result"] as? String {
        if result == "allow" {
            allowed = true
        }
    }
    if allowed {
        print("Success. Logging you in...")
    } else {
        print("Access denied.")
    }
})
```

[1]: https://duo.com/docs/authapi
[2]: https://cocoapods.org/
