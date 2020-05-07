# Xef Loyalty

## Base URL

`https://revoxef.works/api/loyalty/`


## Create an order

```
curl -XPOST -H 'tenant: {tenant}' -H 'Authorization: Bearer {token}' -d 'customer={
    "name":"Jordi",
    "email":"jordi.p@revo.works"
}&order={
    "subtotal" : 1336,
    "sum" : 1336,
    "id" : null,
    "tableId" : null,
    "total" : 1460,
    "taxAmount" : 124,
    "notes" : "You can send some optional notes"
    "contents":[{
        "item_id" : 949,
        "quantity" : 1,
        "menuContents" : null,
        "id" : null,
        "subtotal" : 910,
        "itemPrice" : 1000,
        "tax" : 1000,
        "pointsSpent" : null,
        "modifiers" : [],
        "total" : 1000,
        "taxAmount" : 90
    },{
        "item_id" : 2041,
        "quantity" : 1,
        "menuContents" : null,
        "id" : null,
        "subtotal" : 153,
        "itemPrice" : 160,
        "tax" : 2000,
        "pointsSpent" : null,
        "modifiers" : [],
        "total" : 160,
        "taxAmount" : 7
    },{
        "item_id" : 927,
        "quantity" : 1,
        "menuContents" : null,
        "id" : null,
        "subtotal" : 273,
        "itemPrice" : 300,
        "tax" : 1000,
        "pointsSpent" : null,
        "modifiers" : [],
        "total" : 300,
        "taxAmount" : 27
    }
]}&notification_hook="service:http://www.example.com"' 'https://revoxef.works/api/loyalty/orders'
```

`POST orders`

Headers:

Parameter     | Value
--------------|-----------------
tenant        | account name
Authorization | Bearer {token}

Body:

Parameter         | Type         | Description 
------------------|--------------|-------------------------
order             | `json`       | Json encoded order   
customer          | `array`      | Of name and email `["name" => "The name", "email" => "theEmail@example.com"]`
notification_hook | `string`     | An url where to notify the order status changes prefixed with the service id `service:https://notificationurl.com/notify`


```
Response:
{
    "total"     : 12.23,
    "taxAmount" : 1.23,
    "subtotal"  : 10,
    "sum"       : 10,
    "contents"  : [
        [
            "item_id"       : 1,
            "total"         : 7,
            "taxAmount"     : 2,
            "subtotal"      : 5,
            "itemPrice"     : 7,
            "taxPercentage" : 10,
        ], [
            "item_id"       : 2,
            "total"         : 7,
            "taxAmount"     : 2,
            "subtotal"      : 5,
            "itemPrice"     : 7,
            "taxPercentage" : 10,
        ]
    ]
}
```

## Order with payment

```
order={
    "contents" : [{
        ...
    }],
    "payment" : { 
       "amount" : 12.40,
        "payment_reference" : "your-payment-reference",
        "payment_method_id" : 1
    }
}

```

You can send a payment while creating the order as well by adding the `payment` element inside the order


Field               | Required | Description
--------------------|----------|--------------
`amount`            | yes      | The payment amount
`payment_reference` | no       | A info field that references your payment id
`payment_method_id` | no       | In case you want it to match with a revo payment, or don't send to use the standard `InTouch` payment

## Standalone payment

```
payment={ 
   "amount" : 12.40,
    "payment_reference" : "your-payment-reference",
    "payment_method_id" : 1
}
```

Otherwise, you might need the order id before doing the payment, so you can add it afterwards with another call, where the parameters follow the same rules as the above call

`POST orders/{orderId}/payments`


## Cancel Order
```
curl -XDELETE -H 'tenant: {tenant}' -H 'Authorization: Bearer {token}' \
-d 'reason="Payment Failed"' \
'https://revoxef.works/api/loyalty/orders/123456'
```

During the next 5 minutes after an order has been place, you can cancel it (for example, if the defeered payment failed)

`DELETE orders/{orderId}`

Field             | Required | Description
------------------|----------|--------------
`reason`          | yes      | The reason why the order is canceled


