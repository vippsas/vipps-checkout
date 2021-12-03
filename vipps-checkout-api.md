# Vipps Checkout guide

Vipps Checkout is designed to be a low friction, low complexity flow where Vipps Checkout ensures a smooth and efficient checkout experience using the trusted Vipps technology and brand.

Preliminary documentation. Subject to change

- [Vipps Checkout guide](#vipps-checkout-guide)
- [Flow diagram](#flow-diagram)
- [Example integration](#example-integration)
- [System integration guidelines](#system-integration-guidelines)
  - [Integration partner and plugin guidelines](#integration-partner-and-plugin-guidelines)
    - [System information guidelines](#system-information-guidelines)
  - [Polling integration](#polling-integration)
  - [Example of polling response when checkout SessionState any other state but not SessionStarted](#example-of-polling-response-when-checkout-sessionstate-any-other-state-but-not-sessionstarted)
  - [Webhook integration](#webhook-integration)
  - [Example of webhook notification](#example-of-webhook-notification)
  - [Shipping](#shipping)

# Flow diagram

The standard flow for a Vipps Checkout consists of

1. Generating a Checkout session.
2. Displaying that session in a Vipps Checkout iframe.
3. The user completing checkout process.
4. The Merchant handles the result of the Checkout process.

![Checkout flow](resources/standard_flow.png)

# Example integration

First you need to request a Vipps Checkout Session token from the Vipps APIs. The first thing you need to do is set up a server to server request to set up a Checkout session. An example implementation can be found [in the example-integration](#example-integration) 

Request a session token according to your needs, the full specification of the Checkout session endpoint can be found [here](https://vippsas.github.io/vipps-checkout/#/Session/post_session)

A minimal example with example values:

Request headers:
Make sure to pass the below mandatory headers in the request that you send from your server to Vipps checkout create session endpoint.

```json
{
  "Vipps-System-Name" : "Acme Enterprises Ecommerce DeLuxe",
  "Vipps-System-Version": "3.1.2",
  "Vipps-System-Plugin-Name": "Point Of Sale Excellence",
  "Vipps-System-Plugin-Version": "4.5.6",
  "Client_Id": "<merchant_client_id>",
  "Client_Secret": "<merchant_client_secret>",
  "Ocp-Apim-Subscription-Key": "<Ocp-Apim-Subscription-Key>"
}
```

Request body:

```json
{
    "merchantInfo": {
        "merchantSerialNumber": "123456",
        "fallBackUrl": "https://example.com/vipps", //Will overwrite configuration on Merchant Profile
        "callbackPrefix": "https://example.com/vipps/callbacks-for-payment-updates",
        "callbackAuthorizationToken": "iOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImllX3FXQ1hoWHh0MXpJ"
    },
    "transaction": {
        "currency": "NOK",
        "amount": 20000, //Must be in Minor Units. The smallest unit of a currency.
        "transactionText": "One pair of Vipps socks",
        "orderId" : "31gf1g413121"
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

*special note:* Do not hard code the URLs as shown in the response above as these are subject to change for version bump at any time.

Next up you need to make a request from your client-side code to your own application's endpoint where you request the Vipps Checkout `token`. This `token` can then be used along with `checkoutFrontendUrl` to generate the Vipps Checkout iframe in your frontend.

Here is an example integration written in JavaScript that will make a request to your back-end and embed an iframe:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Merchant</title>
    <script src="https://vippscheckoutprod.z6.web.core.windows.net/vippsCheckoutSDK.js></script>
  </head>
  <body>
    <section id="merchant-order">    
      <button type="button" id="checkout-button">Checkout with Vipps</button>
    </section>
    <section id="vipps-checkout-frame-container"></section>
    <script>
      var merchantBackendAppUrl = '<THE BACKEND OF THE MERCHANT TO RECEIVE CALLBACK>';

      // When clicking the "Checkout with Vipps" button
      document.getElementById('checkout-button').addEventListener('click', function () {

        // Set the amount of the purchase here in oere
        var data = {
          amount: '1600',
        };
        
        // Call merchant backend which will again call Checkout backend to establish session
        fetch(merchantBackendAppUrl + '/create-checkout-session', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
            'Vipps-System-Name': 'direct',
            'Vipps-System-Version': '1.0',
            'Vipps-System-Plugin-Name': 'direct',
            'Vipps-System-Plugin-Version': '1.0',             
          },
          body: JSON.stringify(data),
        })
          .then(function (response) {
            return response.json();
          })
          .then(function (data) {
            var vippsCheckout = VippsCheckout({
              checkoutFrontendUrl: data.checkoutFrontendUrl,
              iFrameContainerId: 'checkout-frame-container',
              token: data.token
            });
          })
          .catch(function (error) {
            console.error('Error:', error);
          });
      });
    </script>
  </body>
</html>

```

`VippsCheckout` comes from the SDK that got loaded in `<head>` (async loading of SDK not available). The SDK's purpose is to attatch the iFrame to the given container element and load Vipps Checkout within it.

The object argument to `VippsCheckout`
```js
{
  checkoutFrontendUrl, // Will be supplied from our create session endpoint
  iFrameContainerId, // The id of the html element to contain the Checkout iFrame
  token // The token from create session endpoint that is specific to each checkout. (Optional when using token as queryParam flow as described below) 
}
```
The SDK provides an alternative flow to enable the use of a query parameter to keep the session "sticky", e.g. if you refresh the page. If the query parameter `token` is present, and the token attribute in the argument object to VippsCheckout is not defined, the SDK will load the iFrame with the token from the query parameter.

We provide a help method that when called will redirect to the current page but attatch a `token` queryParameter to the URL.
Use it like this when receiving data from the create session endpoint to enable this feature.

```js
.then(function (data) {
  var vippsCheckout = VippsCheckout({
              checkoutFrontendUrl: data.checkoutFrontendUrl,
              iFrameContainerId: 'checkout-frame-container'
            });
  vippsCheckout.redirectToCurrentPageWithToken(data.token)
})
```

# System integration guidelines

## Integration partner and plugin guidelines
Vipps Checkout supports Partner key based authentication as described in our [ecom-api](https://github.com/vippsas/vipps-ecom-api/blob/master/vipps-ecom-api.md#partner-keys)

In the initiation request use your own credentials and send the Merchant-Serial-Number as described. Resulting in an on behalf of authentication if the Merchant as a valid connection to your solution.

### System information guidelines
In order to fully utilize the conditions and support of the Vipps platform it is critical that you include all information regarding your system in the initiation headers as per the following example. **Important:** Please use self-explanatory, human readable and reasonably short values that uniquely identify the system (and plugin).


| Header                        | Description                                  | Example value       |
| ----------------------------- | -------------------------------------------- | ------------------- |
| `Merchant-Serial-Number`      | The merchant serial number                   | `123456`            |
| `Vipps-System-Name`           | The name of the ecommerce solution           | `woocommerce`       |
| `Vipps-System-Version`        | The version number of the ecommerce solution | `5.4`               |
| `Vipps-System-Plugin-Name`    | The name of the ecommerce plugin             | `vipps-woocommerce` |
| `Vipps-System-Plugin-Version` | The version number of the ecommerce plugin   | `1.4.1`             |

If any of these are not applicable for your integration please substitute with NA for Not Applicable.

## Polling integration

Vipps Checkout will expose a polling enpoint as described in our [swagger](https://vippsas.github.io/vipps-checkout/#/Session/get_session__sessionId_).

```
It is very highly recommended for your system to combine both webhook and polling based integration. This combination helps prevent a lot of potential redirect edge cases as well as any reliability issues webhooks may come with. This provides a more seamless customer experience.

## Example of polling response when checkout SessionState is SessionStarted. Transaction information, User information and Shipping information are not available in this state of the session.

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

`{callbackPrefix}` + `/checkout/v1/order/`+ `{orderId}`

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
Per now we offer a static shipping feature where you can specify shipping options for the users in our create session API endpoint. Static shipping means a flat rate per shipping option regardless of the customer's address.
We show a title, price and optional description and the ability to show optional logo from a limited set of logos from the most popular shipping providers.

ShippingOptions are provided in the create session endpoint. See [Swagger documentation for more details](https://vippsas.github.io/vipps-checkout/#/Session/post_session)

```json
"ShippingOptions": [
    {
      "IsDefault": true,
      "Priority": 0,
      "ShippingCost": 0,
      "ShippingMethod": "string",
      "ShippingMethodId": "string",
      "ShippingMethodLogoId": "string",
      "Description": "string"
    }
  ]
```
- `IsDefault` is the option pre-checked for the customer. Only one option should have this as true.
- `Priority` allows you to specify the order of your options explicitly by ascending order.
- `ShippingCost` is the amount in oere.
- `ShippingMethod` is the title of the shipping option
- `ShippingMethodId` will be the unique identifier for the shipping option, and will be returned to you in the callback and polling endpoint.
- `ShippingMethodLogoId` shows the logo of the logistics provider. Can be either of these `"posten", "helthjem", "postnord"`.
- `Description` is an optional explaining text that will show under the price. This can typically include estimates of delivery or other information. 
