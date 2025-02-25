# CardScan

CardScan iOS installation guide

## Contents

* [Requirements](#requirements)
* [Installation](#installation)
* [Configure CardScan (Swift)](#configure-cardscan-swift)
* [Using CardScan (Swift)](#using-cardscan-swift)
* [Authors](#authors)
* [License](#license)

## Requirements

* Objective C or Swift 4.0 or higher
* iOS 11 or higher (supports development target of iOS 10.0 or higher)

## Installation

CardScan is available through [CocoaPods](https://cocoapods.org). To install
it, add the following line to your Podfile:

```ruby
pod 'CardScan'
```

Or if you're using Stripe:

```ruby
pod 'CardScan'
pod 'CardScan/Stripe'
```

Next, install the new pod. From a terminal, run:

```
pod install
```

When using Cocoapods, you use the `.xcworkspace` instead of the
`.xcodeproj`. Again from the terminal, run:

```
open YourProject.xcworkspace
```

## Configure CardScan (Swift)

Configure the library when your application launches:

```swift
import UIKit
import CardScan

@UIApplicationMain
class AppDelegate: UIResponder, UIApplicationDelegate {

    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {
    	ScanViewController.configure() 
        // do any other necessary launch configuration
        return true
    }
}
```

## Using CardScan (Swift)

To use CardScan, you create a `ScanViewController`, display it, and
implement the `ScanDelegate` protocol to get the results.

```swift
import UIKit
import CardScan

class ViewController: UIViewController, ScanDelegate {
    @IBAction func scanCardButtonPressed() {
        guard let vc = ScanViewController.createViewController(withDelegate: self) else {
	    print("This device is incompatible with CardScan")
	    return
	}

        self.present(vc, animated: true)
    }

    func userDidSkip(_ scanViewController: ScanViewController) {
        self.dismiss(animated: true)
    }
    
    func userDidCancel(_ scanViewController: ScanViewController) {
        self.dismiss(animated: true)
    }
    
    func userDidScanCard(_ scanViewController: ScanViewController, creditCard: CreditCard) {
    	let number = creditCard.number
	let expiryMonth = creditCard.expiryMonth
	let expiryYear = creditCard.expiryYear

	// If you're using Stripe and you include the CardScan/Stripe pod, you
  	// can get `STPCardParams` directly from CardScan `CreditCard` objects,
	// which you can use with Stripe's APIs
	let cardParams = creditCard.cardParams()

	// At this point you have the credit card number and optionally the expiry.
	// You can either tokenize the number or prompt the user for more
	// information (e.g., CVV) before tokenizing.

        self.dismiss(animated: true)
    }
}
```

## Original Authors

Sam King, Jaime Park, and Andy Li

## License

CardScan is available under the BSD license. See the LICENSE file for more info.
