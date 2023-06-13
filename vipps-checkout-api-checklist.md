<!-- START_METADATA
---
title: Checkout API checklist
sidebar_label: Checklist
sidebar_position: 23
description: Checklist for full integration with the Checkout API.
pagination_next: null
pagination_prev: null
---
END_METADATA -->

# Checklist

The Checkout API should be considered an aggregation API of Vipps MobilePay services, while transaction operations is to be performed on the [ePayment][epayment-api-reference-url] API. For examples of requests and responses, see the relevant API docs for [Checkout][checkout-api-reference-url] and [ePayment][epayment-api-reference-url].

## Checklist for full integration

Integrate _all_ the [Checkout API endpoints](vipps-checkout-api.md)

| Endpoint     | Comment |
|--------------|---------|
|     Initiate | [`POST:checkout/v3/session`][create-checkout-session-endpoint] |
|     Details  | [`GET:checkout/v3/session/{reference}`][retrieve-sessioninfo-endpoint] |
|     Callback |[`POST:[callbackPrefix]/checkout/{version}/order/{orderId}`](vipps-checkout-api.md#example-of-callback) |

Integrate _applicable_ [ePayment API endpoints](vipps-checkout-api.md)

| Endpoint | Comment |
|----------|---------|
|     Get payment details | [`GET:epayment/v1/{reference}`][get-payment-endpoint] |
|     Cancel payment | [`POST:epayment/v1/{reference}cancel`][cancel-payment-endpoint] |
|     Full and partial capture payment | [`POST:epayment/v1/{reference}/capture`][capture-payment-endpoint] |
|     Full and partial refund payment | [`POST:epayment/v1/{reference}refund`][refund-payment-endpoint] |

## Quality assurance

| Action | Comment |
|--------|---------|
|     Handle callbacks | Correctly handle callbacks from Vipps, both for successful and unsuccessful payments. See the API documentation for [how callback URLs are built](vipps-checkout-api.md#callback-handling), make test calls to make sure you handle the `POST` requests correctly. Vipps does not have capacity to manually do this for you. |
|     Handle errors | Make sure to log and handle [all errors](https://developer.vippsmobilepay.com/docs/APIs/ecom-api/vipps-ecom-api.md#errors). All integrations should display errors in a way that the users (customers and merchant employees/administrators) can see and understand them.|
|     Include Vipps HTTP headers | Send the [Vipps HTTP headers](https://developer.vippsmobilepay.com/docs/vipps-developers/common-topics/http-headers) in all API requests for better tracking and troubleshooting (mandatory for partners and platforms, who must send these headers as part of the checklist approval). |
|     Add information to the payment history| We recommend using the [Order Management API](https://developer.vippsmobilepay.com/docs/APIs/order-management-api) to add receipts and/or images to the payment history. This is a great benefit for the end user experience. It is also mandatory for merchants using ["Vipps Assisted Content Monitoring"](https://developer.vippsmobilepay.com/docs/APIs/order-management-api/vipps-order-management-api#vipps-assisted-content-monitoring). |

## Avoid integration pitfalls

| Action    | Comment |
|-----|-----------|
|     Send useful `reference` | Follow our [reference recommendations](https://developer.vippsmobilepay.com/docs/vipps-developers/common-topics/orderid). |
|     Poll for payment details | The Merchant _must not_ rely on `fallback` or `callback` alone, and must poll Payments Details [`GET:/epayment/v1/{reference}`][get-payment-endpoint] or Session Details [`GET:/checkout/v3/{reference}`][retrieve-sessioninfo-endpoint]  as documented (this is part of the first item in this checklist, but it's still a common error). For pure payment status polling the ePayment API is recommended.  Follow our [polling recommendations](https://developer.vippsmobilepay.com/docs/vipps-developers/common-topics/polling-guidelines). |
|     Handle redirects| The merchant must handle that the `fallback` URL is opened in the default browser on the phone, and not in a specific browser, in a specific tab, in an embedded browser, requiring a session token, etc. Follow our [recommendations regarding handling redirects](https://developer.vippsmobilepay.com/docs/vipps-developers/common-topics/redirects/). See the FAQ: [How can I open the fallback URL in a specific (embedded) browser?](https://developer.vippsmobilepay.com/docs/vipps-developers/faqs/common-problems-faq#how-can-i-open-the-fallback-url-in-a-specific-embedded-browser)|
|     Follow design guidelines| The Vipps branding must be according to the [Vipps design guidelines](https://developer.vippsmobilepay.com/docs/vipps-design-guidelines).|
|     Educate customer support| Make sure your customer service, etc. has all the tools and information they need available in _your_ system, through the APIs listed in the first item in this checklist, and that they do not need to visit [portal.vipps.no](https://portal.vipps.no) for normal work.|

[checkout-api-reference-url]: https://developer.vippsmobilepay.com/api/checkout
[create-checkout-session-endpoint]: https://developer.vippsmobilepay.com/api/checkout#tag/Session/paths/~1v3~1session/post
[retrieve-sessioninfo-endpoint]: https://developer.vippsmobilepay.com/api/checkout#tag/Session/paths/~1v3~1session~1%7BsessionId%7D/get
[epayment-api-reference-url]: https://developer.vippsmobilepay.com/api/epayment
[create-payment-endpoint]: https://developer.vippsmobilepay.com/api/epayment#tag/CreatePayments/operation/createPayment
[get-payment-endpoint]: https://developer.vippsmobilepay.com/api/epayment#tag/QueryPayments/operation/getPayment
[get-payment-event-log-endpoint]: https://developer.vippsmobilepay.com/api/epayment#tag/QueryPayments/operation/getPaymentEventLog
[cancel-payment-endpoint]: https://developer.vippsmobilepay.com/api/epayment#tag/AdjustPayments/operation/cancelPayment
[capture-payment-endpoint]: https://developer.vippsmobilepay.com/api/epayment#tag/AdjustPayments/operation/capturePayment
[refund-payment-endpoint]: https://developer.vippsmobilepay.com/api/epayment#tag/AdjustPayments/operation/refundPayment
[adjust-authorization-endpoint]: https://developer.vippsmobilepay.com/api/epayment#tag/AdjustPayments/operation/adjustAuthorization
[force-approve-endpoint]: https://developer.vippsmobilepay.com/api/epayment#tag/ForceApprove/operation/forceApprove
