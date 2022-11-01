<!-- START_METADATA
---
title: Checklist
sidebar_position: 23
---
END_METADATA -->

# Checkout integration checklist

The Checkout API should be considered an aggregation API of Vipps services, while transaction operations is to be performed on the [epayment][epayment-api-reference-url] API.

## Checklist

- [ ] Integrate _all_ the [API endpoints](vipps-checkout-api.md):
  - [ ] Initiate [`POST:checkout/v2/session`][create-checkout-session-endpoint]
  - [ ] Details [`GET:checkout/v2/session/{sessionId}`][retrieve-sessioninfo-endpoint]
  - [ ] Full and partial Capture [`POST:epayment/v1/{reference}/capture`][capture-payment-endpoint]
  - [ ] Cancel [`POST:epayment/v1/{reference}cancel`][cancel-payment-endpoint]
  - [ ] Full and partial Refund [`POST:epayment/v1/{reference}refund`][refund-payment-endpoint]
  - [ ] Details [`GET:epayment/v1/{reference}`][get-payment-endpoint]
  - For examples of requests and responses, see the relevant API docs for [checkout][checkout-api-reference-url] and [epayment][epayment-api-reference-url].
- [ ] Send the [Vipps HTTP headers](vipps-checkout-api.md#integration-partner-and-plugin-guidelines)
      in all API requests for better tracking and troubleshooting (mandatory for partners and platforms):
  - [ ] `Merchant-Serial-Number`
  - [ ] `Vipps-System-Name`
  - [ ] `Vipps-System-Version`
  - [ ] `Vipps-System-Plugin-Name`
  - [ ] `Vipps-System-Plugin-Version`
- [ ] Follow the [orderId recommendations](https://github.com/vippsas/vipps-ecom-api/blob/master/vipps-ecom-api.md#orderid-recommendations).
- [ ] We recommend using Vipps Order management, as this is a massive benefit for the end user experience. It is mandatory for merchants using ["Vipps Facilitated Content Monitoring"](https://github.com/vippsas/vipps-order-management-api/blob/main/vipps-order-management-api.md).
- [ ] Correctly handle callbacks from Vipps, both for successful and unsuccessful payments.
      See the API documentation for
      [how callback URLs are built](vipps-checkout-api.md#callback-handling),
      make test calls to make sure you handle the `POST` requests correctly.
      Vipps does not have capacity to manually do this for you.
  - [ ] Callback [`POST:[callbackPrefix]/checkout/{version}/order/{orderId}`](vipps-checkout-api.md#example-of-callback)
- [ ] Avoid Integration pitfalls
  - [ ] The Merchant _must not_ rely on `fallback` or `callback` alone, and must poll Payments Details [`GET:/epayment/v1/{reference}`][get-payment-endpoint] or Session Details [`GET:/checkout/v2/{sessionId}`][retrieve-sessioninfo-endpoint]
        as documented (this is part of the first item in this checklist, but it's still a common error). For pure payment status polling the ePayment API is recommended.
  - [ ] The merchant must handle that the `fallback` URL is opened in the default browser on the phone,
        and not in a specific browser, in a specific tab, in an embedded browser, requiring a session token, etc.
        See the API guide:
        [Recommendations regarding handling redirects](https://github.com/vippsas/vipps-ecom-api/blob/master/vipps-ecom-api.md#recommendations-regarding-handling-redirects).
        See the FAQ: [How can I open the fallback URL in a specific (embedded) browser?](https://github.com/vippsas/vipps-developers/blob/master/faqs/common-problems-faq.md#how-can-i-open-the-fallback-url-in-a-specific-embedded-browser)
  - [ ] The Vipps branding must be according to the
        [Vipps design guidelines](https://github.com/vippsas/vipps-design-guidelines).
  - [ ] Make sure your customer service, etc has all the tools and information they need
        available in _your_ system, through the APIs listed in the first item in this checklist,
        and that they do not need to visit
        [portal.vipps.no](https://portal.vipps.no)
        for normal work.

[checkout-api-reference-url]: https://vippsas.github.io/vipps-developer-docs/api/checkout
[create-checkout-session-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/checkout/#tag/Session/paths/~1v2~1session/post
[retrieve-sessioninfo-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/checkout/#tag/Session/paths/~1v2~1session~1%7BsessionId%7D/get
[cancel-session-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/checkout/#tag/Session/paths/~1v2~1session~1cancel/post
[epayment-api-reference-url]: https://vippsas.github.io/vipps-epayment-api/
[create-payment-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/CreatePayments/operation/createPayment
[get-payment-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/QueryPayments/operation/getPayment
[get-payment-event-log-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/QueryPayments/operation/getPaymentEventLog
[cancel-payment-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/AdjustPayments/operation/cancelPayment
[capture-payment-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/AdjustPayments/operation/capturePayment
[refund-payment-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/AdjustPayments/operation/refundPayment
[adjust-authorization-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/AdjustPayments/operation/adjustAuthorization
[force-approve-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/ForceApprove/operation/forceApprove
