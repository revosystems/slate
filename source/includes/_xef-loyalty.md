# Xef Loyalty

## Basic usage
The main URL for the external API:

`https://revoxef.works/api/loyalty`

> The API has a data size límit of 2MB for each request.

> The API has a limit of 120 requests every minute so the best practice is to cache for x time the fetched information done with the `getXXX` actions.

The URL for the integrations API environment:

`https://integrations.revoxef.works/api/loyalty`

And you should provide the mandatory headers for the authentication


Header        | Value
--------------|----------
tenant        | {account-username}
Authorization | Bearer {the-token}
client-token  | {client-token}
Content-Type  | application/json

## Create an order

POST `https://revoxef.works/api/loyalty/orders`

The payload to create the order, must be sent as a JSON.

The parameters for the payload, are the following:

| Field        | Type    | Required     | Extra info                                                                                        |
|--------------|---------|--------------|---------------------------------------------------------------------------------------------------|
| order        | JSON    | **required** | [see Order payload](#order-payload)                                                               |
| customer     | JSON    | optional     | [see Customer payload](#customer-payload)                                                         |
| delivery     | JSON    | optional     | [see Delivery payload](#delivery-payload)                                                         |
| skipMerge    | boolean | optional     | default: false. If you don't want the order to be merged if there is another one already opened.  |
| warehouse_id | number  | optional     | Send the Revo XEF [warehouse_id](#warehouses) if you want the stock to be discounted.                            |
| emailToSendInvoice | string  | optional     | Email where the invoice will be sent. |

> General payload example:

```sh
{
    "order": {
        ...
    },
    "customer": {
        ...
    },
    "delivery": {
        ...
    }
}
```

> Response:

```sh
{
    "order_id": 1
}
```

### Order payload

Most of the parameters are recalculated when the order is processed in the POS.

| Field         | Type    | Required     | Extra info                                                                                       |
|---------------|---------|--------------|--------------------------------------------------------------------------------------------------|
| total         | decimal | **required** | must be the sum of the order contents with their extra modifiers. Discounts must be applied too. |
| table_id      | number  | optional     | If not sent, order is set as a delivery.                                                         |
| tableName     | string  | optional     | default: 'InTouch'. If table_id sent, gets real tableName, otherwise gets payload data.          |
| guests        | number  | optional     | default: 1                                                                                       |
| notes         | string  | optional     |                                                                                                  |
| extraId       | string  | optional     | Used to register an external order_id/reference.                                                 |
| orderDiscount | JSON    | optional     | [see Order discount payload](#order-discount-payload)                                            |
| contents      | array   | **required** | Must be an array of JSON. [see Contents payload](#contents-payload)                              |
| payment      | JSON    | optional     | [see Payment payload](#payment-payload)                                                           |

> Order payload example:

```sh
{
    "order": {
        "total": 10.50,
        "table_id": 1,
        "tableName": "TableName",
        "guests": 1,
        "notes": "Notes",
        "extraId": "Ext-1",
        "orderDiscount": {
            ...
        },
        "contents": [
            {
                ...
            },
            ...
        ],
        "payment": {
            ...
        },
    }
    ...
}
```

### Order discount payload

| Field  | Type    | Required     | Extra info               |
|--------|---------|--------------|--------------------------|
| name   | string  | optional     | default: 'Discount'      |
| amount | decimal | **required** | Must have negative sign. |

> Order discount payload example:

```sh
{
    "order": {
        "total": 8.00, // discount subtracted
        ...
        "orderDiscount": {
            "name": "Discount",
            "amount": -2.50
        },
        ...
    }
    ...
}
```

### Contents payload

| Field  | Type    | Required     | Extra info               |
|--------|---------|--------------|--------------------------|
| item_id   | number  | **required** | Existing [item](#items) ID. It can be a normal item, a selling format item or a menu item. |
| itemPrice | decimal | **required** | Individual item price. |
| quantity | number | optional | default: 1 |
| modifiers | array   | optional | **Only for normal and selling format items.** Must be an array of JSON. [see Modifiers payload](#modifiers-payload)
| menuContents | array   | optional | **Only for menu items.** Must be an array of JSON. [see MenuContents payload](#menucontents-payload)   

> Content payload example:

```sh
{
    "order": {
        "total": 13.00
        ...
        "contents": {
            "item_id": 1,
            "itemPrice": 6.50,
            "quantity": 2,
            "modifiers" or "menuContents": [
                {
                    ...
                },
                ...
            ],
        }
        ...
    }
    ...
}
```

### Modifiers payload

**Only for normal and selling format items.**

For normal items (item.type = 0):

There is no quantity and each modifier must be sent idividually.

| Field  | Type    | Required     | Extra info               |
|--------|---------|--------------|--------------------------|
| id   | number  | optional | Existing [modifier](#modifiers) ID. |
| name | string | **required** | If modifier ID is found, it is set the modifier name. |
| price | decimal | **required** | - |

> Modifier (normal item) payload example:

```sh
{
    "order": {
        "total": 17.80
        ...
        "contents": {
            "itemPrice": 6.50,
            "quantity": 2,
            ...
            "modifiers": [
                {
                    "id": 1, // modifier ID exists ("name": "Nuts")
                    "price": 1.20
                },
                {
                    "name": "Nuts",
                    "price": 1.20
                },
                ...
            ]
        }
        ...
    }
    ...
}
```

For selling format items (item.type = 4):

**Normal modifiers cannot be set.**

| Field  | Type    | Required     | Extra info               |
|--------|---------|--------------|--------------------------|
| id   | number  | **required** | Existing [format pivot](#selling-format-pivots) ID. |
| price | decimal | optional | default: 0 |

> Modifier (selling format item) payload example:

```sh
{
    "order": {
        ...
        "contents": {
            ...
            "modifiers": [ // only 1 modifier to set the selling format
                {
                    "id": 1,
                }
            ]
        }
        ...
    }
    ...
}
```


### MenuContents payload

**Only for menu items (item.type = 1):**

| Field  | Type    | Required     | Extra info               |
|--------|---------|--------------|--------------------------|
| item_id   | number  | **required** | Existing [item](#items) ID. It can be a normal menu item or a selling format menu item. |
| name | string | **required** | Menu ittem name.
| price | decimal | **required** | Item menu price.
| quantity | number | optional | Only can be set 1 quantity. |
| dishOrder | number | optional | **Must be an existing [Menu Item-Category Pivot](#menu-item-category-pivot).** |
| modifiers | array  | optional | **Only for normal and selling format items.** Must be an array of JSON. [see Modifiers payload](#modifiers-payload)

> MenuContents (menu item) payload example:

```sh
{
    "order": {
        "total": 25.70
        ...
        "contents": {
            "item_id": 12, // menu item
            "itemPrice": 10.00,
            ...
            "menuContents": [
                {
                    "item_id": 1, // normal item
                    "price": 6.50,
                    "quantity": 1,
                    "modifiers": [
                        {
                            "name": "Nuts",
                            "price": 1.20
                        },
                        ...
                    ]
                },
                {
                    "item_id": 2, // selling format item
                    "itemPrice": 7,
                    "quantity": 1,
                    "modifiers": [
                        {
                            "id",
                            "price": 1.00
                        }
                    ]
                }
                ...
            ]
        }
        ...
    }
    ...
}
```

### Payment payload

| Field  | Type    | Required     | Extra info               |
|--------|---------|--------------|--------------------------|
| amount | decimal | **required** | The amount to pay. Must not exceed the order total amount - order already paid amount. |
| payment_method_id | number | optional | Existing [payment method](#payment-methods) ID. default: channel payment method is used or created. |
| payment_reference | string | optional | The reference of the payment. |
| tip | decimal | optional |  |
| contents | array | optional | **Only for [Standalone payment](#standalone-payment) section.** Pay per items. Array of content IDs. The amount must match contents+modifiers sum amount. |

Regarding the payment methods, there are 2 fix payment methods for all accounts:

- Card => "payment_method_id": 1

- Cash => "payment_method_id": 2

> Payment payload example:

```sh
{
    "order": {
        "total": 12.20,
        ...
        "payment": {
            "amount": 12.20
            "payment_method_id": 1,
            "payment_reference": "QR code payment.",
            ...
        }
    }
    ...
}
```

### Customer payload

If a customer is found with the email or phone from this section, respectively, we will get the existing customer, and the other fields will not be updated.

If the customer is not found, we will create one with all the fields sent.

**The customer info and the delivery info are not the same.** Customer object is the customer itself, the customer data linked to the invoice. The delivery object is a complement data only linked to the delivery order. It is up to you if you want to match customer and delivery fields, in case of non existing customer.

We recommend to manage customers using the specific endpoints. Check the [customer section](#customers)

| Field  | Type    | Required     | Extra info               |
|--------|---------|--------------|--------------------------|
| name | string | **required** | |
| email | string | **required** | |
| phone | string | optional | |
| nif | string | optional | |
| web | string | optional | |
| address | string | optional | |
| postalCode | string | optional | |
| city | string | optional | |
| state | string | optional | |
| extra_id | string | optional | |
| notes | string | optional | |

> Customer payload example:

```sh
{
    "order": {
        ...
    },
    "customer": {
        "name": "Customer Name",
        "email": "customer@revo.test"
    }
    ...
}
```

### Delivery payload

**The delivery info and the customer info are not the same.**

The delivery data is the info for the delivery/pickup order. This info will not be saved in the customer data.

| Field  | Type    | Required     | Extra info               |
|--------|---------|--------------|--------------------------|
| phone | number | **required** | The phone for the delivery/pickup order. |
| date | string | optional | Date when the order has to be prepared. "YYYY-MM-DD HH:MM:SS" - default: current time.  |
| address | string | optional | Set address for delivery orders. NO address for pickup orders. |
| city | string | optional | |
| channel | number | optional | default: REVO channel (InTouch) |


> Delivery payload example:

```sh
{
    "order": {
        ...
    },
    "delivery": {
        "phone": 123456789,
        "date": "2009-05-02 21:45:00",
        ...
    }
    ...
}
```

## Standalone payment

If you only want to make a payment for an existing order, you must use the following endpoint:

`POST orders/{orderId}/payments`

**The payload structure is the same as the payment done in the order creation, but sent as a form-data body request.** [See again the details.](#payment-payload)

> Standalone payload example:

```sh
# Must be sent as a form-data body request.
"payment": {
    "amount": 12.20
    "payment_method_id": 1,
    "payment_reference": "QR code payment.",
    "contents": [1,2,3]
    ...
}
```

## Fetch an Order
`GET orders/{orderId}`


You can fetch an order information using this endpoint

## Fetch an Order with Table Id

If you want to fetch an order for an specific table you can use this endpoint.

`GET tables/{tableId}/order`


> It will return a 404 if there is no open order at that table at the moment


## Fetch multiple Order with list of tables id

If you want to fetch an order for an specific table you can use this endpoint.
It will return the `open` orders paginated by 50.

`GET orders?table_id[]=5&table_id[]=6&table_id[]=7`

> You can send the array using the body as well instead of the query




> It will return a 404 if there is no open order at that table at the moment


## Cancel Order
```
curl -XDELETE -H 'tenant: {tenant}' -H 'Authorization: Bearer {token}' \
-d 'reason="Payment Failed"' \
'https://revoxef.works/api/loyalty/orders/123456'
```

This endpoint makes a refund (a new order with negatives values).

There is a limit time to cancel the order. If the account has configured the [Previous delivery minutes](https://revoxef.works/config/delivery), the limit to cancel the order will be the *Delivery date* subtract the *Delivery minutes*. Otherwise, you will have 5 minutes after an order has been place.

`DELETE orders/{orderId}`

Field             | Required | Description
------------------|----------|--------------
`reason`          | yes      | The reason why the order is canceled

## Fetch Invoice details

You can use this endpoint to fetch an specific invoice details

`GET invoices/{invoiceId}`


## Fetch tables

Get all the tables with is name and id

`GET tables`

## Tables availability

```
{
    "available" : true
}

```

Use this method to know if a table is available or not (there is no open order on it)

`GET tables/{tableId}/available`

