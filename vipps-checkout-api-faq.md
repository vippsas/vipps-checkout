<!-- START_METADATA
---
title: Checkout API Frequently Asked Questions
sidebar_label: FAQ
sidebar_position: 24
description: Frequently asked questions for the Checkout API.
pagination_next: null
pagination_prev: null
---
END_METADATA -->

# Frequently asked questions

Here are the Checkout API FAQs.
See the
[Checkout API guide](vipps-checkout-api.md)
for more details.

For general information and questions, please check in the
[Knowledge base](https://developer.vippsmobilepay.com/docs/common-topics/).

<!-- START_COMMENT -->
ℹ️ Please use the website:
[Vipps MobilePay Technical Documentation](https://developer.vippsmobilepay.com/docs/APIs/checkout-api>).
<!-- END_COMMENT -->

## General Information

### What is Checkout?
Checkout is an online payment solution that allows businesses to offer their customers a fast, secure, and convenient way to pay for goods and services online.

### How do I sign up for Checkout?
To sign up for Checkout, you need to create an account on the Vipps website and follow the instructions to set up your account. You will need to provide information about your business and bank account details.

### What kind of support is available for Checkout users?
We provide support through our website, email, and through our extensive partner network. There is also a knowledge base available on the website that contains answers to frequently asked questions.

## Payment Methods

### What payment methods are available with Checkout?
Vipps Checkout supports a range of payment methods, including Vipps, credit and debit cards. Local payment methods will also become available in select markets during 2024, e.g. bank payments in Finland.

### Can I use Checkout for recurring payments?
Checkout does not currently support recurring payments, but we plan on launching Vipps Recurring Payments in 2023.

## Security and Fees

### Is it safe to use Checkout for online payments?
Yes, Vipps Checkout is a secure payment solution that uses industry-standard encryption to protect your and your users' personal and financial information.

### What fees are associated with using Checkout?
Vipps Checkout charges a percentage fee for the payment volume processed. Our standard prices can be found [here](https://vipps.no/alle-priser/bedrift/).

## Integration and Functionality

### Can I integrate Checkout with my existing website or online store?
Yes, Vipps Checkout can be integrated with most major website and e-commerce platforms, including Shopify, WooCommerce, and Magento. Check out our [list of plugins](https://developer.vippsmobilepay.com/docs/plugins/) for more information.

### How long does it take for payments to be paid out with Checkout?
You will typically get your money on your account within three (3) banking days. So money paid to you on a Monday will be on your account on Wednesday, and payment on Friday will be on your account on Tuesday. See [Settlements](https://developer.vippsmobilepay.com/docs/settlements/) for more details.

### How does Checkout compare to other online payment solutions?
Checkout offers Vipps as it is supposed to work with card payments, user information retrieval, and shipping selection all in one well-designed bundle, and compares favorably to other online payment solutions.

## Technical information

### Is it possible to add a newsletter option in Checkout?

Yes, [custom consent](https://developer.vippsmobilepay.com/docs/APIs/checkout-api/vipps-checkout-api/#custom-consent) can be used.

### How can I pre-fill my customers phone number in the payment page (landing page)?

Yes, check out the [pre-fill customer data](https://developer.vippsmobilepay.com/docs/APIs/checkout-api/vipps-checkout-api/#prefill-customer-data) feature.

## Capture, reservations, and refunds

### How are subsequent transaction operations (Capture/Cancel/Refund) handled?

All subsequent transaction operations are fully supported in the
[ePayment API](https://developer.vippsmobilepay.com/docs/APIs/epayment-api).

See the [Knowledge base](https://developer.vippsmobilepay.com/docs/common-topics/) for common
questions about captures, reservations, and refunds.

## Testing

### I can't test card payments in Merchant Test (MT) environment, is there something wrong?

We currently don't support card payments in test environment, only payments using the MT app. If you need to do verification, we suggest doing a NOK 1 payment in production, and do a subsequent refund, which will make the funds available again within a week.

The only difference between a wallet payment and card payment is that *paymentMethod* is *Wallet* or *Card*.

## Card payments

See:
[Card payments](https://developer.vippsmobilepay.com/docs/common-topics/payments#card-payments)

## Shipping

### Can I have "Pick-up in store" as shipping option with Checkout?

Yes.

*Checkout* will basically display whatever shipping methods defined by the webshop.
For example, if you want *Checkout*  to display "Pick up at the Royal Norwegian Castle", we will display that upon receiving that message through the API.

For WooCommerce-based web shops, this is done in the shipping configuration part of WooCommerce admin:
`https://[_your webshop URL_]/wp-admin/admin.php?page=wc-settings&tab=shipping`

## Errors

**Note: our error message format may evolve, so avoid building strict logic around it**

Any errors that occur will return a non-successful response code with a body based on <https://tools.ietf.org/html/rfc7807>.

Common errors include:

* Invalid credentials (status code 401)
* Missing mandatory fields in a request (status code 400)
* Session expired or not known (status code 404).

An example of error returned when querying an expired session:

```json
{
    "errorCode": "Session-00100",
    "title": "Session expired or not known.",
    "status": 404,
    "instance": "urn:uuid:d1bb89d3-50ab-4e90-94c0-54a67da0a7ec",
}
```

Example of the result of a failed validation:
```json
{
    "errorCode": "Validation-00001",
    "title": "One or more validation errors occurred.",
    "status": 400,
    "instance": "urn:uuid:d1bb89d3-50ab-4e90-94c0-54a67da0a7ec",
    "errors": {
        "Transaction.Reference": [
            "The Reference field is required."
        ]
    }
}
```
