
# Checkout SDK Integration Documentation

## Overview
The Checkout SDK provide seamless way for businesses to accept payment from their customers. The SDK offers three primary methods of integration: Redirect, Iframe, and Modal. This provides flexibility for developers to choose the integration method that best suits their needs.

::: tip
The SDK can be used for both internal and external integrations. Internal integrations are for Hubtel internal apps, while external integrations are for merchant. Code snippets are provided for each integration method to help you get started.
:::


## Methods of Integration
The Hubtel Checkout SDK supports three primary methods of integration:

[**1. Redirect Integration:**](/tools/unified-checkout/redirect)  This method opens the checkout page in a new tab or window. 

[**2. Iframe Integration:**](/tools/unified-checkout/iframe) This method embeds the checkout page within an iframe on your webpage.

[**3. Modal Integration:**](/tools/unified-checkout/modal) This method opens the checkout page in a modal popup


# Iframe Integration

The iframe integration method embeds the checkout page within an iframe on your webpage. This method is suitable for developers who want to keep their users on the same page while making a payment.

## Installation

To use the Hubtel Checkout SDK to collect payment, install the NPM Package or include the the CDN link in your HTML file:

:::tabs

== NPM Package

```bash
npm install hubtel-checkout

```
Then import it in the component where you want to use it

```javascript
import  CheckoutSdk  from 'hubtel-checkout';
```

== Inline (CDN)




```html
<script src="https://unified-pay.hubtel.com/js/v1/checkout.js"></script>
```

::: tip
If you are using a framework like Next, Nuxt etc, follow their recommended approach for adding script. See the example below


// Nuxt Js Example

```ts

//In your nuxt.config.ts

export default defineNuxtConfig({
...
 app: {
  head: {
    script: [
      {
        src: "https://unified-pay.hubtel.com/js/v1/checkout.js",
      },
    ],
  },
 }
 ...
});

```

Next Js Example

```jsx
// In root layout add:

<Script
  src="https://auth.hubtel.com/js/v1/auth.js"
  strategy="beforeInteractive"
/>
```

:::

#### Example Usage

Collect the purchase information, configuration options and iframe style and pass them to `initIframe`  method of the Checkout SDK instance.

:::tabs
== Javascript

```html
<!-- In your HTML   -->

<form id="checkout-form">
  <div>
    <label for="amount">Amount:</label>
    <input type="number" id="amount" name="amount" required />
  </div>

  <div>
    <label for="description">Description:</label>
    <input type="text" id="description" name="description" required />
  </div>

  <div>
    <label for="phone">Customer Phone Number:</label>
    <input type="tel" id="phone" name="phone" required />
  </div>

  <button type="submit">Pay Now</button>
</form>

<div id="checkout-iframe"></div>
```

```javascript
// In your javascript code

document
  .getElementById("checkout-form")
  .addEventListener("submit", function (event) {
    event.preventDefault();

    const amount = document.getElementById("amount").value;
    const description = document.getElementById("description").value;
    const phone = document.getElementById("phone").value;

    const checkout = new CheckoutSdk();

    const purchaseInfo = {
      amount: amount,
      purchaseDescription: description,
      customerPhoneNumber: phone,
      clientReference: "unique-client-reference-12345",
    };

    const config = {
      branding: "enabled",
      callbackUrl: "https://yourcallbackurl.com",
      merchantAccount: 11334,
      basicAuth: "your-basic-auth-here",
    };

      checkout.initIframe({ purchaseInfo, config, iframeStyle, callBacks: {
            onInit: () => console.log('Iframe Initialized'),
            onPaymentSuccess: (data) => console.log('Payment Success', data),
            onPaymentFailure: (data) => console.log('Payment Failure', data),
            onLoad: () => console.log('Iframe Loaded'),
            onFeesChanged: (fees) => console.log('Fees Changed', fees),
            onResize: (size) => console.log('Iframe Resized', size?.height),
        }})
  });
```

== Vue/Nuxt

```html
<!-- In your HTML   -->

<form v-model="checkoutForm" @submit.prevent="receivePayment">
  <label for="amount">Amount:</label>
  <input type="number" id="amount" name="amount" required /><br /><br />

  <label for="description">Description:</label>
  <input type="text" id="description" name="description" required /><br /><br />

  <label for="phone">Customer Phone Number:</label>
  <input type="tel" id="phone" name="phone" required /><br /><br />

  <button type="submit">Pay Now</button>
</form>

<div id="checkout-iframe"></div>
```

