
A `recur` payment is a payment that references a `recurrenceToken` created
through a previous payment in order to charge the same card.

{:.code-header}
**Request**

```http
POST /psp/creditcard/payments HTTP/1.1
Host: {{ page.apiHost }}
Authorization: Bearer <AccessToken>
Content-Type: application/json

{
    "payment": {
        "operation": "Recur",
        "intent": "Authorization|AutoCapture",
        "recurrenceToken": "{{ page.paymentId }}",
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