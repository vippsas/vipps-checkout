<!-- START_METADATA
---
title: "How it works: WooCommerce"
sidebar_position: 12
---
END_METADATA -->

# How Checkout works for WooCommerce

With our official WooCommerce plugin, it's easy to start accepting payments with Vipps, VISA, or MasterCard through *Checkout*.

For technical information about the plugin, see
[Vipps for WooCommerce](https://developer.vippsmobilepay.com/docs/plugins-ext/woocommerce/) or 
from the WordPress site: [Pay with Vipps for WooCommerce](https://wordpress.org/plugins/woo-vipps/).



<!-- START_HIDDEN_IN_GITHUB
<iframe width="100%" height="500" src="https://www.youtube-nocookie.com/embed/86RSKuQ5GME" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share; fullscreen"></iframe>
END_HIDDEN_IN_GITHUB -->

<!-- START_COMMENT -->
See the [WooCommerce *Checkout* enablement video](https://www.youtube.com/watch?v=86RSKuQ5GME).
<!-- END_COMMENT -->

If you have a valid set of [API keys](https://developer.vippsmobilepay.com/docs/knowledge-base/api-keys/), which means if you have a *Vipps på nett* or *Checkout* agreement, you can start using *Checkout*.

If you don't have an agreement with Vipps MobilePay, register on the [merchant portal](https://portal.vipps.no/register/vippscheckout).

**Don't know what API keys are?** [Here you can find](https://developer.vippsmobilepay.com/docs/knowledge-base/api-keys/) more info on what they are, how they're used, and how to get them.

# Vipps for WooCommerce plugin installation

To use *Checkout* in WooCommerce, you use our official plugin, which is kind of a program you install in you WooCommerce store.

The [detailed installation process](https://developer.vippsmobilepay.com/docs/plugins-ext/woocommerce/#installation) can be summarized as:

@@ -46,23 +48,23 @@ The [detailed installation process](https://developer.vippsmobilepay.com/docs/pl

Checkout replaces the default WooCommerce checkout window with a Vipps window, giving you better conversion and less hassle.

Features specific to *Checkout* are accessed in the *Payments* settings in the Admin panel.
![image](https://user-images.githubusercontent.com/25223283/226337801-7561a625-4ad5-4a68-aa56-96a6c1bcaf68.png)

To turn on *Checkout*, check the box at the top of the "Checkout" screen.
![image](https://user-images.githubusercontent.com/25223283/226338565-250873b7-ff9d-449a-8b7e-ce7392441a2c.png)

# Improve your shipping selection

Standard WooCommerce shipping selection is not optimal, so we have integrated with the major Norwegian shipping providers to remove friction from this important step in the sales process. This list currently includes:

* Bring/Posten
* Postnord
* Porterbuddy
* Helthjem
* Instabox

If you have agreements with one or more of these providers, you can add their API keys to *Checkout*. We can then present a tailored shipping selection window for your customers. For example, we can provide the improved shipping selection shown below.

## Improved shipping selection with Posten

@@ -74,7 +76,7 @@ If you have agreements with one or more of these providers, you can add their AP

## How to guide

There are a few steps required to enable the improved shipping selection. For details, we have a [technical how-to-guide](https://developer.vippsmobilepay.com/docs/APIs/checkout-api/checkout-how-it-works-shipping).

We have made a [simple video](https://www.youtube.com/watch?v=f4NVq4A73UA), showing how to enable PorterBuddy in Checkout, but the same principles apply for the other options.