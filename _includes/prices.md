{% assign payment_instrument = include.payment_instrument | default: 'creditcard' %}

The `prices` resource lists the prices related to a specific payment.

{:.code-header}
**Request**

```http
GET /psp/{{ payment_instrument }}/payments/{{ page.payment_id }}/prices/ HTTP/1.1
Host: {{ page.api_host }}
Authorization: Bearer <AccessToken>
Content-Type: application/json
```

{:.code-header}
**Response**

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "payment": "/psp/{{ payment_instrument }}/payments/{{ page.payment_id }}",
    "prices": {
        "id": "/psp/{{ payment_instrument }}/payments/{{ page.payment_id }}/prices",
        "priceList": [
            {
                "type": "VISA",
                "amount": 2350,
                "vatAmount": 0
            },
            {
                "type": "MasterCard",
                "amount": 2350,
                "vatAmount": 0
            }
        ]
    }
}
```

{:.table .table-striped}
| Property             | Type      | Description                                                                                                                                                                                 |
| :------------------- | :-------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `payment`            | `string`  | The relative URI of the  `payment`  the prices resource belongs to.                                                                                                                         |
| `prices`             | `object`  | The `prices`  resource.                                                                                                                                                                     |
| └➔&nbsp;`id`         | `string`  | The relative URI of the current  `prices`  resource.                                                                                                                                        |
| └➔&nbsp;`priceList`  | `array`   | The array of price objects. **Note:** Even if you specifiy  `CreditCard`  in the input message the system will return all your configured card brands instead when you expan the priceList. |
| └─➔&nbsp;`type`      | `string`  | The type of the price object.                                                                                                                                                               |
| └─➔&nbsp;`amount`    | `integer` | Amount is entered in the lowest momentary units of the selected currency. E.g. 10000 = 100.00 NOK, 5000 = 50.00 SEK.                                                                        |
| └─➔&nbsp;`vatAmount` | `integer` | If the amount given includes VAT, this may be displayed for the user in the payment page (redirect only). Set to 0 (zero) if this is not relevant.                                          |

### Prices Object Types

Each payment instrument have one or more prices object types. This is most
relevant when using card based payments as each type correspond
to a card brand or bank respectively.

{% case payment_instrument %}

{% when "creditcard" %}

#### Card Payments

The generic type `CreditCard` enables all card brands, supported by merchant
contract.

{:.table .table-striped}
| Type            | Description                                |
| :-------------- | :----------------------------------------- |
| `Visa`          | Visa                                       |
| `MasterCard`    | MasterCard                                 |
| `Amex`          | American Express                           |
| `Dankort`       | Dankort can only be used with DKK currency |
| `Diners`        | Diners Club                                |
| `Finax`         | Finax                                      |
| `Jcb`           | JCB                                        |
| `IkanoFinansDK` | Ikano Finans Denmark                       |
| `Maestro`       | MasterCard Maestro                         |

{% when "invoice" %}

#### Invoice Payments

{:.table .table-striped}
| Type      | Description    |
| :-------- | :------------- |
| `Invoice` | Always Invoice |

{% when "mobilepay" %}

#### MobilePay Payments

{:.table .table-striped}
| Type        | Description      |
| :---------- | :--------------- |
| `Mobilepay` | Always MobilePay |

{% when "swish" %}

#### Swish Payments

{:.table .table-striped}
| Type    | Description  |
| :------ | :----------- |
| `Swish` | Always Swish |

{% when "vipps" %}

#### Vipps Payments

{:.table .table-striped}
| Type    | Description  |
| :------ | :----------- |
| `Vipps` | Always Vipps |

{% endcase %}
