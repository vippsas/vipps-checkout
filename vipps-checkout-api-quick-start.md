<!-- START_METADATA
---
title: Quick start
sidebar_position: 5
---
END_METADATA -->

# Quick start

Use the Checkout API to create a checkout session, retrieve session information from the database, and cancel the current checkout session.

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

Document version 1.0.4.

## Postman

### Prerequisites

Review
[Vipps quick start guides](https://vippsas.github.io/vipps-developer-docs/docs/vipps-developers/vipps-quick-start-guides) for information about getting your test environment set up.

### Step 1: Get the Vipps Postman collection and environment

Save the following files to your computer:

* [Vipps Checkout API Postman collection](tools/vipps-checkout-api-postman-collection.json)
* [Vipps Checkout API Postman environment](tools/vipps-checkout-api-postman-environment.json)

### Step 2: Import the Vipps Postman files

1. In Postman, click *Import* in the upper-left corner.
1. In the dialog that opens, with *File* selected, click *Upload Files*.
1. Select the two files you have just downloaded and click *Import*.

### Step 3: Set up Postman environment

1. Click the down arrow, next to the "eye" icon in the top-right corner, and select the environment you have imported.
1. Click the "eye" icon and, in the dropdown window, click `Edit` in the top-right corner.
1. Fill in the `Current Value` for the following fields to get started. For the first three keys, go to *Vipps Portal > Utvikler -> Test Keys.*
   * `client_id`
   * `client_secret`
   * `Ocp-Apim-Subscription-Key`
   * `merchantSerialNumber`

## Make API calls

See the
[API reference](https://vippsas.github.io/vipps-developer-docs/api/checkout)
for details about the calls.

### Create a checkout session

1. Go to request `Creates a Checkout Session`. Fill in the request body.
1. Send the filled in request `Creates a Checkout Session`.
1. Check that the response is OK.
1. In the response body - find the `pollingUrl`. Copy the part after the last / - this is the sessionId.

## Retrieve the session information

1. Go to request `Retrieves sessionInfo from database when merchant polls`.
1. In the url, replace the `:sessionId` with the sessionId.
1. Check that you get an OK response.

## Cancel the current checkout session

1. Go to request `Cancels the current Checkout session if payment is not initiated`.
1. In the url, replace the `:sessionId` with the sessionId.
1. Check that you get an OK response.

## Questions?

We're always happy to help with code or other questions you might have!
Please create an [issue](https://github.com/vippsas/vipps-checkout-api/issues),
a [pull request](https://github.com/vippsas/vipps-checkout-api/pulls),
or [contact us](https://github.com/vippsas/vipps-developers/blob/master/contact.md).

Sign up for our [Technical newsletter for developers](https://github.com/vippsas/vipps-developers/tree/master/newsletters).
