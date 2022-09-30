<!-- START_METADATA
---
title: Quick start
sidebar_position: 5
---
END_METADATA -->

# Quick start

Use the Checkout API to create a checkout session, retrieve session information, and cancel the current checkout session.

<!-- START_TOC -->

## Table of Contents

* [Postman](#postman)
  * [Prerequisites](#prerequisites)
  * [Step 1: Get the Vipps Postman collection and environment](#step-1-get-the-vipps-postman-collection-and-environment)
  * [Step 2: Import the Vipps Postman files](#step-2-import-the-vipps-postman-files)
  * [Step 3: Set up Postman environment](#step-3-set-up-postman-environment)
* [Make API calls](#make-api-calls)
  * [Create a checkout session](#create-a-checkout-session)
  * [Retrieve the session information](#retrieve-the-session-information)
  * [Cancel the current checkout session](#cancel-the-current-checkout-session)

<!-- END_TOC -->

Document version 1.1.0.

## Postman

### Prerequisites

Review
[Vipps quick start guides](https://github.com/vippsas/vipps-developers/blob/master/vipps-quick-start-guides.md) for information about getting your test environment set up.

### Step 1: Get the Vipps Postman collection and environment

Save the following files to your computer:

* [Vipps Checkout API Postman collection](tools/vipps-checkout-api-postman-collection.json)
* [Vipps API Global Postman environment](https://github.com/vippsas/vipps-developers/blob/master/tools/vipps-api-global-postman-environment.json)

### Step 2: Import the Vipps Postman files

1. In Postman, click *Import* in the upper-left corner.
1. In the dialog that opens, with *File* selected, click *Upload Files*.
1. Select the two files you have just downloaded and click *Import*.

### Step 3: Set up Postman environment

1. Click the down arrow, next to the "eye" icon in the top-right corner, and select the environment you have imported.
1. Click the "eye" icon and, in the dropdown window, click `Edit` in the top-right corner.
1. Fill in the `Current Value` for the following fields to get started. For the first keys, go to *Vipps Portal > Utvikler -> Test Keys.*
   * `client_id` - Merchant key is required for getting the access token.
   * `client_secret` - Merchant key is required for getting the access token.
   * `Ocp-Apim-Subscription-Key` - Merchant subscription key.
   * `merchantSerialNumber` - Merchant id.
   * `internationalMobileNumber` - The mobile number for the test app profile you have received or registered, including country code.

You can update any of the other environment variables. Be aware of this:

* Any currency amount must be an Integer value minimum 100 in øre.
* Most URLs must be `https`.

## Make API calls

### Create a checkout session

1. Send request `Create a Checkout Session`. This starts a simple session by using
   [`POST:/v2/session`](https://vippsas.github.io/vipps-developer-docs/api/checkout#tag/Session/paths/~1v2~1session/post).

   The `reference` and `sessionId` variables are automatically set in the environment
   of this Postman example and can be used for subsequent calls relating to this session.

## Retrieve the session information

1. Send request `Get session info`. This retrieves the session information by using
   [`GET:/v2/session/{sessionId}`](https://vippsas.github.io/vipps-developer-docs/api/checkout#tag/Session/paths/~1v2~1session~1%7BsessionId%7D/get).
   The `sessionId` variable was previously set in the environment.

   You will see the details appear in the lower pane.

## Cancel the current checkout session

1. Send request `Cancel current session` to cancel the current Checkout session, if payment is not initiated.

   The `sessionId` variable, previously set in the environment, is sent in the body of
   [`GET:/v2/session/cancel`](https://vippsas.github.io/vipps-developer-docs/api/checkout#tag/Session/paths/~1v2~1session~1cancel/post).

## Questions?

We're always happy to help with code or other questions you might have!
Please create an [issue](https://github.com/vippsas/vipps-checkout-api/issues),
a [pull request](https://github.com/vippsas/vipps-checkout-api/pulls),
or [contact us](https://github.com/vippsas/vipps-developers/blob/master/contact.md).

Sign up for our [Technical newsletter for developers](https://github.com/vippsas/vipps-developers/tree/master/newsletters).
