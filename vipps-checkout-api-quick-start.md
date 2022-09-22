<!-- START_METADATA

title: Postman
sidebar_position: 60

---

END_METADATA -->

# Postman

[Postman](https://www.getpostman.com/) is a common tool for working with REST APIs.
We offer a [Postman Collection](https://www.getpostman.com/collection) to make development easier.
See the [Postman documentation](https://www.getpostman.com/docs/) for more information about using Postman.

By following the steps below, you can make calls to all the
endpoints, and see the full `request` and `response` for each call.

We also have a short [getting started guide to Postman](https://github.com/vippsas/vipps-developers/blob/master/vipps-quick-start-guides.md).

## Setting up Postman

### Step 1: Get the Postman Collection

Import the collection by following the steps below:

1. Click `Import` in the upper-left corner.
2. Import the [vipps-checkout-api-postman-collection.json](tools/vipps-checkout-api-postman-collection.json) file.

### Step 2: Import the Postman Environment

1. Click `Import` in the upper-left corner.
2. Import the [vipps-checkout-api-postman-environment.json](tools/vipps-checkout-api-postman-environment.json) file.

### Step 3: Setup Postman Environment

1. Click the down arrow, next to the "eye" icon in the top-right corner, and select the environment you have imported.
1. Click the "eye" icon and, in the dropdown window, click `Edit` in the top-right corner.
1. Fill in the `Current Value` for the following fields to get started. For the first three keys, go to _Vipps Portal > Utvikler -> Test Keys._
   - `client_id`
   - `client_secret`
   - `Ocp-Apim-Subscription-Key`
   - `merchantSerialNumber`

### Step 4: Run tests

1. Go to request `Creates a Checkout Session`. Fill in the request body.
1. Send the filled in request `Creates a Checkout Session`.
1. Check that the response is OK.
1. In the response body - find the `pollingUrl`. Copy the part after the last / - this is the sessionId.
1. Go to request `Retrieves sessionInfo from database when merchant polls`.
1. In the url, replace the `:sessionId` with the sessionId.
1. Check that you get an OK response.
1. Go to request `Cancels the current Checkout session if payment is not initiated`.
1. In the url, replace the `:sessionId` with the sessionId.
1. Check that you get an OK response.

## Questions?

We're always happy to help with code or other questions you might have!
Please create an [issue](https://github.com/vippsas/vipps-checkout-api/issues),
a [pull request](https://github.com/vippsas/vipps-checkout-api/pulls),
or [contact us](https://github.com/vippsas/vipps-developers/blob/master/contact.md).

Sign up for our [Technical newsletter for developers](https://github.com/vippsas/vipps-developers/tree/master/newsletters).
