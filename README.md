# Hubtel Web Merchant  Checkout SDK 

The Checkout SDK provide seamless way for businesses to accept payment from their customers. The SDK offers three primary methods of integration: Redirect, Iframe, and Modal. This provides flexibility for developers to choose the integration method that best suits their needs.

## Installation

To use the Hubtel Checkout SDK to collect payment, install the NPM Package or include the CDN link in your HTML file:

`NPM`

```bash
npm i @hubteljs/checkout
```

`CDN`

```html
<script src="https://unified-pay.hubtel.com/js/v1/checkout.js"></script>
```

## Methods of Integration

The Hubtel Checkout SDK supports three primary methods of integration:

**1. Modal Integration:** This method opens the checkout page in a modal popup

**2. Redirect Integration:** This method opens the checkout page in a new tab or window.

**3. Iframe Integration:** This method embeds the checkout page within an iframe on your webpage.

## Pre Checkout

**Note**: Before calling the Checkout SDK, most apps would call a Pre-Checkout API provided by their backend team to create the order for the client app to handle the payment. The implementation might differ.

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
    "description": "Payment of GHS 50.00 for (18013782) (MR SOMUAH STA ADANE-233557913587) ",
    "clientReference": "f03dbdcf8d4040dd88d0a82794b0229f",
    "amount": 1.0,
    "customerMobileNumber": "233557913587",
    "callbackUrl": "https://instantservicesproxy.hubtel.com/v2.1/checkout/callback"
  }
}
```

The response from the Pre-Checkout API usually contains all information needed for a successful checkout. Once retrieved the data is then passed onto the Hubtel Checkout SDK to begin the checkout. 


## Modal Integration

The modal integration method opens the checkout page in a modal popup. This is done by calling the `openModal` method of the Checkout SDK instance and passing the purchase information, configuration options and callBacks for event handling as parameters.

#### Example Usage

```javascript
// Import the Checkout SDK if you are using the npm package
import CheckoutSdk from "@hubteljs/checkout";

// Initialize the Checkout SDK
const checkout = new CheckoutSdk();

// Purchase information
const purchaseInfo = {
  amount: 50,
  purchaseDescription:
    "Payment of GHS 5.00 for (18013782) (MR SOMUAH STA ADANE-233557913587)",
  customerPhoneNumber: "233557913587",
  clientReference: "unique-client-reference-12345",
};

// Configuration options
const config = {
  branding: "enabled",
  callbackUrl: "https://yourcallbackurl.com",
  merchantAccount: 11334,
  basicAuth: "your-basic-auth-here",
};

// A function to open the payment modal
const initPay = () => {
  checkout.openModal({
    purchaseInfo,
    config,
    callBacks: {
      onInit: () => console.log("Iframe initialized: "),
      onPaymentSuccess: (data) => {
        console.log("Payment succeeded: ", data);
        // You can close the popup here
        checkout.closePopUp();
      },
      onPaymentFailure: (data) => console.log("Payment failed: ", data),
      onLoad: () => console.log("Checkout has been loaded: "),
      onFeesChanged: (fees) =>
        console.log("Payment channel has changed: ", fees),
      onResize: (size) =>
        console.log("Iframe has been resized: ", size?.height),
      onClose: (size) => console.log("The modal has closed ", size?.height),
    },
  });
};
```

# Redirect Integration

The redirect method opens the checkout page in a new tab or window. This is done by calling the `redirect` method of the Checkout SDK instance and passing the purchase information and configuration options as parameters.

#### Example Usage

```javascript

// Import the Checkout SDK if you are using the npm package
import CheckoutSdk from "@hubteljs/checkout";

// Initialize the Checkout SDK
const checkout = new CheckoutSdk();

//Purchase information
const purchaseInfo = {
  amount: 50,
  purchaseDescription:
    "Payment of GHS 50.00 for (18013782) (MR SOMUAH STA ADANE-233557913587)",
  customerPhoneNumber: "233557913587",
  clientReference: "unique-client-reference-12345",
};

// Configuration options
const config = {
  branding: "enabled",
  callbackUrl: "https://yourcallbackurl.com",
  merchantAccount: 11334,
  basicAuth: "your-basic-auth-here",
};

