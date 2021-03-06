If we want to reverse a previously captured amount, we need to perform
`create-paymentorder-reversal` against the accompanying `href` returned in the
`operations` list. See the abbreviated request and response below:

{:.code-header}
**Request**

```http
POST /psp/paymentorders/{{ page.payment_order_id }}/reversals HTTP/1.1
Host: {{ page.api_host }}
Authorization: Bearer <AccessToken>
Content-Type: application/json

{
    "transaction": {
        "description": "Reversal of captured transaction",
        "amount": 1500,
        "vatAmount": 375,
        "payeeReference": "ABC123",
        "orderItems": [
            {
                "reference": "P1",
                "name": "Product1",
                "type": "PRODUCT",
                "class": "ProductGroup1",
                "itemUrl": "https://example.com/products/123",
                "imageUrl": "https://example.com/product123.jpg",
                "description": "Product 1 description",
                "discountDescription": "Volume discount",
                "quantity": 4,
                "quantityUnit": "pcs",
                "unitPrice": 300,
                "discountPrice": 200,
                "vatPercent": 2500,
                "amount": 1000,
                "vatAmount": 250
            },
            {
                "reference": "P2",
                "name": "Product2",
                "type": "PRODUCT",
                "class": "ProductGroup1",
                "description": "Product 2 description",
                "quantity": 1,
                "quantityUnit": "pcs",
                "unitPrice": 500,
                "vatPercent": 2500,
                "amount": 500,
                "vatAmount": 125
            }
        ]
    }
}
```

{:.table .table-striped}
| Required | Property                       | Type         | Description                                                                                                                                                                                                                                                                                                           |
| :------: | :----------------------------- | :----------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|  ✔︎︎︎︎︎  | `transaction`                  | `object`     | The transaction object.                                                                                                                                                                                                                                                                                               |
|  ✔︎︎︎︎︎  | └➔&nbsp;`amount`               | `integer`    | The amount including VAT in the lowest monetary unit of the currency. E.g. `10000` equals 100.00 NOK and `5000` equals 50.00 NOK.                                                                                                                                                                                     |
|  ✔︎︎︎︎︎  | └➔&nbsp;`vatAmount`            | `integer`    | The amount of VAT in the lowest monetary unit of the currency. E.g. `10000` equals 100.00 NOK and `5000` equals 50.00 NOK.                                                                                                                                                                                            |
|  ✔︎︎︎︎︎  | └➔&nbsp;`payeeReference`       | `string(30)` | A unique reference from the merchant system. It is set per operation to ensure an exactly-once delivery of a transactional operation. See [payeeReference][payee-reference] for details.                                                                                                                              |
|  ✔︎︎︎︎︎  | └➔&nbsp;`description`          | `string`     | Textual description of why the transaction is reversed.                                                                                                                                                                                                                                                               |
|  ✔︎︎︎︎︎  | └➔&nbsp;`orderItems`           | `array`      | The array of items being purchased with the order. Used to print on invoices if the payer chooses to pay with invoice, among other things. Required in `capture` requests if already sent with the initial creation of the Payment Order. Note that this should only contain the items to be captured from the order. |
|  ✔︎︎︎︎︎  | └─➔&nbsp;`reference`           | `string`     | A reference that identifies the order item.                                                                                                                                                                                                                                                                           |
|  ✔︎︎︎︎︎  | └─➔&nbsp;`name`                | `string`     | The name of the order item.                                                                                                                                                                                                                                                                                           |
|  ✔︎︎︎︎︎  | └─➔&nbsp;`type`                | `enum`       | `PRODUCT`, `SERVICE`, `SHIPPING_FEE`, `PAYMENT_FEE`, `DISCOUNT`, `VALUE_CODE` or `OTHER`. The type of the order item.                                                                                                                                                                                                 |
|  ✔︎︎︎︎︎  | └─➔&nbsp;`class`               | `string`     | The classification of the order item. Can be used for assigning the order item to a specific product category, such as `MobilePhone`. Note that `class` cannot contain spaces. Swedbank Pay may use this field for statistics.                                                                                        |
|          | └─➔&nbsp;`itemUrl`             | `string`     | The URL to a page that can display the purchased item, product or similar.                                                                                                                                                                                                                                            |
|          | └─➔&nbsp;`imageUrl`            | `string`     | The URL to an image of the order item.                                                                                                                                                                                                                                                                                |
|          | └─➔&nbsp;`description`         | `string`     | The human readable description of the order item.                                                                                                                                                                                                                                                                     |
|          | └─➔&nbsp;`discountDescription` | `string`     | The human readable description of the possible discount.                                                                                                                                                                                                                                                              |
|  ✔︎︎︎︎︎  | └─➔&nbsp;`quantity`            | `integer`    | The 4 decimal precision quantity of order items being purchased.                                                                                                                                                                                                                                                      |
|  ✔︎︎︎︎︎  | └─➔&nbsp;`quantityUnit`        | `string`     | The unit of the quantity, such as `pcs`, `grams`, or similar. This is a free-text field and is used for your own book keeping.                                                                                                                                                                                        |
|  ✔︎︎︎︎︎  | └─➔&nbsp;`unitPrice`           | `integer`    | The price per unit of order item, including VAT.                                                                                                                                                                                                                                                                      |
|          | └─➔&nbsp;`discountPrice`       | `integer`    | If the order item is purchased at a discounted price. This property should contain that price, including VAT.                                                                                                                                                                                                         |
|  ✔︎︎︎︎︎  | └─➔&nbsp;`vatPercent`          | `integer`    | The percent value of the VAT multiplied by 100, so `25%` becomes `2500`.                                                                                                                                                                                                                                              |
|  ✔︎︎︎︎︎  | └─➔&nbsp;`amount`              | `integer`    | The total amount including VAT to be paid for the specified quantity of this order item, in the lowest monetary unit of the currency. E.g. `10000` equals `100.00 SEK` and `5000` equals `50.00 SEK`.                                                                                                                 |
|  ✔︎︎︎︎︎  | └─➔&nbsp;`vatAmount`           | `integer`    | The total amount of VAT to be paid for the specified quantity of this order item, in the lowest monetary unit of the currency. E.g. `10000` equals `100.00 SEK` and `5000` equals `50.00 SEK`.                                                                                                                        |

