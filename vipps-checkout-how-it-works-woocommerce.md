---
title: "How it works: WooCommerce"
sidebar_position: 12
---

# How Checkout works for WooCommerce

With [Vipps' official WooCommerce plugin](https://wordpress.org/plugins/woo-vipps/), it's easy to start accepting payments with Vipps, VISA, or MasterCard through Vipps Checkout.

<iframe width="100%" height="500" src="https://www.youtube-nocookie.com/embed/86RSKuQ5GME" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share; fullscreen"></iframe>

If you can't see the above frame, click the [WooCommerce Vipps Checkout enablement video](https://www.youtube.com/watch?v=86RSKuQ5GME).
If you have a valid set of Vipps API keys, which means if you have a *Vipps på nett* or *Vipps Checkout* agreement, you can start using Vipps Checkout.

If you don't have an agreement with Vipps, [click here to order](https://portal.vipps.no/register/vippscheckout).

**Don't know what API keys are?** [Here you can find](https://developer.vippsmobilepay.com/docs/vipps-developers/common-topics/api-keys/) more info on what they are, how they're used, and how to get them



# Vipps for WooCommerce plugin installation

To use Vipps Checkout in WooCommerce, you use our official plugin, which is kind of a program you install in you WooCommerce store.

The installation process is described [here](https://github.com/vippsas/vipps-woocommerce), but can be summarized as:

1. Install the plugin using the WordPress [built-in installer](https://codex.wordpress.org/Managing_Plugins#Installing_Plugins). The plugin can also be installed manually by uploading the plugin files to the `/wp-content/plugins/` directory.
2. Activate the plugin through the 'Plugins' screen on WordPress.
3. Go to the WooCommerce Settings page and choose Payment Gateways (Betalinger) and enable Vipps.
4. Go the settings page for the Vipps plugin and enter your Vipps account keys. Your account keys are available in the [Vipps Developer Portal](https://portal.vipps.no).

# Checkout specific setup

Checkout replaces the default WooCommerce checkout window with a Vipps-designed window, giving you better conversion and less hassle.

Features specific to Vipps Checkout are accessed in the *Payments* settings in the Admin panel.
![image](https://user-images.githubusercontent.com/25223283/226337801-7561a625-4ad5-4a68-aa56-96a6c1bcaf68.png)

To turn on Checkout, simply check the box at the top of the "Vipps Checkout" screen.
![image](https://user-images.githubusercontent.com/25223283/226338565-250873b7-ff9d-449a-8b7e-ce7392441a2c.png)

# Improve your shipping selection

Standard WooCommerce shipping selection is not optimal, so we have integrated with the major Norwegian shipping providers to remove friction from this important step in the sales process. This list currently includes:

- Bring/Posten
- Postnord
- Porterbuddy
- Helthjem
- Instabox

If you have agreements with one or more of these providers, you can add their API keys to Vipps Checkout. We can then present a tailored shipping selection window for your customers. For example, we can provide the improved shipping selection shown below.

## Improved shipping selection with Posten

![Posten shipping selection WooCommerce](https://user-images.githubusercontent.com/25223283/226342343-479fb87c-6f4c-4557-8b77-bafd6c36eac7.gif)

## Standard shipping selection

![Standard shipping selection WooCommerce](https://user-images.githubusercontent.com/25223283/226344800-09395fc7-b1d8-4db3-8815-1d3a71e0a9ab.gif)

## How to guide

There are a few steps required to enable the improved shipping selection. For details, we have a [technical how-to-guide](https://developer.vippsmobilepay.com/docs/APIs/checkout-api/vipps-checkout-how-it-works-shipping).

We have made a [simple video](https://www.youtube.com/watch?v=f4NVq4A73UA), showing how to enable PorterBuddy in Checkout, but the same principles apply for the other options.

<iframe width="100%" height="500" src="https://www.youtube-nocookie.com/embed/f4NVq4A73UA" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share; fullscreen"></iframe>
