## <!-- START_METADATA

title: "FAQs"
sidebar_position: 24

---

END_METADATA -->

# FAQs

Here are the Checkout API FAQs.
See the
[Checkout API guide](vipps-checkout-api.md)
for more details.

For more common Vipps questions, see:

- [Vipps API General FAQ](https://vippsas.github.io/vipps-developer-docs/docs/vipps-developers/faqs)

<!-- START_COMMENT -->

💥 Please use the documentation pages here: <https://vippsas.github.io/vipps-developer-docs/docs/APIs/checkout-api>. 💥

<!-- END_COMMENT -->

## Capture, reservations, and refunds

### How are subsequent transaction operations (Capture/Cancel/Refund) handled?

All subsequent transaction operations are fully supported in the
[Vipps epayment API](https://vippsas.github.io/vipps-developer-docs/docs/APIs/epayment-api).

See the [Vipps FAQs](https://vippsas.github.io/vipps-developer-docs/docs/vipps-developers/faqs) for common
questions about captures, reservations, and refunds.

## Testing

### I can't test card payments in Merchant Test (MT) environment, is there something wrong?

We currently don't support card payments in test environment, only payments using Vipps MT app. If you need to do verification, we suggest doing a NOK 1 payment in production, and do a subsequent refund, which will make the funds available again within a week.

The only difference between a Vipps wallet payment and card payment is that _paymentMethod_ is _Wallet_ or _Card_.

## Card payments

### What card types are supported?

Visa and Mastercard. This includes any cards that are co-branded with VISA and Mastercard.

### Which issuer countries are supported?

Cards issued in the following countries are accepted: EEA/EØS (European Economic Area), UK, Canada, USA, Australia and New Zealand.

### Is Visa Electron supported?

Yes, if the Visa Electron card is enabled for online purchases.

## Shipping

### Can I have "Pick-up in store" as shipping option with Checkout?

Yes.

Checkout will basically display whatever shipping methods defined by the webshop.
For example, if you want Checkout to display "Pick up at the Royal Norwegian Castle", we will display that upon receiving that message through the API.

For WooCommerce-based webshops, this is done in the shipping configuration part of WooCommerce admin:
`https://[_your webshop URL_]/wp-admin/admin.php?page=wc-settings&tab=shipping`
