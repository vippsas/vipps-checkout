<!-- START_METADATA
---
title: Quick start for the Checkout API
sidebar_label: Quick start
sidebar_position: 5
description: Quick start guide for the using the Checkout API with Postman.
pagination_next: null
pagination_prev: null
---
END_METADATA -->

# Quick start

Use the Checkout API to create a checkout session and retrieve session information.

<!-- START_COMMENT -->

💥 Please use the documentation pages here: <https://developer.vippsmobilepay.com/docs/APIs/checkout-api>. 💥

<!-- END_COMMENT -->

## Postman

### Prerequisites

Review
[Quick start guides](https://developer.vippsmobilepay.com/docs/vipps-developers/quick-start-guides) for information about getting your test environment set up.

### Step 1: Get the collection and environment

Save the following files to your computer:

* [Checkout API Postman collection](/tools/vipps-checkout-api-postman-collection.json)
* [Global Postman environment](https://raw.githubusercontent.com/vippsas/vipps-developers/master/tools/vipps-api-global-postman-environment.json)

### Step 2: Import the Postman files

1. In Postman, click *Import* in the upper-left corner.
1. In the dialog that opens, with *File* selected, click *Upload Files*.
1. Select the two files you have just downloaded and click *Import*.

### Step 3: Set up Postman environment

1. Click the down arrow, next to the "eye" icon in the top-right corner, and select the environment you have imported.
1. Click the "eye" icon and, in the dropdown window, click `Edit` in the top-right corner.
1. Fill in the `Current Value` for the following fields to get started. For the first keys, go to *Vipps Portal* > *Utvikler* > *Test Keys*.
   * `client_id` - Merchant key is required for getting the access token.
   * `client_secret` - Merchant key is required for getting the access token.
   * `Ocp-Apim-Subscription-Key` - Merchant subscription key.
   * `merchantSerialNumber` - Merchant ID.
   * `internationalMobileNumber` - The mobile number for the test app profile you have received or registered, including country code. Do not include the `+`.

You can update any of the other environment variables. Be aware of this:

* Any currency amount must be an Integer value minimum 100 in øre.
* Most URLs must be `https`.

## Make API calls

### Create a checkout session

1. Send request `Create a Checkout Session`. This starts a simple session by using
   [`POST:/v3/session`](https://developer.vippsmobilepay.com/api/checkout#tag/Session/paths/~1v3~1session/post).

   The `reference` variable is automatically set in the environment
   of this Postman example and can be used for subsequent calls relating to this session.

   To display the session, you will need to load the Vipps Checkout SDK in your website, as described in
   [API Guide: Displaying the session](vipps-checkout-api.md#step-2-displaying-the-session).

## Retrieve the session information

1. Send request `Get session info`. This retrieves the session information by using
   [`GET:/v3/session/{reference}`](https://developer.vippsmobilepay.com/api/checkout#tag/Session/paths/~1v3~1session~1%7Breference%7D/get).
   The `reference` variable was previously set in the environment.

   You will see the details appear in the lower pane.
