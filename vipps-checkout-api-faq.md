<!-- START_METADATA
---
title: FAQ
sidebar_position: 24
---
END_METADATA -->


# Vipps Checkout API: Frequently Asked Questions

<!-- START_COMMENT -->

ℹ️ Please use the new documentation:
[Vipps Technical Documentation](https://vippsas.github.io/vipps-developer-docs/docs/APIs/checkout-api).

<!-- END_COMMENT -->

Here are the Checkout API FAQs.
See the
[Vipps Checkout API guide](vipps-checkout-api.md)
for more details.

For more common Vipps questions, see:

* [Vipps API General FAQ](https://vippsas.github.io/vipps-developer-docs/docs/vipps-developers/faqs/)

Document version 1.0.0.

<!-- START_TOC -->

## Table of contents

* [Capture, Reservations, and Refunds](#capture-reservations-and-refunds)
  * [How are subsequent transaction operations (Capture/Cancel/Refund) handled](#how-are-subsequent-transaction-operations-capturecancelrefund-handled)
* [Testing](#testing)
  * [I can't test card payments in Merchant Test (MT) environment, is there something wrong?](#i-cant-test-card-payments-in-merchant-test-mt-environment-is-there-something-wrong)
* [Card payments](#card-payments)
  * [What card types are supported?](#what-card-types-are-supported)
  * [Which issuer countries are supported?](#which-issuer-countries-are-supported)
  * [Is Visa Electron supported?](#is-visa-electron-supported)
* [Shipping](#shipping)
  * [Can I have "Pick-up in store" as shipping option with Vipps Checkout?](#can-i-have-pick-up-in-store-as-shipping-option-with-vipps-checkout)
* [Questions?](#questions)

<!-- END_TOC -->

## Capture, Reservations, and Refunds

### How are subsequent transaction operations (Capture/Cancel/Refund) handled?

All subsequent transaction operations are fully supported in the
[Vipps epayment API](https://vippsas.github.io/vipps-developer-docs/docs/APIs/epayment-api/).

See the [Vipps FAQs](https://vippsas.github.io/vipps-developer-docs/docs/vipps-developers/faqs/) for common
questions about captures, reservations, and refunds.

## Testing

### I can't test card payments in Merchant Test (MT) environment, is there something wrong?

We currently don't support card payments in test environment, only payments using Vipps MT app. If you need to do verification, we suggest doing a NOK 1 payment in production, and do a subsequent refund, which will make the funds available again within a week.

The only difference between a Vipps wallet payment and card payment is that *paymentMethod* is *Wallet* or *Card*.

## Card payments

### What card types are supported?

Visa and Mastercard. This includes any cards that are co-branded with VISA and Mastercard.

### Which issuer countries are supported?

Cards issued in the following countries are accepted: EEA/EØS (European Economic Area), UK, Canada, USA, Australia and New Zealand.

### Is Visa Electron supported?

Yes, if the Visa Electron card is enabled for online purchases.

## Shipping

### Can I have "Pick-up in store" as shipping option with Vipps Checkout?

Yes.

Vipps Checkout will basically display whatever shipping methods defined by the webshop.
For example, if you want Vipps Checkout to display "Pick up at the Royal Norwegian Castle", we will display that upon receiving that message through the API.

For WooCommerce-based webshops, this is done in the shipping configuration part of WooCommerce admin:
`https://[_your webshop URL_]/wp-admin/admin.php?page=wc-settings&tab=shipping`

## Questions?

We're always happy to help with code or other questions you might have!
Please create an [issue](https://github.com/vippsas/vipps-checkout-api/issues),
a [pull request](https://github.com/vippsas/vipps-checkout-api/pulls),
or [contact us](https://vippsas.github.io/vipps-developer-docs/docs/vipps-developers/contact).

Sign up for our [Technical newsletter for developers](https://vippsas.github.io/vipps-developer-docs/docs/vipps-developers/newsletters).
