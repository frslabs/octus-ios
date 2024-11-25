
# OCTUS iOS SDK

![version](https://img.shields.io/badge/version-v1.7.6-blue)

Octus SDK uses advanced deep learning technologies for accurate and fast ID scanning and OCR. Businesses can integrate the Octus SDK into native iOS Apps which comes with pre-built screens and configurations. The SDK returns the scanned images, extracted data and error codes. And as a safety measure, the SDK does not store any of the personal data or ID images that are scanned.

You can find the release history at [Changelog](CHANGELOG.md)

‼ ATTENTION ‼ → BREAKING CHANGE introduced at Octus SDK `v1.6.6`. We have introduced a new license format. If you are using versions prior to `v1.6.6` and intend to update to `v1.6.6` or above please contact `support@frslabs.com` for an updated license.

# Table Of Content
- [Prerequisite](#prerequisite)
- [iOS SDK Requirements](#requirements)
- [Permission](#Permission)
- [Installation](#installation)
- [Usage example](#Usage-example)
- [Octus Result](#Octus-Result)
- [Octus Parameters](#octus-parameters)
- [Octus Error Codes](#octus-error-codes)
- [Help](#help)

## Prerequisite

You will need a valid license to use the Octus SDK, which can be obtained by contacting `support@frslabs.com` . 

Depending on the license - offline or online - you have opted for, the ping functionality to billing servers will be disabled or enabled. For instance, if you have opted for the offline SDK model, then there will be no server ping needed to our billing server to bill you. However, if you have chosen a transaction based pricing, then after each transaction, a ping request will be made to our billing server. This cannot be overrided by the App. A point to note is that if the ping transaction fails for any reason, the whole transaction will be void without any results from the SDK.

Once you have the license , follow the below instructions for a successful integration of Octus SDK onto your iOS Application

## Minimum Requirements

- Xcode 13.0
- iOS 13.0+
- Swift 5.0

## Permission

In Info.plist file add following code to allow your application to access iPhone's camera:
``<key>NSCameraUsageDescription</key>
<string>Allow access to camera</string>``

## Installation

#### CocoaPods
You can use [CocoaPods](http://cocoapods.org/) to install `Octus` by adding it to your `Podfile`:

```ruby
platform :ios, '13.0'
source 'https://gitlab.com/frslabs-public/ios/octus.git'
source 'https://github.com/CocoaPods/Specs.git'
use_frameworks!
pod 'Octus','1.7.6'
pod 'TesseractOCRiOS', :git => 'https://github.com/gali8/Tesseract-OCR-iOS.git'
pod 'TensorFlowLiteSwift', '2.6.0'
```

###### Save/Edit Netrc settings to install custom pod

You will need a valid netrc credentials to install octus from maven, which can be obtained by contacting `support@frslabs.com`. 

1. Create or edit .netrc file under current user's home directory
2. Write the below lines into that file, replace <YOUR_USERNAME> and <YOUR_PASSWORD> with your credentials which is shared through email and save the file.
```ruby
machine www.repo2.frslabs.space
login <YOUR_USERNAME>
password <YOUR_PASSOWRD>
```
3. In terminal enter below command to install the pod
pod install or pod update.

4. Connect with physical device to build and run Octus, It will not build/run in simulator due to camera dependency.

To get the full benefits import `Octus` wherever you import UIKit

``` swift
import UIKit
import Octus
```
#### Supported Tessdata installation
1. Download and drop ```model.trainneddata``` in tessdata folder of your project.
2. Congratulations! 

## Usage example

```swift
import Octus

let scanner = IdScannerController(delegate: self)
scanner.modalPresentationStyle = .fullScreen
scanner.licenceKey = "YOUR_LICENCE_KEY"
scanner.isManualScanEnabled = false
scanner.isOrientationFlat = false
scanner.documentType = Document.PAN.rawValue
scanner.documentCountry = Country.in.rawValue
scanner.documentSubType = ScanMode.OCR.rawValue
scanner.documentSide = 1
scanner.isWatermarkNeeded = true/false (CHEQUE)
present(scanner, animated: false)
```
#### Handling the result

```swift
class  ViewController: UIViewController, IdScannerControllerDelegate {

    /// Tells the delegate that the user scanned a document.
    ///
    /// - Parameters:
    ///   - scanner: The scanner controller object managing the scanning interface.
    ///   - results: The results of the user scanning with the camera. 
    ///   - results.octusResult: return output as NSMutableDictionary.
    /// - Discussion: Your delegate's implementation of this method should dismiss the octus scanner controller.
    func idScannerController(_ scanner: IdScannerController, didFinishScanningWithResults results: IdScannerResults) {
          DispatchQueue.main.async {
            print("OctusResult: ", results.octusResult)
        }      
    }
    
    /// Tells the delegate that the user cancelled the scan operation.
    ///
    /// - Parameters:
    ///   - scanner: The scanner controller object managing the scanning interface.
    /// - Discussion: Your delegate's implementation of this method should dismiss the image scanner controller.
    func  idScannerControllerDidCancel(_ scanner: IdScannerController) {
          DispatchQueue.main.async {
            print("Scan operation cancelled")
        }
    }
    
    /// Tells the delegate that an error occured during the user's scanning experience.
    ///
    /// - Parameters:
    ///   - scanner: The scanner controller object managing the scanning interface.
    ///   - error: The error that occured.
    func idScannerController(_ scanner: IdScannerController, didFailWithError error: Int{        
          DispatchQueue.main.async {
            print("ErrorCode: ", error)
        }
    }  
 }
 
``` 

## Octus Result

```swift

     let resultJson = convertToJson(jsonObject: results.octusResult)
     let resultDict = convertToDictionary(text: resultJson)
   
     let result = resultDict!["OctusData"] as! [String:String]   
     let code = result["code"]
     
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
     let imagePathCrop = result["croppedImagePath"] ?? ""
     
     /// Retrieve image from document directory
            
     let image = getImageFromDocumentDirectory(imagePath: imagePathFace, fileName: "fileName")
     
     `FileNames` - 
         Front Image - "OCTUS_FILE_FRONT.png"
         Back Image - "OCTUS_FILE_BACK.png"
         Face Image - "OCTUS_FILE_FACE.png"
         Crop Image - "OCTUS_FILE_CROP.jpg"
     
      if imagePath.count > 0 {
           let resultImage = getImageFromDocumentDirectory(imagePath: imagePath, fileName: fileName)
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
        }
        else {
            return UIImage()
        }        
    }
    
```

## Octus Error Codes

Error codes and their meaning are tabulated below

| Code          | Message              |
| ------------- | ------------------- |
| 801  | Scan timed out                |
| 802  | Invalid ID parameters passed  |
| 803  | Camera permission denied      |
| 805  | License expired               |
| 806  | License invalid               |
| 807  | Octus server error            |
| 808  | Transaction API error         |
| 811  | QR not detected               |
| 812  | QR parsing failed             |
| 108  | Internet unavailable          |
| 401  | API limit exceeded            |
| 429  | Too many requests             |

## Octus Parameters

- `scanner.licenceKey = "LICENCE KEY"`   ***(Required)***
  
  Accepts the Octus licence key as a `String`
  
- `scanner.isManualScanEnabled = false` ***(Required)***

  `true` - Displays a button used to start the scan when clicked
  `false` - Automatically starts scanning as soon as camera preview is ready
  
- `scanner.documentCountry = Country.countryISOcode.rawValue` ***(Required)***
  
  Sets the country associated with the Document.
  
  For the complete list of supported countries refer ``ISO_3166-1_alpha-2`` format code
 
- `scanner.documentType = Document.value.rawValue` ***(Required)***
  
  Sets the Document which has to be scanned. Possible values are, 
  
  | Value          | Effect                 |
  | -------------- | ---------------------- |
  | Document.PAN   | Pan Card               |
  | Document.ADR   | Aadhaar Card           |
  | Document.VID   | Voter ID               |
  | Document.NID   | National ID            |
  | Document.PPT   | Passport               |
  | Document.VSA   | Visa                   |
  | Document.DRV   | Driving Licence        |
  | Document.CQL   | Cheque Leaf            |
  | Document.GST   | GST Form               |
  | Document.IMG_A | Masked Aadhaar Image Capture  |
  | Document.IMG_H | Plain Horizontal Image capture   |
  | Document.IMG_V | Plain Vertical Image capture    |
  | Document.E_MANDATE | E-mandate Document Capture |
  
- `scanner.documentSubType = ScanMode.OCR.rawValue` ***(Required)***  
  
  Sets the Document Sub Type . Majority of the documents support only `ScanMode.OCR.rawValue` as a sub type. 
  
  *Documents where both `ScanMode.OCR.rawValue` and `ScanMode.BARCODE.rawValue` apply are ,*
  
  - `Document.ADR`
   
  *Documents where only `ScanMode.MRTD.rawValue` apply are ,*
 
  - `Document.PPT`
  - `Document.VSA`

-  `scanner.isWatermarkNeeded = true/false` ***(Optional)*** (CHEQUE)

    - Sets the "CANCELLED" watermark to cheque if it is true.
    - Doesn't set "CANCELLED" watermark to cheque if it false.
   
  
  Possible values for Sub Type are,
  
  | Value            | Effect                         |
  | ---------------- | ------------------------------ |
  | ScanMode.OCR.rawValue | Scans the document in OCR mode |
  | ScanMode.BARCODE.rawValue | Scans the document in QR mode  |
  | ScanMode.MRTD.rawValue | Scans the document in MRZ mode  |
  | ScanMode.CROP.rawValue | Scans the document in Crop mode  |
  
  Note: ScanMode.OCR is only supported from iOS version 13 and above. For versions below this, please use our APIs.
  
  
- `scanner.documentSide = 1 (or 2)` ***(Required)***
  
   Set the document side 1 or 2 according to documentType AND ScanMode
     
  | Value          | ScanMode      |  DocumentSide   |
  | -------------- | ------------- | --------------  |
  | Document.PAN   | OCR           |      1          |
  | Document.ADR   | OCR, BARCODE  |      2, 1       |
  | Document.VID   | OCR           |      2          | 
  | Document.PPT   | MRTD          |      1 OR 2     |   
  | Document.DRV   | OCR           |      2          |
  | Document.CQL   | OCR           |      1          |
  | Document.GST   | OCR           |      1          |
  | Document.IMG_A | CROP          |      2          |
  | Document.IMG_H | CROP          |      2          |
  | Document.IMG_V | CROP          |      2          |
  | Document.E_MANDATE | OCR       |      1         |
    
   Note: In case of MRTD, DocumentSide = 2 is only applicable for Indian passports with address on the back page.
   
 - `scanner.isOrientationFlat = false`  ***(Optional)*** *(Defaults to **false**)*
  
  Sets the value for which the scanner should lock the orientation with respect to the scan surface . Possible values are,
  
  | Value               | Effect     |
  | ------------------- | ---------- |
  | true  | Scans only when orientation of the phone(camera) is perpendicular(flat) to the scan surface. |
  | false | Scans ignoring the orientation of the phone(camera) to the scan surface. |  
  
  ## NOTE

In order to get the Bank details, an API call has to be made to the server to do the MICR lookup and fetch the details.  
   
  
## Help

For any queries/feedback , contact us at `support@frslabs.com` 

