<h3 align="center">
    <img src="Ji.png" width=220 alt="Ji: a Swift XML/HTML parser" />
</h3>

# Ji 戟
[![CI Status](https://travis-ci.org/honghaoz/Ji.svg?branch=master)](https://travis-ci.org/honghaoz/Ji)
[![CocoaPods Version](https://img.shields.io/cocoapods/v/Ji.svg?style=flat)](http://cocoapods.org/pods/Ji)
[![Carthage Compatible](https://img.shields.io/badge/Carthage-compatible-0473B3.svg?style=flat)](https://github.com/Carthage/Carthage)
[![License](https://img.shields.io/cocoapods/l/Ji.svg?style=flat)](http://cocoapods.org/pods/Ji)
[![Platform](https://img.shields.io/cocoapods/p/Ji.svg?style=flat)](http://cocoapods.org/pods/Ji)

Ji (戟) is a Swift wrapper on libxml2 for parsing XML/HTML.

## Features
- [x] Build XML/HTML Tree and Navigate.
- [x] XPath Query Supported.
- [x] Comprehensive Unit Test Coverage.
- [x] Support Swift Package Manager (SPM). Linux compatible.

## Requirements

- iOS 8.0+ / Mac OS X 10.9+ / watchOS 2.0+ / tvOS 9.0+
- Xcode 8.0+

## Installation

### [CocoaPods](http://cocoapods.org)

To integrate **Ji** into your Xcode project using CocoaPods, specify it in your `Podfile`:

```ruby
use_frameworks!

pod 'Ji', '~> 4.2.0'
```

Then, run the following command:

```bash
$ pod install
```

### [Carthage](http://github.com/Carthage/Carthage)

To integrate `Ji` into your Xcode project using Carthage, specify it in your `Cartfile`:

```ogdl
github "honghaoz/Ji" ~> 4.2.0
```

### [Swift Package Manager (SPM)](https://swift.org/package-manager)

#### Prerequisites
- OSX

```bash
brew install libxml2
brew link --force libxml2
```

- Linux
```bash
$ sudo apt-get install libxml2-dev
```

#### Update `Package.swift`
To integrate `Ji` in your project, add the proper description to your `Package.swift` file:
```swift
// swift-tools-version:4.2
import PackageDescription

let package = Package(
    name: "YOUR_PROJECT_NAME",
    dependencies: [
        .package(url: "https://github.com/honghaoz/Ji.git", from: "4.2.0")
    ],
    targets: [
        .target(
            name: "YOUR_TARGET_NAME",
            dependencies: ["Ji"]
        ),
        ...
    ]
)
```

### Manually

If you prefer not to use a dependency manager, you can integrate Ji into your project manually.

- Add sources into your project:
  - Drag `Ji.swift`, `JiHelper.swift` and `JiNode.swift` in [**Sources/Ji**](https://github.com/honghaoz/Ji/tree/master/Sources/Ji) folder into your project.
  - Drag [**Sources/Clibxml2**](https://github.com/honghaoz/Ji/tree/master/Sources/Clibxml2) folder into your project.

- Configure your project:
    - Open project, select the target, under **Build Settings**, in **Header Search Paths**, add `$(SDKROOT)/usr/include/libxml2`
    - Under **Build Settings**, in **Import Paths**, add `$(SRCROOT)/Clibxml2` (Make sure this is the path to the `Clibxml2` folder)

## Usage

> If you are using CocoaPods to integrate Ji. Import Ji first:
> ```swift
> import Ji
> ```

- Init with `URL`:
```swift
let jiDoc = Ji(htmlURL: URL(string: "http://www.apple.com/support")!)
let titleNode = jiDoc?.xPath("//head/title")?.first
print("title: \(String(describing: titleNode?.content))") // title: Optional("Official Apple Support")
```

- Init with `String`:
```swift
let xmlString = "<?xml version='1.0' encoding='UTF-8'?><note><to>Tove</to><from>Jani</from><heading>Reminder</heading><body>Don't forget me this weekend!</body></note>"
let jiDoc = Ji(xmlString: xmlString)
let bodyNode = jiDoc?.rootNode?.firstChildWithName("body")
print("body: \(String(describing: bodyNode?.content))") // body: Optional("Don\'t forget me this weekend!")
```

- Init with `Data`:
```swift
let googleIndexData = try? Data(contentsOf: URL(string: "http://www.google.com")!)
if let googleIndexData = googleIndexData {
    let jiDoc = Ji(htmlData: googleIndexData)!
    let htmlNode = jiDoc.rootNode!
    print("html tagName: \(String(describing: htmlNode.tagName))") // html tagName: Optional("html")

    let aNodes = jiDoc.xPath("//body//a")
    if let firstANode = aNodes?.first {
        print("first a node tagName: \(String(describing: firstANode.name))") // first a node tagName: Optional("a")
        let href = firstANode["href"]
        print("first a node href: \(String(describing: href))") // first a node href: Optional("http://www.google.ca/imghp?hl=en&tab=wi")
    }
} else {
    print("google.com is inaccessible")
}

let 戟文档 = 戟(htmlURL: URL(string: "https://cocoapods.org/pods/Ji")!)
let attribution = 戟文档?.xPath("//ul[@class='attribution']")?.first
print("作者(Author): \(String(describing: attribution?.content))") // 作者(Author): Optional("ByHonghao Zhang")
```

## License

The MIT License (MIT)

Copyright (c) 2019 Honghao Zhang (张宏昊)

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
