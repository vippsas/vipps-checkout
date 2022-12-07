## <!-- START_METADATA

title: V3-migration
sidebar_position: 25

---

END_METADATA -->

# Migrating from Checkout V2 to V3

Checkout V3 improves upon V2 through alignment of polling and callback responses, improvement of field names and expanding models related to logistics options to permit more advanced features. This guide takes you through the things you need to change in your existing V2 integration to support Checkout V3.

## Initiate Session

### CheckoutConfiguration

The field "Configuration" has been added to InitiateSessionRequest and following fields have been moved into it:

- customerInteraction
- userFlow
- requireUserInfo
- elements

The elements field replaces the former ContactFields and AddressFields flags with an enum. They are equivalent according to the following table.

| Elements              | ContactFields | AddressFields |
| --------------------- | ------------- | ------------- |
| Full (default)        | true          | true          |
| PaymentAndContactInfo | true          | false         |
| PaymentOnly           | false         | false         |

We have also added a new field Countries which allows merchants to specify which countries they support.

### CallbackUrl

callbackUrlPrefix has been replaced by callbackUrl so that you send in the whole url where you want the "Payment Completed" callback.

### Strongly typed fields

The following are now strongly typed:

- customerInteraction
- userFlow

The permitted values for these fields used to be case insensitive. They are now case sensitive.

### Logistics

The logisticsOption object is now changed to better accomodate more advanced Checkout features. It is now a polymorphic object whose type is determined by which logistics carrier is used. Refer to Swagger for more details.

## Polling and callback

The response object from polling and the callback object are now identical.

### UserDetails

"userDetails" has been removed.
If requireUserInfo is set to true in session initiation, user info will instead be provided in its own dedicated response object UserInfo.

```javascript
"userInfo": {
    "sub": string,
    "email": string
}
```

### PaymentDetails

paymentDetails.State is now Authorized with a z instead of Authorised with a s.

### Renaming of region to city

billingDetails.region has been renamed to billingDetails.city
shippingDetails.region has been renamed to shippingDetails.city

### Standardized country name

billingDetails.country and shippingDetails.country used to be derived from the free-text field filled in by the customer. We have now changed this to always be two-letter codes that adheres to the ISO-3166-1 Alpha-2 standard (https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2).

## Cancel Session

The cancel session endpoint has been removed.