```vue
<script setup lang="ts">

import  CheckoutSdk  from 'hubtel-checkout';

// Ensure you validate the form before submitting

  const checkoutForm = ref<{amount: number, description: string, phoneNumber: string, }>({});
  function receivePayment = () => {


        const checkout = new CheckoutSdk();

        const purchaseInfo = {
            amount: checkoutForm.amount,
            purchaseDescription: checkoutForm.description,
            customerPhoneNumber: checkoutForm.phone,
            clientReference: 'unique-client-reference-12345'
        };

        const config = {
            branding: 'enabled',
            callbackUrl: 'https://yourcallbackurl.com',
            merchantAccount: 11334,
            basicAuth: 'your-basic-auth-here',
        };

        const iframeStyle = {
            width: '100%',
            height: '100%',
            border: 'none',
        };

        checkout.initIframe({ purchaseInfo, config, iframeStyle, callBacks: {
            onInit: () => console.log('Iframe Initialized'),
            onPaymentSuccess: (data) => console.log('Payment Success', data),
            onPaymentFailure: (data) => console.log('Payment Failure', data),
            onLoad: () => console.log('Iframe Loaded'),
            onFeesChanged: (fees) => console.log('Fees Changed', fees),
            onResize: (size) => console.log('Iframe Resized', size?.height),
        }})
  }
</script>
```

== React/Next

```jsx
import { useState } from "react";
import  CheckoutSdk  from 'hubtel-checkout';


function PaymentForm() {
  const [checkoutForm, setCheckoutForm] = useState({
    amount: "",
    description: "",
    phone: "",
  });

  const handleInputChange = (e) => {
    const { name, value } = e.target;
    setCheckoutForm((prevForm) => ({
      ...prevForm,
      [name]: value,
    }));
  };

  const receivePayment = (e) => {
    e.preventDefault(); // Prevent form submission

    const checkout = new CheckoutSdk();
    const purchaseInfo = {
      amount: checkoutForm.amount,
      purchaseDescription: checkoutForm.description,
      customerPhoneNumber: checkoutForm.phone,
      clientReference: "unique-client-reference-12345",
    };

    const config = {
      branding: "enabled",
      callbackUrl: "https://yourcallbackurl.com",
      merchantAccount: 11334,
      basicAuth: "your-basic-auth-here",
    };

    checkout.initIframe({
      purchaseInfo,
      config,
      callBacks: {
        onInit: () => console.log("Iframe Initialized"),
        onPaymentSuccess: (data) => console.log("Payment Success", data),
        onPaymentFailure: (data) => console.log("Payment Failure", data),
        onLoad: () => console.log("Iframe Loaded"),
        onFeesChanged: (fees) => console.log("Fees Changed", fees),
        onResize: (size) => console.log("Iframe Resized", size?.height),
      },
    });
  };

  return (
    <form onSubmit={receivePayment}>
      <div>
        <label htmlFor="amount">Amount:</label>
        <input
          type="number"
          id="amount"
          name="amount"
          value={checkoutForm.amount}
          onChange={handleInputChange}
          required
        />
      </div>

      <div>
        <label htmlFor="description">Description:</label>
        <input
          type="text"
          id="description"
          name="description"
          value={checkoutForm.description}
          onChange={handleInputChange}
          required
        />
      </div>

      <div>
        <label htmlFor="phone">Customer Phone Number:</label>
        <input
          type="tel"
          id="phone"
          name="phone"
          value={checkoutForm.phone}
          onChange={handleInputChange}
          required
        />
      </div>

      <button type="submit">Pay Now</button>
    </form>
  );
}

export default PaymentForm;
```

::::

#### Parameters

| Parameter             | Type     | Description                                                        |
| --------------------- | -------- | ------------------------------------------------------------------ |
| **purchaseInfo**      | Object   | Contains details about the purchase.                               |
| `amount`              | number   | The purchase amount.                                               |
| `purchaseDescription` | string   | A brief description of the purchase.                               |
| `customerPhoneNumber` | string   | The customer’s phone number.                                       |
| `clientReference`     | string   | A unique reference for the purchase.                               |
| **config**            | Object   | Contains configuration options.                                    |
| `branding`            | string   | Set to "enabled" to show Hubtel branding or "disabled" to hide it. |
| `callbackUrl`         | string   | The URL to which the SDK will send the response.                   |
| `merchantAccount`     | number   | The merchant account ID. Not required for internal integration.    |
| `basicAuth`           | string   | Basic auth credentials. Not required for internal integration.     |
| `iframeStyle`         | Object   | CSS styles for the iframe.                                         |
| **callBacks**           | Object   | Callback functions for various events.                             |
| `onInit`              | function | Called when the checkout is initialized.                             |
| `onPaymentSuccess`    | function | Called when the payment is successful.                             |
| `onPaymentFailure`    | function | Called when the payment fails.                                     |
| `onLoad`              | function | Called when the iframe is loaded.                                  |
| `onFeesChanged`       | function | Called when the fees change.                                       |
       