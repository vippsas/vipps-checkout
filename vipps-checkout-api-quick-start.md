<!-- START_METADATA
---
title: Quick start for the Checkout API
sidebar_label: Quick start
sidebar_position: 5
description: Quick steps for getting started with the Checkout API.
toc_min_heading_level: 2
toc_max_heading_level: 5
pagination_next: null
pagination_prev: null
---

import ApiSchema from '@theme/ApiSchema';
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

END_METADATA -->

# Quick start

Use the Checkout API to create a checkout session and retrieve session information.

<!-- START_COMMENT -->
ℹ️ Please use the website:
[Vipps MobilePay Technical Documentation](https://developer.vippsmobilepay.com/docs/APIs/checkout-api>).
<!-- END_COMMENT -->

## Before you begin

The examples use standard example values that you must change to
use *your* values. This includes API keys, HTTP headers, reference, etc.
Note that any currency amount must be an Integer value minimum 100 in øre.

**A note on errors:** An endpoint may return a non-successful response code for many reasons, including invalid API keys, missing fields in an input, etc.
When errors occur, a response based on <https://tools.ietf.org/html/rfc7807> will be returned. The message format may evolve, so avoid building strict logic around it.

## Your first Checkout

### Step 1 - Setup

You must have already signed up as an organization with Vipps MobilePay and have
your test credentials from the merchant portal.

You will need the following values, as described in the
[Getting started guide](https://developer.vippsmobilepay.com/docs/getting-started):

* `client_id` - Client_id for a test sales unit.
* `client_secret` - Client_id for a test sales unit.
* `Ocp-Apim-Subscription-Key` - Subscription key for a test sales unit.
* `Merchant-Serial-Number` - The unique ID for a test sales unit.

<Tabs
defaultValue="curl"
groupId="sdk-choice"
values={[
{label: 'curl', value: 'curl'},
{label: 'Postman', value: 'postman'},
]}>
<TabItem value="postman">

In Postman, import the following files:

* [Checkout API Postman collection](/tools/vipps-checkout-api-postman-collection.json)
* [Global Postman environment](https://github.com/vippsas/vipps-developers/blob/master/tools/vipps-api-global-postman-environment.json)

🔥 **Don't store production keys in the cloud version of Postman, because this introduces a risk of exposure.** 🔥

Update the *Current Value* field in your Postman environment with your **Merchant Test** keys.
Use *Current Value* field for added security, as these values are not synced to the cloud.

</TabItem>
<TabItem value="curl">

No additional setup needed :)

</TabItem>
</Tabs>

### Step 2 - Create a checkout session

Create a checkout session with: [`POST:/v3/session`][create-session-endpoint].

<Tabs
defaultValue="curl"
groupId="sdk-choice"
values={[
{label: 'curl', value: 'curl'},
{label: 'Postman', value: 'postman'},
]}>
<TabItem value="postman">

```bash
Send request Create a Checkout Session
```

The `reference` variable is automatically set in the environment
of this Postman example.

</TabItem>
<TabItem value="curl">

```bash
curl https://apitest.vipps.no/checkout/v3/session \
-H "Content-Type: application/json" \
-H "client_id: YOUR-CLIENT-ID" \
-H "client_secret: YOUR-CLIENT-SECRET" \
-H "Ocp-Apim-Subscription-Key: YOUR-SUBSCRIPTION-KEY" \
-H "Merchant-Serial-Number: YOUR-MSN" \
-H "Vipps-System-Name: YOUR-COMPANY-NAME" \
-H "Vipps-System-Version: 3.1.2" \
-H "Vipps-System-Plugin-Name: YOUR-PLUGIN-NAME" \
-H "Vipps-System-Plugin-Version: 4.5.6" \
-X POST \
-d '{
  "merchantInfo": {
    "callbackUrl": "https://example.com/vipps/callbacks-for-checkout",
    "returnUrl": "https://example.com/vipps/fallback-result-page-for-both-success-and-failure/acme-shop-123-order123abc",
    "callbackAuthorizationToken": "538dd1d0-9e7f-4732-8134-dfed7fd0b236"
  },
  "transaction": {
    "amount": {
      "value": 1000,
      "currency": "NOK"
    },
    "reference": "UNIQUE-SESSION-REFERENCE",
    "paymentDescription": "One pair of socks."
  }
}'
```

</TabItem>
</Tabs>

Take note of the `reference` value, as it can be used for subsequent calls relating to this session.

To display the session, you will need to load the Checkout SDK in your website, as described in
[API Guide: Displaying the session](vipps-checkout-api.md#step-2-displaying-the-session).

### Step 3 - Retrieve the session information

Retrieve the session information by using
[`GET:/v3/session/{reference}`][get-session-endpoint] with `reference` from the previous step.

<Tabs
defaultValue="curl"
groupId="sdk-choice"
values={[
{label: 'curl', value: 'curl'},
{label: 'Postman', value: 'postman'},
]}>
<TabItem value="postman">

```bash
Send request Get session info
```

You will see the details appear in the lower pane.

</TabItem>
<TabItem value="curl">

```bash
curl https://apitest.vipps.no/checkout/v3/session/UNIQUE-SESSION-REFERENCE \
-H "Content-Type: application/json" \
-H "client_id: YOUR-CLIENT-ID" \
-H "client_secret: YOUR-CLIENT-SECRET" \
-H "Ocp-Apim-Subscription-Key: YOUR-SUBSCRIPTION-KEY" \
-H "Merchant-Serial-Number: YOUR-MSN" \
-H "Vipps-System-Name: acme" \
-H "Vipps-System-Version: 3.1.2" \
-H "Vipps-System-Plugin-Name: acme-webshop" \
-H "Vipps-System-Plugin-Version: 4.5.6" \
-X GET
```

</TabItem>
</Tabs>

[create-session-endpoint]: https://developer.vippsmobilepay.com/api/checkout#tag/Session/paths/~1v3~1session/post
[get-session-endpoint]: https://developer.vippsmobilepay.com/api/checkout#tag/Session/paths/~1v3~1session~1%7Breference%7D/get
