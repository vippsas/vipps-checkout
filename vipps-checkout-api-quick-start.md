<!-- START_METADATA
---
title: Quick start for the Checkout API
sidebar_label: Quick start
id: quick-start
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

💥 Please use the documentation pages here: <https://developer.vippsmobilepay.com/docs/APIs/checkout-api>. 💥

<!-- END_COMMENT -->

## Before you begin

This document covers the quick steps for getting started with the ePayment API.
You must have already signed up as an organization with Vipps MobilePay and have
your test credentials from the merchant portal, as described in the
[Getting started guide](https://developer.vippsmobilepay.com/docs/getting-started).

**Important:** The examples use standard example values that you must change to
use *your* values. This includes API keys, HTTP headers, reference, etc.

## Your first Payment

### Step 1 - Setup

<Tabs
defaultValue="curl"
groupId="sdk-choice"
values={[
{label: 'curl', value: 'curl'},
{label: 'Postman', value: 'postman'},
]}>
<TabItem value="postman">

**Please note:** Postman is discontinuing their offline version. Use only your test keys and delete them after testing.
Ensure that your company allows for cloud use before continuing.

If you wish to use Postman, import the following files:

* [Checkout API Postman collection](/tools/vipps-checkout-api-postman-collection.json)
* [Global Postman environment](https://github.com/vippsas/vipps-developers/blob/master/tools/vipps-api-global-postman-environment.json)

In Postman, tweak the environment with your own values (see
[API keys](https://developer.vippsmobilepay.com/docs/common-topics/api-keys/)):

* `client_id` - Merchant key required for getting the access token.
* `client_secret` - Merchant key required for getting the access token.
* `Ocp-Apim-Subscription-Key` - Merchant subscription key.
* `merchantSerialNumber` - Merchant ID.
* `internationalMobileNumber` - The MSISDN for the test app profile you have received or registered. This is your test mobile number *including* country code.

You can update any of the other environment variables. Be aware of this:

* Any currency amount must be an Integer value minimum 100 in øre.
* Most URLs must be `https`.

</TabItem>
<TabItem value="curl">

No setup needed :)

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
-H "Merchant-Serial-Number: 123456" \
-H "Vipps-System-Name: acme" \
-H "Vipps-System-Version: 3.1.2" \
-H "Vipps-System-Plugin-Name: acme-webshop" \
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
-H "Merchant-Serial-Number: 123456" \
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