// A function to redirect to the payment page
const redirect = () => {
  checkout.redirect({
    purchaseInfo,
    config,
  });
};
```

# Iframe Integration

The iframe integration method embeds the checkout page within an iframe on your webpage. This method is suitable for developers who want to keep their users on the same page while making a payment.


#### Example Usage

In your HTML file, add a div element with the id `hubtel-checkout-iframe` where you want the checkout to be rendered.

```html
 
 <body>
  ...
   <!-- Add this element to where you want the checkout to be rendered. By default the checkout will use the available  height and width of it's container -->

    <div id="hubtel-checkout-iframe"></div>
  ...

 </body>

```

In your JavaScript file, call the `initIframe` method of the Checkout SDK instance and pass the purchase information, configuration options, iframe style and callBacks for event handling as parameters.

```javascript

// Import the Checkout SDK if you are using the npm package
import CheckoutSdk from "@hubteljs/checkout";

// Initialize the Checkout SDK
const checkout = new CheckoutSdk();

//Purchase information
const purchaseInfo = {
  amount: 50,
  purchaseDescription:
    "Payment of GHS 50.00 for (18013782) (MR SOMUAH STA ADANE-233557913587)",
  customerPhoneNumber: "233557913587",
  clientReference: "unique-client-reference-12345",
};

// Configuration options
const config = {
  branding: "enabled",
  callbackUrl: "https://yourcallbackurl.com",
  merchantAccount: 11334,
  basicAuth: "your-basic-auth-here",
};

const iframeStyle = {
    width: '100%',
    height: '100%',
    border: 'none',
  };

// A function to render the iframe on the page where the div with id 'hubtel-checkout-iframe' is located
const openIframe = () => {
    checkout.initIframe({ purchaseInfo, config, iframeStyle, callBacks: {
      onInit: () => console.log('Checkout initialize Initialized'),
      onPaymentSuccess: (data) => console.log('Payment Success', data),
      onPaymentFailure: (data) => console.log('Payment Failure', data),
      onLoad: () => console.log('Iframe Loaded'),
      onFeesChanged: (fees) => console.log('Fees Changed', fees),
      onResize: (size) => console.log('Iframe Resized', size?.height),
    }})
};

```


#### Parameters

| Parameter             | Type     | Description                                                                                            |
| --------------------- | -------- | ------------------------------------------------------------------------------------------------------ |
| **purchaseInfo**      | Object   | Contains details about the purchase.                                                                   |
| `amount`              | number   | The purchase amount.                                                                                   |
| `purchaseDescription` | string   | A brief description of the purchase.                                                                   |
| `customerPhoneNumber` | string   | The customer’s phone number.                                                                           |
| `clientReference`     | string   | A unique reference for the purchase.                                                                   |
| **config**            | Object   | Contains configuration options.                                                                        |
| `branding`            | string   | Set to "enabled" to show the business name and logo or "disabled" to hide them.                        |
| `callbackUrl`         | string   | The URL to which the SDK will send the response.                                                       |
| `merchantAccount`     | number   | The merchant account ID.                                                                               |
| `basicAuth`           | string   | Basic auth credentials.                                                                                |
| `integrationType`     | string   | Specifies the integration type. Default is "External" which is the type used for external integration. |
| **callacks**          | Object   | Callback functions for various events.                                                                 |
| `onInit`              | function | Called when the checkout is initialized.                                                                 |
| `onPaymentSuccess`    | function | Called when the payment is successful.                                                                 |
| `onPaymentFailure`    | function | Called when the payment fails.                                                                         |
| `onLoad`              | function | Called when the iframe or modal is loaded.                                                                      |
| `onFeesChanged`       | function | Called when the the user select a different payment channel                                                                           |
| `onClose`             | function | Called when the the modal is closed.                                                                   |
| `onResize`             | function | Called when the iframe have been resized closed.                                                                   |

## Contribution

We welcome contributions from the developer community to improve the Hubtel Checkout SDK. To contribute:

1. **Fork the Repository**: Start by forking the Hubtel Checkout SDK repository on GitHub.

2. **Create a Feature Branch**: Create a new branch for your feature or bugfix.

3. **Implement Your Changes**: Make your code changes or improvements.

4. **Write Tests**: Write tests for your code changes or improvements.

5. **Submit a Pull Request**: Push your changes to your forked repository and submit a pull request for review.
