<!-- START_METADATA
---
title: Quick start for the Checkout API
sidebar_label: Quick start
sidebar_position: 5
@@ -8,6 +8,7 @@ toc_min_heading_level: 2
toc_max_heading_level: 5
pagination_next: null
pagination_prev: null
---

import ApiSchema from '@theme/ApiSchema';
@@ -21,8 +22,10 @@ END_METADATA -->
Use the Checkout API to create a checkout session and retrieve session information.

<!-- START_COMMENT -->
ℹ️ Please use the website:
[Vipps MobilePay Technical Documentation](https://developer.vippsmobilepay.com/docs/APIs/checkout-api>).
<!-- END_COMMENT -->

## Before you begin
@@ -44,10 +47,10 @@ your test credentials from the merchant portal.
You will need the following values, as described in the
[Getting started guide](https://developer.vippsmobilepay.com/docs/getting-started):

* `client_id` - Client_id for a test sales unit.
* `client_secret` - Client_id for a test sales unit.
* `Ocp-Apim-Subscription-Key` - Subscription key for a test sales unit.
* `Merchant-Serial-Number` - The unique ID for a test sales unit.

<Tabs
defaultValue="curl"
@@ -60,13 +63,13 @@ values={[

In Postman, import the following files:

* [Checkout API Postman collection](/tools/checkout-api-postman-collection.json)
* [Global Postman environment](https://github.com/vippsas/vipps-developers/blob/master/tools/vipps-api-global-postman-environment.json)

🔥 **To reduce risk of exposure, never store production keys in Postman or any similar tools.** 🔥

Update the *Current Value* field in your Postman environment with your **Merchant Test** keys.
Use *Current Value* field for added security, as these values are not synced to the cloud.

</TabItem>
<TabItem value="curl">
@@ -134,7 +137,7 @@ curl https://apitest.vipps.no/checkout/v3/session \
Take note of the `reference` value, as it can be used for subsequent calls relating to this session.

To display the session, you will need to load the Checkout SDK in your website, as described in
[API Guide: Displaying the session](checkout-api.md#step-2-displaying-the-session).

### Step 3 - Retrieve the session information

Retrieve the session information by using
[`GET:/checkout/v3/session/{reference}`][get-session-endpoint] with `reference` from the previous step.
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