If the reversal request succeeds, the response should be similar to the example below:

{:.code-header}
**Response**

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "payment": "/psp/creditcard/payments/{{ page.payment_order_id }}",
    "reversals": {
        "id": "/psp/creditcard/payments/{{ page.payment_order_id }}/cancellations/{{ page.transaction_id }}",
        "transaction": {
            "id": "/psp/creditcard/payments/{{ page.payment_order_id }}/transactions/{{ page.transaction_id }}",
            "type": "Reversal",
            "state": "Completed",
            "amount": 15610,
            "vatAmount": 3122,
            "description": "Reversing the capture amount",
            "payeeReference": "ABC987"
        }
    }
}
```

{:.table .table-striped}
| Property                  | Type      | Description                                                                                                                                                                                                  |
| :------------------------ | :-------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `payment`                 | `string`  | The relative URI of the payment this reversal transaction belongs to.                                                                                                                                        |
| `reversals`               | `object`  | The reversal object, containing information about the reversal transaction.                                                                                                                                  |
| └➔&nbsp;`id`              | `string`  | The relative URI of the reversal transaction.                                                                                                                                                                |
| └➔&nbsp;`transaction`     | `object`  | The transaction object, containing information about the current transaction.                                                                                                                                |
| └─➔&nbsp;`id`             | `string`  | The relative URI of the current `transaction` resource.                                                                                                                                                      |
| └─➔&nbsp;`created`        | `string`  | The ISO-8601 date and time of when the transaction was created.                                                                                                                                              |
| └─➔&nbsp;`updated`        | `string`  | The ISO-8601 date and time of when the transaction was created.                                                                                                                                              |
| └─➔&nbsp;`type`           | `string`  | Indicates the transaction type.                                                                                                                                                                              |
| └─➔&nbsp;`state`          | `string`  | `Initialized`, `Completed` or `Failed`. Indicates the state of the transaction.                                                                                                                              |
| └─➔&nbsp;`number`         | `string`  | The transaction `number`, useful when there's need to reference the transaction in human communication. Not usable for programmatic identification of the transaction, for that `id` should be used instead. |
| └─➔&nbsp;`amount`         | `integer` | Amount is entered in the lowest momentary units of the selected currency. E.g. `10000` = 100.00 NOK, `5000` = 50.00 SEK.                                                                                     |
| └─➔&nbsp;`vatAmount`      | `integer` | If the amount given includes VAT, this may be displayed for the user in the payment page (redirect only). Set to 0 (zero) if this is not relevant.                                                           |
| └─➔&nbsp;`description`    | `string`  | A human readable description of maximum 40 characters of the transaction.                                                                                                                                    |
| └─➔&nbsp;`payeeReference` | `string`  | A unique reference for the transaction. See [`payeeReference`][payee-reference] for details.                                                                                                                 |
