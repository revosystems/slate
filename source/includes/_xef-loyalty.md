# Xef Loyalty

## Base URL

`https://revoxef.works/api/loyalty/`


## Create an order
Data can be passed as show in the example. Although it can also be passed as form params, passing `Content-Type: application/json` header. 

```
curl -XPOST -H 'tenant: {tenant}' -H 'Authorization: Bearer {token}' -d 'customer={
    "name":"Jordi",
    "email":"jordi.p@revo.works"
}&order={
    "subtotal" : 1336,
    "sum" : 1336,
    "table_id" : null,	// Optional (In case you want it into a table in xef)
    "tableName" : 		// Optional (In case you want to give another table name)
    "total" : 1460,
    "taxAmount" : 124,
    "notes" : "You can send some optional notes",
    "contents":[{
        "item_id" : 949,
        "quantity" : 1.000,
        "menuContents" : null,
        "id" : null,
        "subtotal" : 910,
        "itemPrice" : 1000,
        "tax" : 1000,
        "modifiers" : [],
        "total" : 1000,
        "taxAmount" : 90,
        "dishOrder" : 2, //Optional
        "notes" : "You can also send optional notes here"
    },{
        "item_id" : 2041,
        "quantity" : 1,
        "menuContents" : null,
        "id" : null,
        "subtotal" : 153,
        "itemPrice" : 160,
        "tax" : 2000,
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
        "modifiers" : [],
        "total" : 300,
        "taxAmount" : 27
    }
]}&delivery={  // Optional; object to specify delivery info.
    "channel": null,  // Optional; Code according to Delivery plarform (Deliveroo, Ubereats, Deliverect, Just Eats, etc.)
    "address":"Carrer del Bruc, 23, 1er, 08241 pis, Barcelona",      // Full address
    "phone": "666666666",  // Optional
    "date": "2020-09-23 14:01:59"
}
```

`POST orders`

Headers:

Parameter     | Value
--------------|-----------------
tenant        | account name
Authorization | Bearer {token}
client-token  | {client-token}

Body:

Parameter         | Type         | Description 
------------------|--------------|-------------------------
order             | `json`       | Json encoded order   
customer          | `array`      | *optional* Of name and email `["name" => "The name", "email" => "theEmail@example.com"]`
delivery          | `json`       | *optional* Delivery object `["address" => "The address ('Pickup' if delivery not needed)", "date" => "Datetime when order should be delivered", "phone" => "The phone", "channel" => "The delivery channel, if neeeded"]`
skipMerge		  | `bool`       | *optional* if you don't want the order to be merged if there is another one already opened
warehouse_id	  | `integer`    | *optional* Send the xef revo warehouse_id if you want the stock to be discounted

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
       "tip" : 0.50,
       "payment_reference" : "your-payment-reference",
       "payment_method_id" : 1
    }
    "emailToSendInvoice" : "anemail@example.com",
    "locale" : "en"
}

```

You can send a payment while creating the order as well by adding the `payment` element inside the order


Field               | Required | Description
--------------------|----------|--------------
`amount`            | yes      | The payment amount
`tip`               | no       | The tip for that payment
`payment_reference` | no       | A info field that references your payment id
`payment_method_id` | no       | In case you want it to match with a revo payment, or don't send to use the standard `InTouch` payment
`emailToSendInvoice`| no       | If you want to send the invoice to an email
`locale`            | no       | The locale to use when sending the email

## Standalone payment

```
payment={ 
    "amount" : 12.40,
    "tip" : 0.40,
    "payment_reference" : "your-payment-reference",
    "payment_method_id" : 1
}
```

Otherwise, you might need the order id before doing the payment, so you can add it afterwards with another call, where the parameters follow the same rules as the above call

`POST orders/{orderId}/payments`


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

During the next 5 minutes after an order has been place, you can cancel it (for example, if the defeered payment failed)

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

