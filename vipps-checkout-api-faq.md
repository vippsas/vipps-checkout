# FAQ

### How are subsequent transaction operations (Capture/Cancel/refund) handled

All subsequent transaction operations are fully supported in the Vipps epayment API. As described [here](https://github.com/vippsas/vipps-epayment-api)

Please feel free to make a PR with a question if something is unclear.

## Testing

### I can't test card payments in Merchant Test (MT) environment, is there something wrong?

We currently don't support card payments in test environment, only payments using Vipps MT app. If you need to do verification, we suggest doing a NOK 1 payment in production, and do a subsequent refund, which will make the funds available again within a week.

## When paying by card

### What card types are supported?

Visa and Mastercard. This includes any cards that are co-branded with VISA and Mastercard.

### Which issuer countries are supported?

Cards issued in the following countries are accepted: EEA/EØS (European Economic Area), UK, Canada, USA, Australia and New Zealand.

### Is Visa Electron supported?

Yes, if the Visa Electron card is enabled for online purchases.
