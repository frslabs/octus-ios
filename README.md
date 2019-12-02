
# Octus SDK
> Octus SDK uses advanced deep learning technologies for accurate and fast Document/ID scanning and OCR
> 
[![CocoaPods Compatible](https://img.shields.io/cocoapods/v/EZSwiftExtensions.svg)]

Octus SDK uses advanced deep learning technologies for accurate and fast ID scanning and OCR. Businesses can integrate the Octus SDK into native Android Apps which comes with pre-built screens and configurations. The SDK returns the scanned images, extracted data and error codes. And as a safety measure, the SDK does not store any of the personal data or ID images that are scanned.

## Features

- [x] Auto scanning domestic and internationals ID cards
- [x] OCR is highly accurate 
- [x] Scan duration is very less 
- [x] MRTD document supported

## Requirements

- iOS 10.0+
- Xcode 11.2

## Installation

#### CocoaPods
You can use [CocoaPods](http://cocoapods.org/) to install `Octus` by adding it to your `Podfile`:

```ruby
platform :ios, '13.0'
use_frameworks!
pod 'Octus'
pod 'TesseractOCRiOS', '4.0.0'
```

To get the full benefits import `Octus` wherever you import UIKit

``` swift
import UIKit
import Octus
```
#### Supported Tessdata installation
1. Download and drop ```mrz.trainneddata``` in your project tessdata folder.  
2. Congratulations! 

## Usage example

```swift
import Octus

let scanner = IdScannerController(delegate: self)
scanner.modalPresentationStyle = .fullScreen
scanner.licenceKey = "Your Licence Key"
scanner.documentType = Document.PAN.rawValue
scanner.documentCountry = Country.in.rawValue
scanner.documentSubType = ScanMode.OCR.rawValue
present(scanner, animated: false)
```
#### Handling the result

```swift
func idScannerController(_ scanner: IdScannerController, didFinishScanningWithResults results: IdScannerResults) {
print("Octus Result: ", result)
scanner.dismiss(animated:  true, completion:  nil)
}
func  idScannerControllerDidCancel(_ scanner: IdScannerController) {
scanner.dismiss(animated:  true, completion:  nil)
}
func idScannerController(_ scanner: IdScannerController, didFailWithError error: Int{
print("ErrorCode: ", error)
scanner.dismiss(animated: true, completion: nil)
}
``` 

## Contribute

We would love you for the contribution to **YourLibraryName**, check the ``LICENSE`` file for more info.


