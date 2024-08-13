
# External Unified Checkout SDK Integration Documentation

## Overview
The Checkout SDK provide seamless way for businesses to accept payment from their customers. The SDK offers three primary methods of integration: Redirect, Iframe, and Modal. This provides flexibility for developers to choose the integration method that best suits their needs.


## Installation

To use the Hubtel Checkout SDK to collect payment, install the NPM Package or include the the CDN link in your HTML file:


```bash
npm install hubtel-checkout
```

## Methods of Integration
The Hubtel Checkout SDK supports three primary methods of integration:

**1. Modal Integration:** This method opens the checkout page in a modal popup

**2. Redirect Integration:**  This method opens the checkout page in a new tab or window. 

**3. Iframe Integration:** This method embeds the checkout page within an iframe on your webpage.




**Note**: Before calling the Checkout SDK, most internal apps would call a Pre-Checkout API provided by their backend team. The implementation might differ.

Sample Pre-checkout Request:

```json
{
  "amount": "1",
  "phoneNumber": "0200000000"
}
```

Sample Pre-checkout Response:

```json
{
  "message": "success",
  "code": "200",
  "data": {
    "description": "Payment of GHS 1.00 for (18013782) (MR SOMUAH STA ADANE-233557913587) ",
    "clientReference": "f03dbdcf8d4040dd88d0a82794b0229f",
    "amount": 1.0,
    "customerMobileNumber": "233557913587",
    "callbackUrl": "https://instantservicesproxy.hubtel.com/v2.1/checkout/callback"
  }
}
```

The response from the Pre-Checkout API usually contains all information needed for a successful checkout. Once retrieved the data is then passed onto the Hubtel Checkout SDK to begin the checkout.

**Note**: The above approach may not be applicable in your case.


## Modal Integration

The modal integration method opens the checkout page in a modal popup. This is done by calling the `openModal` method of the Checkout SDK instance and passing the purchase information, configuration options and callBacks for event handling as parameters.


#### Example Usage

```javascript

// Import the Checkout SDK
import CheckoutSdk from "hubtel-checkout";

// Initialize the Checkout SDK
const checkout = new CheckoutSdk();

// Call the initPay function to open the modal
const initPay = () => {
    const purchaseInfo = {
      amount: 50,
      purchaseDescription: 'Payment of GHS 1.00 for (18013782) (MR SOMUAH STA ADANE-233557913587)',
      customerPhoneNumber: "233557913587",
      clientReference: "unique-client-reference-12345",
    };

    const config = {
      branding: "enabled",
      callbackUrl: "https://yourcallbackurl.com",
      merchantAccount: 11334,
      basicAuth: "your-basic-auth-here",
    };

    checkout.openModal({
      purchaseInfo,
      config,
      callBacks: {
        onInit: () => console.log("Iframe initialized: "),
        onPaymentSuccess: (data) => {console.log("Payment succeeded: ", data)
          // You can close the popup here
          checkout.closePopUp();
        },
        onPaymentFailure: (data) => console.log("Payment failed: ", data),
        onLoad: () => console.log("Checkout has been loaded: "),
        onFeesChanged: (fees) => console.log("Payment channel has changed: ", fees),
        onResize: (size) => console.log("Iframe has been resized: ", size?.height),
        onClose: (size) => console.log("The modal has closed ", size?.height),
      },
    });
}
```

#### Parameters  

| Parameter             | Type     | Description                                                                     |
| --------------------- | -------- | ------------------------------------------------------------------------------- |
| **purchaseInfo**      | Object   | Contains details about the purchase.                                            |
| `amount`              | number   | The purchase amount.                                                            |
| `purchaseDescription` | string   | A brief description of the purchase.                                            |
| `customerPhoneNumber` | string   | The customer’s phone number.                                                    |
| `clientReference`     | string   | A unique reference for the purchase.                                            |
| **config**            | Object   | Contains configuration options.                                                 |
| `branding`            | string   | Set to "enabled" to show the business name and logo or "disabled" to hide them. |
| `callbackUrl`         | string   | The URL to which the SDK will send the response.                                |
| `merchantAccount`     | number   | The merchant account ID.              |
| `basicAuth`           | string   | Basic auth credentials.               |   
| `integrationType`     | string   | Specifies the integration type. Default is "External" which is the type used for external integration.              |    
| **callacks**          | Object   | Callback functions for various events.                                          |
| `onInit`              | function | Called when the iframe is initialized.                                          |
| `onPaymentSuccess`    | function | Called when the payment is successful.                                          |
| `onPaymentFailure`    | function | Called when the payment fails.                                                  |
| `onLoad`              | function | Called when the iframe is loaded.                                               |
| `onFeesChanged`       | function | Called when the fees change.                                                    |
| `onClose`             | function | Called when the the modal is closed.                                            |



## Redirect Integration

The redirect method opens the checkout page in a new tab or window. This is done by calling the `redirect` method of the Checkout SDK instance and passing the purchase information and configuration options as parameters.


#### Example Usage

```javascript

// Import the Checkout SDK
import CheckoutSdk from "hubtel-checkout";

// Initialize the Checkout SDK
const checkout = new CheckoutSdk();



const redirect = () =>{

const purchaseInfo = {
      amount: 50,
      purchaseDescription: 'Payment of GHS 1.00 for (18013782) (MR SOMUAH STA ADANE-233557913587)',
      customerPhoneNumber: "233557913587",
      clientReference: "unique-client-reference-12345",
    };

    const config = {
      branding: "enabled",
      callbackUrl: "https://yourcallbackurl.com",
      merchantAccount: 11334,
      basicAuth: "your-basic-auth-here",
    };

 checkout.redirect({
      purchaseInfo,
      config
    });
}
```