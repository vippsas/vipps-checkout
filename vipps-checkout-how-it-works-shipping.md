<!-- START_METADATA
---
title: How it works: shipping
sidebar_position: 11
---
END_METADATA -->

# How it works: shipping
Vipps Checkout supports all major shipping providers in the norwegian market, including:
- Bring/Posten
- Postnord
- Porterbuddy
- Helthjem
- Instabox

A provider is chosen by setting the `brand` property to one of the allowed string values. Consult the [API spec](https://vippsas.github.io/vipps-developer-docs/api/checkout#tag/Session/paths/~1session/post) for further details. This will set the logo and name of the provider.

![Shipping provider logo example](resources/shipping_logo-example.png)

If none of the providers fit your use-case, e.g. indicating in-store-pickup, setting `brand: "OTHER"` will set a generic icon.

![Shipping provicer default logo](resources/shipping_logo-default.png)

## Enriching features
For some of the providers we offer enriching features, including:
- Pickup point
- Home delivery

These features change the user interface to allow for more specific selection of delivery time or place, where some of the features require you (the merchant) to provide credentials. Credentials are provided in the `logistics.integrations` property at session initiation, and are used to perform a "pre booking" (i.e. reserve a timeslot) for some of the shipping providers.

An enriching feature can be chosen by setting the `type` property on the logistics option, again referring to the [API spec](https://vippsas.github.io/vipps-developer-docs/api/checkout#tag/Session/paths/~1session/post) for which features are available for each shipping provider.

### Pickup point

The pickup point feature is enabled by setting `type: "PICKUP_POINT"`. The title will become `{providerName} pick-up point` (e.g. "Posten pick-up point). The consumer gets to choose an available pickup point based on the address. Vipps relays the selected option as part of the content in the "session completed callback".

![Pickup point animation](resources/shipping_pickup-point.gif)

### Home delivery

The home delivery feature is enabled by setting `type: "HOME_DELIVERY"`. The title will become `{providerName} home delivery` (e.g. "Porterbuddy home delivery") by default. The consumer gets to choose an available delivery window based on the address. Vipps relays the selected option as part of the content in the "session completed callback".

![Home delivery animation](resources/shipping_home-delivery.gif)
