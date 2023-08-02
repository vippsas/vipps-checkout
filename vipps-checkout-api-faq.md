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

## Capture, reservations, and refunds

### How are subsequent transaction operations (Capture/Cancel/Refund) handled?

All subsequent transaction operations are fully supported in the
[ePayment API](https://developer.vippsmobilepay.com/docs/APIs/epayment-api).

See the [Common FAQs](https://developer.vippsmobilepay.com/docs/faqs) for common
questions about captures, reservations, and refunds.

## Testing

### I can't test card payments in Merchant Test (MT) environment, is there something wrong?

We currently don't support card payments in test environment, only payments using Vipps MT app. If you need to do verification, we suggest doing a NOK 1 payment in production, and do a subsequent refund, which will make the funds available again within a week.

The only difference between a Vipps wallet payment and card payment is that *paymentMethod* is *Wallet* or *Card*.

## Card payments

See:
[Card payments](https://developer.vippsmobilepay.com/docs/faqs/users-and-payments-faq/#card-payments)

## Shipping

### Can I have "Pick-up in store" as shipping option with Vipps Checkout?

Yes.

Vipps Checkout will basically display whatever shipping methods defined by the webshop.
For example, if you want Vipps Checkout to display "Pick up at the Royal Norwegian Castle", we will display that upon receiving that message through the API.

For WooCommerce-based web shops, this is done in the shipping configuration part of WooCommerce admin:
`https://[_your webshop URL_]/wp-admin/admin.php?page=wc-settings&tab=shipping`
