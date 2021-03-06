## Unscheduled Purchase

An `unscheduled purchase`, also called a Merchant Initiated Transaction (MIT),
is a payment which uses a `paymentToken` created through a previous payment in
order to charge the same card at a later time. They are done by the merchant
without the cardholder being present.  

`unscheduled purchase`s differ from `recur` as they are done as singular
transactions, and not recurring ones. Examples of this can be car rental
companies charging the payer's card for toll road expenses after the rental
period.

{:.code-header}
**Request**

```http
POST /psp/creditcard/payments HTTP/1.1
Host: {{ page.apiHost }}
Authorization: Bearer <AccessToken>
Content-Type: application/json

{
    "payment": {
        "operation": "UnscheduledPurchase",
        "intent": "Authorization",
        "paymentToken": "{{ page.paymentId }}",
        "currency": "NOK",
        "amount": 1500,
        "vatAmount": 0,
        "description": "Test Recurrence",
        "userAgent": "Mozilla/5.0...",
        "language": "nb-NO",
        "urls": {
            "callbackUrl": "https://example.com/payment-callback"
        },
        "payeeInfo": {
            "payeeId": "{{ page.merchantId }}"
            "payeeReference": "CD1234",
            "payeeName": "Merchant1",
            "productCategory": "A123",
            "orderReference": "or-12456",
            "subsite": "MySubsite"
        }
    }
}
```
**Response**

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "payment": {
    "id": "/psp/creditcard/payments/5adc265f-f87f-4313-577e-08d3dca1a26c",
    "number": 1234567890,
    "created": "2016-09-14T13:21:29.3182115Z",
    "updated": "2016-09-14T13:21:57.6627579Z",
    "state": "Ready|Pending|Failed|Aborted",
    "operation": "PurchaseDirect",
    "intent": "Authorization",
    "currency": "NOK",
    "amount": 1500,
    "remainingCaptureAmount": 1500,
    "remainingCancellationAmount": 1500,
    "remainingReversalAmount": 0,
    "description": "Test Unscheduled",
    "initiatingSystemUserAgent": "PostmanRuntime/3.0.1",
    "userAgent": "Mozilla/5.0...",
    "language": "nb-NO",
    "paymentToken": "{{ page.paymentId }}", 
    "prices": { "id": "/psp/creditcard/payments/{{ page.paymentId }}/prices" },
    "transactions": { "id": "/psp/creditcard/payments/{{ page.paymentId }}/transactions" },
    "authorizations": { "id": "/psp/creditcard/payments/{{ page.paymentId }}/authorizations" },
    "captures": { "id": "/psp/creditcard/payments/{{ page.paymentId }}/captures" },
    "reversals": { "id": "/psp/creditcard/payments/{{ page.paymentId }}/reversals" },
    "cancellations": { "id": "/psp/creditcard/payments/{{ page.paymentId }}/cancellations" },
    "urls" : { "id": "/psp/creditcard/payments/{{ page.paymentId }}/urls" },
    "payeeInfo" : { "id": "/psp/creditcard/payments/{{ page.paymentId }}/payeeInfo" }
  }
}
```
