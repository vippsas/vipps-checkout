# Vipps Checkout guide

Vipps Checkout is designed to be a low friction, low complexity flow where Vipps Checkout ensures a smooth and efficient checkout experience using the trusted Vipps technology and brand.

Preliminary documentation. Subject to change

- [Vipps Checkout guide](#vipps-checkout-guide)
- [Flow diagram](#flow-diagram)
- [Checkout Features](#checkout-features)
  - [Example requests](#example-requests)
  - [SDK guidelines](#sdk-guidelines)
  - [Merchant hosting of Session](#merchant-hosting-of-session)
  - [Example Integration](#example-integration)
      - [Sticky checkout example using query parameters](#sticky-checkout-example-using-query-parameters)
  - [Modularity of Vipps Checkout fields](#modularity-of-vipps-checkout-fields)
    - [AddressFields false example](#addressfields-false-example)
    - [Addressfields and ContactFields false example](#addressfields-and-contactfields-false-example)
    - [Combination with shipping](#combination-with-shipping)
- [System integration guidelines](#system-integration-guidelines)
  - [Integration partner and plugin guidelines](#integration-partner-and-plugin-guidelines)
    - [Partner signup API guidelines](#partner-signup-api-guidelines)
  - [System information guidelines](#system-information-guidelines)
  - [Transaction operations (Capture, Cancel, Refund)](#transaction-operations-capture-cancel-refund)
  - [Polling integration](#polling-integration)
    - [Example of polling response when checkout SessionState is SessionStarted](#example-of-polling-response-when-checkout-sessionstate-is-sessionstarted)
  - [Example of polling response when checkout SessionState any other state but not SessionStarted](#example-of-polling-response-when-checkout-sessionstate-any-other-state-but-not-sessionstarted)
  - [Webhook integration](#webhook-integration)
  - [Example of webhook notification](#example-of-webhook-notification)
  - [Shipping](#shipping)
    - [Static Shipping](#static-shipping)
    - [Dynamic Shipping](#dynamic-shipping)
  - [Vipps side Transaction information](#vipps-side-transaction-information)
    - [Recommended integration (currently in pilot mode)](#recommended-integration-currently-in-pilot-mode)
  - [In-App Payment](#in-app-payment)

# Flow diagram

The standard flow for a Vipps Checkout consists of

1. Generating a Checkout session.
2. Displaying that session in a Vipps Checkout iframe.
3. The user completing checkout process.
4. The Merchant handles the result of the Checkout process.

![Checkout flow](resources/standard_flow.png)

# Checkout Features

In order to support this basic flow it is important to read the following documentation closely. You should always utilize the newest API Version as specified in the [swagger](https://vippsas.github.io/vipps-checkout-api/). You should also consult the [checklist](https://github.com/vippsas/vipps-checkout-api/blob/main/vipps-checkout-api-checklist.md) for a structured walkthrough of the expected integration points.

## Example requests

First you need to request a Vipps Checkout Session token from the Vipps APIs. The first thing you need to do is set up a server to server request to set up a Checkout session. An example reference request may be seen below.

Request a session token according to your needs, the full specification of the Checkout session endpoint can be found [here](https://vippsas.github.io/vipps-checkout-api/#/Session/post_v2_session)

A minimal example with example values:

Request headers:
Make sure to pass the below mandatory headers in the request that you send from your server to Vipps checkout create session endpoint.

```json
{
  "Vipps-System-Name": "Acme Enterprises Ecommerce DeLuxe",
  "Vipps-System-Version": "3.1.2",
  "Vipps-System-Plugin-Name": "Point Of Sale Excellence",
  "Vipps-System-Plugin-Version": "4.5.6",
  "Client_Id": "<merchant_client_id>",
  "Client_Secret": "<merchant_client_secret>",
  "Ocp-Apim-Subscription-Key": "<Ocp-Apim-Subscription-Key>",
  "merchantSerialNumber": "123456"
}
```

Request body:

```json
{
    "merchantInfo": {
        "merchantSerialNumber": "123456",
        "returnUrl": "https://example.com/vipps", //Will overwrite configuration on Merchant Profile
        "callbackPrefix": "https://example.com/vipps/callbacks-for-payment-updates",
        "callbackAuthorizationToken": "iOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImllX3FXQ1hoWHh0MXpJ",
        "termsAndConditionsUrl": "https://example.com/vipps/termsAndConditions"
    },
    "transaction": {
        "reference" : "31gf1g413121",
        "paymentDescription": "One pair of Vipps socks",
        "amount": {
          "currency": "NOK",
          "value": 20000, //Must be in Minor Units. The smallest unit of a currency.
        }
    },
    "logistics": {
        "dynamicOptionsCallback": "https://example.com/vipps/dynamiclogisticsoptions", // URL for dynamic logistics. If not given, only fixed logistics options will be used.
        "fixedOptions": [
          {
            "id": "postenstore",
            "isDefault": true,
            "priority": 1,
            "brand": "posten",
            "product": "Pick-up in store",
            "description": "Pick up your package at the local store"
            "amount": {
              "currency": "NOK",
              "value": 3900 //Must be in Minor Units. The smallest unit of a currency.
            },
          },
          {
            "id": "postenmailbox",
            "isDefault": false,
            "priority": 2,
            "brand": "posten",
            "product": "Mailbox",
            "description": "Receive your package in the mailbox"
            "amount": {
              "currency": "NOK",
              "value": 2900 //Must be in Minor Units. The smallest unit of a currency.
            },
          }
        ]
    }
}
```

Example response:

```json
{
  "token": "eyJhbGciOiJodHRwOi8vd3d3LnczLm9yZy8yMDAxLzA0L3htbGRzaWctbW9yZSNobWFjLXNoYTI1NiIsInR5cCI6IkpXVCJ9.eyJzZXNzaW9uSWQiOiJUdHF1Y3I5ZDdKRHZ6clhYWTU1WUZRIiwic2Vzc2lvblBvbGxpbmdVUkwiOiJodHRwOi8vbG9jYWxob3N0OjUwMDAvY2hlY2tvdXQvc2Vzc2lvbi9UdHF1Y3I5ZDdKRHZ6clhYWTU1WUZRIn0.ln7VzZkNvUGu0HhyA_a8IbXQN35WhDBmCYC9IvyYL-I",
  "checkoutFrontendUrl": "https://vippscheckout.vipps.no/v1/",
  "pollingUrl": "https://api.vipps.no/checkout/v1/session/31gf1g413121"
}
```

_special note:_ Do not hard code the URLs as shown in the response above as these are subject to change for version bump at any time.

## SDK guidelines

Vipps Checkout provides an SDK that is _very_ highly recommended. The SDK provides a simple and effective toolset, it also automatically manages many corner cases, alleviating complexity from your integration. For concrete guidelines consult our [Example Integration](#example-integration).

## Merchant hosting of Session

Next up you need to make a request from your client-side code to your own application's endpoint where you request the Vipps Checkout `token`. This `token` can then be used along with `checkoutFrontendUrl` to generate the Vipps Checkout iframe in your frontend.

_Special note:_ It is expected that the checkout frontend is not opened directly opened in it's own tab. The intended behavior is to open the session in an merchant hosted website inside an iframe using the SDK as described below.

## Example Integration

Here is an example integration written in JavaScript that will make a request to your back-end and embed an iframe:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Merchant</title>
    <script src="https://checkout.vipps.no/vippsCheckoutSDK.js"></script>
  </head>
  <body>
    <section id="merchant-order">
      <button type="button" id="checkout-button">Checkout with Vipps</button>
    </section>
    <section id="vipps-checkout-frame-container"></section>
    <script>
      var merchantBackendAppUrl =
        "<THE BACKEND OF THE MERCHANT TO RECEIVE CALLBACK>";

      // When clicking the "Checkout with Vipps" button
      document
        .getElementById("checkout-button")
        .addEventListener("click", function () {
          // Set the amount of the purchase here in oere
          var data = {
            amount: "1600",
          };

          // Call merchant backend which will again call Checkout backend to establish session
          fetch(merchantBackendAppUrl + "/create-checkout-session", {
            method: "POST",
            headers: {
              "Content-Type": "application/json",
              "Vipps-System-Name": "direct",
              "Vipps-System-Version": "1.0",
              "Vipps-System-Plugin-Name": "direct",
              "Vipps-System-Plugin-Version": "1.0",
            },
            body: JSON.stringify(data),
          })
            .then(function (response) {
              return response.json();
            })
            .then(function (data) {
              var vippsCheckout = VippsCheckout({
                checkoutFrontendUrl: data.checkoutFrontendUrl,
                iFrameContainerId: "vipps-checkout-frame-container",
                language: "no",
                token: data.token,
              });
            })
            .catch(function (error) {
              // Handle atlest these two types of errors here:
              // 1. Fetch to create session endpoint failed
              // 2. VippsCheckout SDK not loaded resulting in VippsCheckout not being defined
            });
        });
    </script>
  </body>
</html>
```

`VippsCheckout` comes from the SDK at `https://checkout.vipps.no/vippsCheckoutSDK.js` that got loaded in `<head>` (async loading of SDK not available). The SDK's purpose is to attach the iFrame to the given container element and load Vipps Checkout within it.

The object argument to `VippsCheckout`

```js
{
  checkoutFrontendUrl, // Will be supplied from our create session endpoint
    iFrameContainerId, // The id of the html element to contain the Checkout iFrame
    language; // Can be set to 'no' norwegian, or 'en' english. This is optional and will default to 'en' english if not specified
  token; // The token from create session endpoint that is specific to each checkout. Optional when using token as queryParam flow as described below.
}
```

#### Sticky checkout example using query parameters

The SDK provides an alternative flow to enable the use of a query parameter to keep the session "sticky", e.g. if you refresh the page. If the query parameter `token` is present, and the token attribute in the argument object to VippsCheckout is not defined, the SDK will load the iFrame with the token from the query parameter.

We provide a help method that when called will redirect to the current page but attach a `token` queryParameter to the URL.
Use it like this when receiving data from the create session endpoint to enable this feature. Note that the `VippsCheckout` initialization must be run outside of your fetch code if you use it in an event handler, or else the iFrame won't load.

```js
var vippsCheckout = VippsCheckout({
              checkoutFrontendUrl: data.checkoutFrontendUrl,
              iFrameContainerId: 'checkout-frame-container'
            });

// <Create session fetch function>
.then(function (data) {
  vippsCheckout.redirectToCurrentPageWithToken(data.token)
})
```

## Vipps Checkout Elements

With Vipps Checkout Elements you can adjust the fields and values present in the Checkout. For example: you might have a purchasing flow where you do not require an address because you are not sending physical goods, or you do not need the customer to identify itself because the customer is already logged into your system.

According to your needs the data collected can be adjusted. Specifically with the following fields added to the request as seen in our API spec [here](https://vippsas.github.io/vipps-checkout-api/).

```json
{
  "addressFields": false,
  "contactFields": false
}
```

These are by default set to `true`, and if not specified will return the full address and contact details in the Checkout session.

### AddressFields false example

If you do not need the address from a user you can disable them leading to the following personal details form

![Address_field_false_form](resources/addressfields_false_form.png)

And the following payment form

![Address_field_false_form](resources/addressfields_false_form_2.png)

### Addressfields and ContactFields false example

If you do not need the contact details for a customer you can disable them leading to the following session

![payment_only](resources/addressfields_false_contactfields_false.png)

### Combination with shipping

These options may be combined with shipping if it fits your scenario. For example leading to this session

![fields_false_with_shipping](resources/fields_false_with_shipping.png)

# System integration guidelines

## Integration partner and plugin guidelines

Vipps Checkout supports Partner key based authentication as described in our [ecom-api](https://github.com/vippsas/vipps-ecom-api/blob/master/vipps-ecom-api.md#partner-keys)

In the initiation request use your own credentials and send the Merchant-Serial-Number as described. Resulting in an on behalf of authentication if the Merchant has a valid connection to your solution.

### Partner signup API guidelines

If you are utilizing the [SignupAPI](https://github.com/vippsas/vipps-signup-api) you need to utilize the "Ecom access" key instead of using the default access token in the API calls. This API is considered deprecated by Vipps and should be migrated away from.

## System information guidelines

In order to fully utilize the conditions and support of the Vipps platform it is critical that you include all information regarding your system in the initiation headers as per the following example. **Important:** Please use self-explanatory, human readable and reasonably short values that uniquely identify the system (and plugin).

| Header                        | Description                                  | Example value       |
| ----------------------------- | -------------------------------------------- | ------------------- |
| `Merchant-Serial-Number`      | The merchant serial number                   | `123456`            |
| `Vipps-System-Name`           | The name of the ecommerce solution           | `woocommerce`       |
| `Vipps-System-Version`        | The version number of the ecommerce solution | `5.4`               |
| `Vipps-System-Plugin-Name`    | The name of the ecommerce plugin             | `vipps-woocommerce` |
| `Vipps-System-Plugin-Version` | The version number of the ecommerce plugin   | `1.4.1`             |

If any of these are not applicable for your integration please substitute with NA for Not Applicable.

## Transaction operations (Capture, Cancel, Refund)

Vipps Checkout should be considered an extension of existing other Vipps commerce functionality. This means that transaction operations other than payment initiation, which is handled by Checkout (see [Checkout Checklist](https://github.com/vippsas/vipps-checkout-api/blob/main/vipps-checkout-api-checklist.md)), should be done on the epayment API described [in their official docs](https://github.com/vippsas/vipps-epayment-api). A guideline for the integration can be found [here](https://github.com/vippsas/vipps-epayment-api/blob/main/docs/api/Getting-Started.md#getting-started-with-the-vipps-merchant-payments-api). You should use the same credentials as the ones you use with Checkout. 

Note that the Ecom API should not be used as it lacks full support for Card transactions. 

```
Special note: Vipps Checkout only supports "Reserve-Capture", if you are on a "Direct-Capture" setup, please seek assistance in accordance with the https://github.com/vippsas/vipps-developers/blob/master/contact.md#how-to-contact-vipps-integration guidelines.
```

## Polling integration

Vipps Checkout will expose a polling endpoint as described in our [swagger](https://vippsas.github.io/vipps-checkout-api/#/Session/get_v2_session__sessionId_).

```
It is very highly recommended for your system to combine both webhook and polling based integration. This combination helps prevent a lot of potential redirect edge cases as well as any reliability issues webhooks may come with. This provides a more seamless customer experience.

```

### Example of polling response when checkout SessionState is SessionStarted

Transaction information, User information and Shipping information are not available in this state of the session.

```json
{
  "sessionId": "eoIjaGeiZA8gqMNvr8uXxg",
  "orderId": "absbsbcb",
  "sessionState": "SessionStarted"
}
```

## Example of polling response when checkout SessionState any other state but not SessionStarted

```json
{
  "sessionId": "bnLxjxBDHiIi3JglEnohyw",
  "orderId": "471050523",
  "sessionState": "PaymentSuccessful",
  "transactionLogHistory": [
    {
      "amount": 1600,
      "transactionText": "Dummy transaction id",
      "timeStamp": "2021-09-24T11:51:36.939Z",
      "operation": "RESERVE",
      "operationSuccess": true,
      "transactionId": "5937913513"
    },
    {
      "amount": 1600,
      "transactionText": "Dummy transaction id",
      "timeStamp": "2021-09-24T11:51:23.181Z",
      "operation": "INITIATE",
      "operationSuccess": true,
      "transactionId": "5937913513"
    }
  ],
  "transactionSummary": {
    "capturedAmount": 0,
    "remainingAmountToCapture": 1600,
    "refundedAmount": 0,
    "remainingAmountToRefund": 0,
    "bankIdentificationNumber": 492560
  },
  "userDetails": {
    "firstName": "Test",
    "lastName": "Testesen",
    "phoneNumber": "4790000004",
    "email": "example@example.no"
  },
  "shippingDetails": {
    "firstName": "Test",
    "lastName": "Testesen",
    "streetAddress": "Stedesen 1",
    "postalCode": "0360",
    "region": "Oslo",
    "country": "NO"
  }
}
```

## Webhook integration

Vipps Checkout will then send a webhook to your defined URL as described in our [Webhooks example](#example-of-polling-response-and-webhook-notification). The webhook request will include all the details generated from the Checkout session.

The webhook will be sent to the following location.

`{callbackPrefix}` + `/checkout/`+ `{apiVersion}` + `/order/`+ `{orderId}`

Where `callbackPrefix`and `orderId`is defined when setting up the session.

Vipps demands that every notification webhook is responded to with a HTTP 202 response. In the eventuality that any other response is sent Vipps will retry with an exponential back off until 202 is received again. During this exponential back off Vipps will pause any new notifications until a 202 is returned on the original webhook notification. It is critical that the endpoint receiving the callback is robust. And can receive any additional data not specified in the minimum example and still be backwards compatible in accordance to our integration guidelines.

## Example of webhook notification

```json
{
  "merchantSerialNumber": "59474",
  "orderId": "393850103",
  "transactionInfo": {
    "amount": 1600,
    "status": "RESERVE",
    "timeStamp": null,
    "transactionId": "393850103"
  },
  "userDetails": {
    "firstName": "Test",
    "lastName": "Testesen",
    "phoneNumber": "4790000004",
    "email": "example@example.no"
  },
  "shippingDetails": {
    "firstName": "Test",
    "lastName": "Testesen",
    "streetAddress": "Stedesen 1",
    "postalCode": "0360",
    "region": "OSLO",
    "country": "NO"
  }
}
```

## Shipping

We offer two methods of providing shipping methods - static and dynamic.

With static shipping, the available shipping methods are provided as part of the session initiation call. With dynamic shipping, you provide a url on session initiation that responds to a callback with shipping options. If neither shipping options nor a dynamic shipping callback url is provided during session initiation, the customer will not be given any shipping options in the Checkout.

### Static Shipping

Static shipping entails a flat rate per shipping option regardless of the customer's address.
We show a title, price and optional description and the ability to show optional logo from a limited set of logos of the most popular shipping providers.

ShippingOptions are provided in the create session endpoint. See [Swagger documentation for more details](https://vippsas.github.io/vipps-checkout-api/#/Session/post_v2_session)

```json
"logistics": {
  "fixedOptions": [
    {
      "id": "string",
      "isDefault": true,
      "priority": 0,
      "brand": "string",
      "product": "string",
      "description": "string"
      "amount": {
        "currency": "string",
        "value": 0
      },
    }
  ]
}

```

- `id` will be the unique identifier for the shipping option, and will be returned to you in the callback and polling endpoint.
- `isDefault` is the option pre-checked for the customer. Only one option should have this as true.
- `priority` allows you to specify the order of your options explicitly by ascending order. Should be provided as an integer.
- `brand` is the logistics provider. This is used to display a logo next to the shipping option. Currently `"posten", "helthjem" and "postnord"` logos are supported. If none of these are given, a generic logo will be displayed instead.
- `product` is the shipping option product name, and is typically used to distinguish different options that the logistics providers offer. Such as "pick-up in store", "home delivery" and "mailbox". This will be displayed in the title of the shipping option.
- `description` is an optional explaining text that will show under the price. This can typically include estimates of delivery or other information.
- `amount.currency` is the currency identificator according to ISO 4217.
- `amount.value` is the amount in the lowest currency subdivision (øre/oere for NOK) as an integer.

### Dynamic Shipping

Shipping options can be calculated on the basis of shipping address. To support this, Checkout sends a callback as a POST to a merchant endpoint. The merchant endpoint is provided in the session initiation in the `logistics.dynamicOptionsCallback` field. If this field is null, dynamic shipping will not be used.
The callback is as follows:

```json
{
  "streetAddress": "string",
  "postalCode": "string",
  "region": "string",
  "country": "string"
}
```

- `streetAddress` is the street address.
- `postalCode` is the zip code.
- `region` is the region.
- `country` is the country.

The merchant endpoint should return a list of shipping options on the same structure as for `fixedOptions` (see section on [Static Shipping](#static-shipping)).

```json
[
  {
    "id": "string",
    "isDefault": true,
    "priority": 0,
    "brand": "string",
    "product": "string",
    "description": "string"
    "amount": {
      "currency": "string",
      "value": 0
    },
  }
]
```

We strongly recommend merchants to also send in fallback shipping options in `logistics.fixedOptions` in case the callback fails for some reason.

The fallback options will be used in case the callback

- does not resolve successfully within 8 seconds
- returns `null`
- returns an empty list

If no fallback options are provided the user will be presented with an error and cannot continue with the checkout.

## Vipps side Transaction information

You might want to find the Vipps transactionId value of a transaction. For example if you want to utilize Vipps assisted content monitoring as described [here](https://github.com/vippsas/vipps-order-management-api/blob/main/vipps-order-management-api.md#vipps-assited-content-monitoring) or you might want to support account procedures.

### Recommended integration (currently in pilot mode)

In order to integrate with the receipts functionality you need to retrieve the pspreference in the "paymentAction": "AUTHORISATION" event from the [eventlog](https://vippsas.github.io/vipps-epayment-api/index.html#operation/getPaymentEventLog) endpoint. This is required if utilizing receipts with Vipps Checkout or Free standing card payments

## In-App Payment

Vipps Checkout can be used inside a Webview in an iOS or Android app to pay for goods and services.

If you wish to have a `ReturnUrl` based on an app url (i.e app://payment/{id}) instead of an https url there are certain requirements. In order to ensure that Checkout always works correctly in a WebView the session must be initiated with the UserFlow property set to "NATIVE_REDIRECT". This ensures that all users are redirected properly to the Vipps app and back to your app after when paying.
