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

For more common questions, see:

* [API General FAQ](https://developer.vippsmobilepay.com/docs/faqs)

<!-- START_COMMENT -->

💥 Please use the documentation pages here: <https://developer.vippsmobilepay.com/docs/APIs/checkout-api>. 💥

<!-- END_COMMENT -->

## Checkout features

### Is it possible to add a newsletter option in Checkout?

Yes, [custom consent](https://developer.vippsmobilepay.com/docs/APIs/checkout-api/vipps-checkout-api/#custom-consent) can be used.

### How can I pre-fill my customers phone number in the payment page (landing page)?

Yes, check out the [pre-fill customer data](https://developer.vippsmobilepay.com/docs/APIs/checkout-api/vipps-checkout-api/#prefill-customer-data) feature.

## Capture, reservations, and refunds

### How are subsequent transaction operations (Capture/Cancel/Refund) handled?

All subsequent transaction operations are fully supported in the
[ePayment API](https://developer.vippsmobilepay.com/docs/APIs/epayment-api).

See the [Common FAQs](https://developer.vippsmobilepay.com/docs/faqs) for common
questions about captures, reservations, and refunds.

## Testing

### I can't test card payments in Merchant Test (MT) environment, is there something wrong?

We currently don't support card payments in test environment, only payments using the MT app. If you need to do verification, we suggest doing a NOK 1 payment in production, and do a subsequent refund, which will make the funds available again within a week.

The only difference between a wallet payment and card payment is that *paymentMethod* is *Wallet* or *Card*.

## Card payments

See:
[Card payments](https://developer.vippsmobilepay.com/docs/faqs/users-and-payments-faq/#card-payments)

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
    "instance": "urn:uuid:d1bb89d3-50ab-4e90-94c0-54a67da0a7ec"
}
```
