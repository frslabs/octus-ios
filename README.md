
# Octus SDK
> Octus SDK uses advanced deep learning technologies for accurate and fast Document/ID scanning and OCR
> 
[![CI Status](https://img.shields.io/travis/frslabsbot/Octus.svg?style=flat)](https://travis-ci.org/frslabsbot/Octus)
[![Version](https://img.shields.io/cocoapods/v/Octus.svg?style=flat)](https://cocoapods.org/pods/Octus)
[![License](https://img.shields.io/cocoapods/l/Octus.svg?style=flat)](https://cocoapods.org/pods/Octus)
[![Platform](https://img.shields.io/cocoapods/p/Octus.svg?style=flat)](https://cocoapods.org/pods/Octus)

Octus SDK uses advanced deep learning technologies for accurate and fast ID scanning and OCR. Businesses can integrate the Octus SDK into native Android Apps which comes with pre-built screens and configurations. The SDK returns the scanned images, extracted data and error codes. And as a safety measure, the SDK does not store any of the personal data or ID images that are scanned.

## Features

- [x] Auto scanning domestic and internationals ID cards
- [x] OCR is highly accurate 
- [x] Scan duration is very less 
- [x] MRTD document supported

## Prerequisite

You will need a valid license to use the Octus SDK, which can be obtained by contacting `support@frslabs.com` . 

Depending on the license - offline or online - you have opted for, the ping functionality to billing servers will be disabled or enabled. For instance, if you have opted for the offline SDK model, then there will be no server ping needed to our billing server to bill you. However, if you have chosen a transaction based pricing, then after each transaction, a ping request will be made to our billing server. This cannot be overrided by the App. A point to note is that if the ping transaction fails for any reason, the whole transaction will be void without any results from the SDK.

Once you have the license , follow the below instructions for a successful integration of Octus SDK onto your iOS Application

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
1. Download and drop ```model.trainneddata``` in your project.  
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
class  ViewController: UIViewController, IdScannerControllerDelegate {
    func idScannerController(_ scanner: IdScannerController, didFinishScanningWithResults results: IdScannerResults) {
        print("ScanResult: ", results.octusResult)
        scanner.dismiss(animated:  true, completion:  nil)
    }
    func  idScannerControllerDidCancel(_ scanner: IdScannerController) {
        scanner.dismiss(animated:  true, completion:  nil)
    }
    func idScannerController(_ scanner: IdScannerController, didFailWithError error:      Int{
        print("ErrorCode: ", error)
        scanner.dismiss(animated: true, completion: nil)
    }
}
``` 

## Octus Result

```swift

     let resultJson = convertToJson(jsonObject: results.octusResult)
     let resultDict = convertToDictionary(text: resultJson)
   
     let result = resultDict!["OctusData"] as! [String:String]   
     let code = obj["code"]
     
     let code = result["code"]
     let docType = result["documentType"]
     let name1 = result["name1"] ?? ""
     let name2 = result["name2"] ?? ""
     let idNumber = result["number"] ?? ""
     let dob = result["dob"] ?? ""
     let yob = result["yob"] ?? ""
     let country = result ["country"] ?? ""
     let expiry = result["expiry"] ?? ""
     let address = result["address"] ?? ""
     let gender = result["gender"] ?? ""
     let issuedBy = result["issuedBy"] ?? ""
     let ifsc = result["ifsc"] ?? ""
     
     let imagePathFace = result["facePath"] ?? "" 
     let imagePathFront = result["frontImagePath"] ?? ""
     let imagePathBack = result["backImagePath"] ?? ""
     
     /// Retrieve image from document directory
            
     let image = getImageFromDocumentDirectory(imagePath: imagePathFace, fileName: "fileName")
     
     FileName - 
     1. Front Image - "doc_front.png"
     2. Back Image - "doc_back.png"
     1. Face Image - "doc_face.png"
     
      if imagePath.count > 0 {
           let resultImage = getImageFromDocumentDirectory(imagePath: imagePath, fileName: "fileName")
           imageView.image = resultImage
        
      }

    /// Supported Mathods
    
    func convertToJson(jsonObject:NSMutableDictionary) -> String{
        let jsonData: NSData
        do {
            jsonData = try JSONSerialization.data(withJSONObject: jsonObject, options:.prettyPrinted) as NSData
            let jsonString = (String(data: jsonData as Data, encoding: String.Encoding.utf8))!.replacingOccurrences(of: "\\", with: "")
            return jsonString
        } catch _ {
            print ("JSON Failure")
        }
        return ""
    }
    
    func convertToDictionary(text: String) -> [String: Any]? {
        if let data = text.data(using: .utf8) {
            do {
                return try JSONSerialization.jsonObject(with: data, options: []) as? [String: Any]
            } catch {
                print(error.localizedDescription)
            }
        }
        return nil
    }
    
    func getImageFromDocumentDirectory(imagePath : String, fileName: String) -> UIImage {
        let nsDocumentDirectory = FileManager.SearchPathDirectory.documentDirectory
        let nsUserDomainMask    = FileManager.SearchPathDomainMask.userDomainMask
        let paths               = NSSearchPathForDirectoriesInDomains(nsDocumentDirectory, nsUserDomainMask, true)
        if let dirPath = paths.first{
            let frontimageURL = URL(fileURLWithPath: dirPath).appendingPathComponent(fileName)
            let frontimage    = UIImage(contentsOfFile: frontimageURL.path)
            return frontimage ?? UIImage()
        }else{
            return UIImage()
        }
        
    }

```

## License

Octus is available under the FRSLabs license. See the LICENSE file for more info.


