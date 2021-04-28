# Vipps Checkout guide

VipVipps Checkout is designed to be a low friction low complexity flow where Vipps Checkout ensures a smooth and efficient checkout experience using the trusted Vipps technology and brand.

Preliminary documentation. Subject to change

- [Vipps Checkout guide](#vipps-checkout-guide)
- [Flow diagram](#flow-diagram)
- [Example integration](#example-integration)
- [Webhooks](#webhooks)

# Flow diagram

The standard flow for a Vipps Checkout consists of

1. Generation of a session.
2. Displaying that session in a Vipps Checkout iframe.
3. The user completing checkout process.
4. The Merchant handling the result of the Checkout process.

![Checkout flow](resources/standard_flow.png)

# Example integration

First you need to request a Vipps Checkout Session token from the Vipps APIs. The first thing you need to do is set up a server server request to set up a Checkout session. An example implementation can be found [in the example-integration](#example-integration) 

Request a session token acording to your needs, the full specification of the Checkout session endpoint can be found [here](https://fantastic-fiesta-211ff2ad.pages.github.io/#/Checkout/get_Checkout)

A minimal example with example values:

```json
{
    "authInfo": {
        "clientId": "dddd",
        "clientSecret": "ss",
        "ocpApimSubscriptionKey": "ss"
    },
    "merchantInfo": {
        "merchantSerialNumber": "123456",
        "callbackPrefix":"https://example.com/vipps/callbacks-for-ecom-payment-update",
        "checkOutWebhookUrl": "https://example.com/vipps",
        "fallBackUrl": "https://example.com/vipps/fallback-result-page/order123abc"
    },
    "amount": {
        "currency": "NOK",
        "amount": 20000
    },
    "scopes": "",
    "reference" : "31gf1g413121",
    "transactionText": "One pair of Vipps socks"
}
```

Example response:

```json
{
  "sessionID": "fjepo1393_31f01f109d213",
  "checkoutFrontendUrl": "https://vippscheckoutprod.z6.web.core.windows.net/"
  "pollingUrl": "https://api.vipps.no/checkout/fjepo1393_31f01f109d213"
}
```


Then set up the iframe to host the Vipps Checkout in your frontend

```
<script>
      var merchantBackendUrl = '<%= merchantBackendUrl %>';
      var checkoutFrontendUrl = '<%= checkoutFrontendUrl %>';
      var frameContainer = document.getElementById('checkout-frame-container');

      var iframe = document.createElement('iframe');

      document.getElementById('checkout-button').addEventListener('click', function () {
        var data = {
          amount: '1600000',
        };

        fetch(merchantBackendUrl + '/create-checkout-session', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify(data),
        })
          .then((response) => response.json())
          .then((data) => {
            iframe.src = checkoutFrontendUrl + '/?token=' + data.token;
            iframe.classList.add('checkout-frame');
            iframe.frameBorder = '0';
            iframe.height = '1500px';
            iframe.id = frameContainer.appendChild(iframe);
          })
          .catch((error) => {
            console.error('Error:', error);
          });
      });
    </script>
```

Insert the token into the iframe and display it to the Customer. 

The user then completes the session in the Iframe and gets redirected back to the fallBackUrl if applicable.

Vipps Checkout will then send a webhook to your defined URL. 

The webhook request will include all the details generated from the Checkout session. 

Example of a standard Checkout session result where the account is set for direct capture.

```json

{
    "merchantAccount": "string",
    "redirectUrl": "https://landing.vipps.no?token=abc123",
    "reference": "reference-string",
    "returnUrl": "https://example.io/redirect?orderId=abcc123",
  "userinfo": {
    "sub": "c06c4afe-d9e1-4c5d-939a-177d752a0944",
    "birthdate": "1815-12-10",
    "email": "user@example.com",
    "email_verified": true,
    "nin": "10121550047",
    "name": "Ada Lovelace",
    "given_name": "Ada",
    "family_name": "Lovelace",
    "sid": "7d78a726-af92-499e-b857-de263ef9a969",
    "phone_number": "4712345678",
    "address": {
        "street_address": "Suburbia 23",
        "postal_code": "2101",
        "region": "OSLO",
        "country": "NO",
        "formatted": "Suburbia 23\\n2101 OSLO\\nNO",
        "address_type": "home"
        }
    },
  "Transaction": {
    "aggregate": {
        "authorizedAmount": {
        "currency": "NOK",
        "type": "PURCHASE",
        "value": 1000
        },
        "capturedAmount": {
        "currency": "NOK",
        "type": "PURCHASE",
        "value": 1000
        },
    },
    "amount": {
        "currency": "NOK",
        "type": "PURCHASE",
        "value": 1000
    },
    "authorisationType": "FINAL_AUTH",
    "authorised": true,
    "directCapture": true,
    "customerInteraction": "CUSTOMER_NOT_PRESENT",
    "paymentMethod": {
        "type": "WALLET",
    }
  }
}
```

# Webhooks

Our webhooks will have the following policy with the following backoff routine: --------

If you need to find 