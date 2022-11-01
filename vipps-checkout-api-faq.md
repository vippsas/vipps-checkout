<!-- START_METADATA
---
title: FAQ
sidebar_position: 24
---
END_METADATA -->

# FAQ

## How are subsequent transaction operations (Capture/Cancel/refund) handled

All subsequent transaction operations are fully supported in the Vipps epayment API. As described [here](https://github.com/vippsas/vipps-epayment-api)

Please feel free to make a PR with a question if something is unclear.

## Testing

### I can't test card payments in Merchant Test (MT) environment, is there something wrong?

We currently don't support card payments in test environment, only payments using Vipps MT app. If you need to do verification, we suggest doing a NOK 1 payment in production, and do a subsequent refund, which will make the funds available again within a week.

The only difference between a Vipps wallet payment and card payment is that *paymentMethod* is *Wallet* or *Card*.

## When paying by card

### What card types are supported?

Visa and Mastercard. This includes any cards that are co-branded with VISA and Mastercard.

### Which issuer countries are supported?

Cards issued in the following countries are accepted: EEA/EØS (European Economic Area), UK, Canada, USA, Australia and New Zealand.

### Is Visa Electron supported?

Yes, if the Visa Electron card is enabled for online purchases.

## Shipping

### Can I have "Pick-up in store" as shipping option with Vipps Checkout?

Yes. 

Vipps Checkout will basically display whatever shipping methods defined byt the webshop, so if you want Vipps Checkout to display "Pick up at the Royal Norwegian Castle" we will do it if we get that message over our API.

For WooCommerce based webshops, this is done in the shipping configuration part of WooCommerce admin: https://[_your webshop URL_]/wp-admin/admin.php?page=wc-settings&tab=shipping
