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
  - [Webhook integration](#webhook-integration)
  - [Example of polling response and webhook notification](#example-of-polling-response-and-webhook-notification)

# Flow diagram

The standard flow for a Vipps Checkout consists of

1. Generating a Checkout session.
2. Displaying that session in a Vipps Checkout iframe.
3. The user completing checkout process.
4. The Merchant handles the result of the Checkout process.

![Checkout flow](resources/standard_flow.png)

# Example integration

First you need to request a Vipps Checkout Session token from the Vipps APIs. The first thing you need to do is set up a server server request to set up a Checkout session. An example implementation can be found [in the example-integration](#example-integration) 

Request a session token according to your needs, the full specification of the Checkout session endpoint can be found [here](https://fantastic-fiesta-211ff2ad.pages.github.io/#/Checkout/get_Checkout)

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
        "checkOutWebhookUrl": "https://example.com/vipps" //Will overwrite configuration on Merchant Profile
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
  "pollingUrl": "https://api.vipps.no/checkout/31gf1g413121"
}
```

*special note:* Do not hard code the URLs as shown in the response above as these are subject to change for version bump at any time.

Next up you need to make a request from your client-side code to your own application's endpoint where you request the Vipps Checkout `token`. This `token` can then be used along with `checkoutFrontendUrl` to generate the Vipps Checkout iframe in your frontend.

Here is an example integration written in JavaScript that will make a request to your back-end and embed an iframe:

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <meta
      name="viewport"
      content="width=device-width, initial-scale=0.86, maximum-scale=5.0, minimum-scale=0.86"
    />
    <title>Merchant</title>
    <script src="https://polyfill.io/v3/polyfill.min.js?version=3.52.1&features=fetch"></script>
  </head>
  <body>
    <section id="merchant-order">    
      <button type="button" id="checkout-button">Checkout with Vipps</button>
    </section>
    <section id="vipps-checkout-frame-container"></section>
    <script>
      const merchantBackendAppUrl = '<THE BACKEND OF THE MERCHANT TO RECEIVE CALLBACK>'; // replace me!

      const token = getParameterByName('token');
      const checkoutFrontendUrl = window.localStorage.getItem(`vipps-checkout-${token}`);

      // Setup iframe element and container to hold the iframe
      const frameContainer = document.getElementById('vipps-checkout-frame-container');
      const iframe = document.createElement('iframe');
      const iframeId = 'vipps-checkout-iframe';
      iframe.frameBorder = '0';
      iframe.width = '100%';

      // The height of the iframe is communicated from the iframe according to it's content
      window.addEventListener(
        'message',
        (e) => {
          if (e.origin === checkoutFrontendUrl) {
            if (e.data.hasOwnProperty('frameHeight')) {
              document.getElementById(iframeId).style.height = `${e.data.frameHeight}px`;
            }
          }
        },
        false,
      );

      // If token query parameter present, we don't need to start a new session and load the current one.
      if (token) {
        iframe.src = `${checkoutFrontendUrl}/?token=${token}`;
        iframe.id = iframeId;
        frameContainer.appendChild(iframe);
      }

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
          },
          body: JSON.stringify(data),
        })
          .then(function (response) {
            return response.json();
          })
          .then(function (data) {
            // Set token in URL and update the address field of the browser.
            window.localStorage.setItem('vipps-checkout-' + data.token, data.checkoutFrontendUrl);
            window.location.href = updateQueryStringParameter('token', data.token);
          })
          .catch(function (error) {
            console.error('Error:', error);
          });
      });

      // Helper functions
      function updateQueryStringParameter(key, value) {
        const url = new URL(window.location.href);
        const urlParams = url.searchParams;

        urlParams.append(key, value);
        url.search = urlParams.toString();

        return url.toString();
      }

      function getParameterByName(name) {
        const queryString = window.location.search;
        const urlParams = new URLSearchParams(queryString);

        return urlParams.get(name);
      }
    </script>
  </body>
</html>

```

Insert the token into the iframe and display it to the Customer. 

The user then completes the session in the Iframe and gets redirected back to the fallBackUrl if applicable.

# System integration guidelines

## Integration partner and plugin guidelines
Vipps Checkout supports Partner key based authentication as described in our [ecom-api](https://github.com/vippsas/vipps-ecom-api/blob/master/vipps-ecom-api.md#partner-keys)

In the initiation request use your own credentials and send the Merchant-Serial-Number as described. Resulting in an on behalf of authentication if the Merchant as a valid connection to your solution.

### System information guidelines
In order to fully utilize the conditions and support of the Vipps platform it is critical that you include all information regarding your system in the initiation headers as per the following example

```
Vipps-System-Name: Acme Enterprises Ecommerce DeLuxe
Vipps-System-Version: 3.1.2
Vipps-System-Plugin-Name: Point Of Sale Excellence
Vipps-System-Plugin-Version: 4.5.6
Content-Type: application/json
```

## Polling integration

Vipps Checkout will expose a polling enpoint as described in our [swagger](https://fantastic-fiesta-211ff2ad.pages.github.io/#/). TODO: Oppdater

```
It is very highly recommended for your system to combine both webhook and polling based integration. This combination helps prevent a lot of potential redirect edge cases as well as any reliability issues webhooks may come with. This provides a more seamless customer experience.
```

## Webhook integration
Vipps Checkout will then send a webhook to your defined URL as described in our [Webhooks example](#example-of-polling-response-and-webhook-notification). The webhook request will include all the details generated from the Checkout session. 

Example of a standard Checkout session result where the account is set for direct capture.

Vipps demands that every notification webhook is responded to with a HTTP 202 response. In the eventuality that any other response is sent Vipps will retry with an exponential back off until 202 is received again. During this exponential back off Vipps will pause any new notifications until a 202 is returned on the original webhook notification. 


## Example of polling response and webhook notification

```json

{
  "merchantAccount": "string",
  "redirectUrl": "https://landing.vipps.no?token=abc123",
  "reference": "reference-string",
  "returnUrl": "https://example.io/redirect?orderId=abcc123",
  "userinfo": {
    "email": "user@example.com",
    "name": "Ada Lovelace",
    "given_name": "Ada",
    "family_name": "Lovelace",
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
    "paymentMethod": {
      "type": "WALLET",
    }
  }
}
```

