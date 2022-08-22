# Vipps On-Site Messaging

Vipps On-Site Messaging contains a _badges_ in different variants that can be used to let your customers know that Vipps payment is accepted.

To be able to use the badge on your site you need to add the Vipps On-Site Messaging JavaScript library.
The library should preferable be added between your page's `<head>...</head>`-tags and only once per page:

```html
<script async type="text/javascript" src="https://checkout.vipps.no/on-site-messaging/v1/vipps-osm.js"></script>
```

If you don't have access to edit your websites code directly you can also place the JavaScript library just before the chosen badge.

## Badge

The On-Site Messaging library contains an easy to integrate _badge_ with tailor made message for use in your online store.
The badge comes in five variants with different color-pallets to suite your website.

#### Example

You can find a demo and examples of all the variants [here](https://checkout.vipps.no/on-site-messaging/v1).

<img src="./resources/osm-badge.png" alt="Vipps Badge" width="480"/>

```html
<vipps-badge variant="gray"></vipps-badge>
```

#### Attributes

All attributes are optional.

| Attribute    | Description                                                                                                                                             | Default |
|:-------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------|:--------|
| variant      | The color variant of the badge.<br/>Supported values: `white`, orange`, `gray`, `light-orange`, `purple`.                                               | `white` |
| language     | ISO 639-1 alpha-2 language code.<br />Supported values: `en`, `no`.                                                                                     | `no`    |
| vipps-senere | Set this to `true` if your business supports the "Vipps Senere"-product.                                                                                | `false` |
| amount       | The payable amount in NOK. Amounts are specified in minor units.<br/>For Norwegian kroner (NOK) that means 1 kr = 100 øre. Example: 499 kr = 49900 øre. |         |

#### Customization

You can customize the placement and size of the _badge_ by either applying your own CSS-style with the `class` or `style` attribute.

If the badge is too small or too large to fit your content, you can override the `font-size` to scale the _badge_ as follows:

```html
<vipps-badge vipps-senere variant="purple" style="font-size: 1.5rem;"></vipps-badge>
```

Which will scale the badge to 1.5x the size of the root font-size. You may also use `px` or `em` values to scale the _badge_.

Use `text-align` to align the badge:

```html
<vipps-badge vipps-senere amount="100000" variant="purple" style="text-align: center;"></vipps-badge>
```

## Integrations

Here is how to integrate the badge to your products in some of the most used e-commerce platforms.
If you are in need of a more advanced set-up or placement, please consult your e-commerce platform documentation.

### WooCommerce

To add the _badge_ to your product in WooCommerce, click to edit the product where you want to display the _badge_.

Under either "Product description" or "Product short description", click the "Text" tab (next to "Visual") - usually in the top right corner of the text area.

Next, just add the JavaScript library and the code snippet for the _badge_:

```html
<script async type="text/javascript" src="https://checkout.vipps.no/on-site-messaging/v1/vipps-osm.js"></script>
<vipps-badge variant="purple"></vipps-badge>
```

<img src="./resources/osm-woocommerce.png" alt="WooCommerce integration" width="600"/>

### Magento

To add the _badge_ to your product in Magento, find your product in Magento Admin > Catalog > Products and open it for edit. Go to Content, Short Description, hide the editor and copy paste in the following:

```html
<script async type="text/javascript" src="https://checkout.vipps.no/on-site-messaging/v1/vipps-osm.js"></script>
<vipps-badge variant="purple"></vipps-badge>
```

<img src="./resources/osm-magento.png" alt="Magento integration" width="600"/>

### Shopify

To add the _badge_ to your product in Shopify, find your product in Shopify Admin > Producs and open it. Press the "Show Html"-button and copy paste in the following:

```html
<script async type="text/javascript" src="https://checkout.vipps.no/on-site-messaging/v1/vipps-osm.js"></script>
<vipps-badge variant="purple"></vipps-badge>
```

<img src="./resources/osm-shopify.png" alt="Shopify integration" width="600"/>
