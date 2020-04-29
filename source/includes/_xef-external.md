# Xef

## Prerequisistes

To be able to use the external api you need a `revo xef` account and a `access token`

1. Login into the desired account
2. Go to account management
3. Create a new [token](https://revoxef.works/account/tokens)


## Basic usage
The main URL for the external api is


`https://revoxef.works/api/external/v2/`

And you should provide the mandatory headers for the authentification


Header        | Value
--------------|----------
tenant        | {account-username}
Authorization | Bearer {the-token}


## Payment Methods

`GET PaymentMethods`

```sh
GET https://revoxef.works/api/external/v2/paymentMethods
```

> Response is an array of payment methods with the following fields:

```
"id": 1,
"name": "Payment method name",
"active": true,
```


## Orders

`GET Orders`

```sh
GET https://revoxef.works/api/external/v2/orders?from=2017-01-02&to=2017-01-04
```

> Response is paginated for every 50 

```sh
    "id": 4,
    "opened": "2017-12-15 08:40:52",
    "closed": "2017-01-03 00:00:00",
    "merged": null,
    "canceled": null,
    "guests": "4",
    "status": "0",
    "orderDiscountAmount": "0",
    "sum": "0",
    "discountAmount": "0",
    "subtotal": "0",
    "taxAmount": "0",
    "total": "14.5",
    "alreadyPaid": "0.0",
    "tenantUser_id": null,
    "tenantUserName": null,
    "discount": null,
    "table_id": null,
    "tableName": null,
    "margin": null,
    "delivery_id": null,
    "orderInvoices": [],
    "delivery": null,
    "orderContents": [
        {
            "id": 1,
            "order_id": "4",
            "item_id": "1",
            "dishOrder": "0",
            "seat": "0",
            "quantity": "2",
            "weight": "0",
            "itemName": "Oran Cartwright",
            "itemPrice": "17.49",
            "alreadyPaidQuantity": "0",
            "alreadyPrintedQuantity": "0",
            "modifiers": null,
            "discount": null,
            "taxPercentage": "0",
            "discountAmount": "79.84",
            "taxAmount": "85.91",
            "total": "98.98",
            "extrasAmount": "12.96",
            "subtotal": "74.96",
            "priceWithExtrasIndividual": "66.52",
            "notes": null,
            "price_id": null,
            "optional_modifiers": null,
            "format_pivot_id": null,
            "margin": null,
            "menuMenuContents": [],
        }
       ],
      } 
```

| Field    | Required | Description                                                                 |
| ---------|----------|-----------------------------------------------------------------------------|
| `from`   | optional | The starting date of the orders fetched                                     |
| `to`     | optional | The ending date of the orders fetched                                       |
| `page`   | optional | As the orders are paginated, use this parameter to select the page to fetch |

  
> You can add `?withPayments` parameter. This way apart from `orderContents` another array will be loaded called `orderInvoices`, it will contain an array of invoices and each invoice will contain an array of `orderPayments`. 

  
```sh  
"orderContents": [{…}, {…}, …],
"orderInvoices": [
    {
        "orderPayments": [
            {
                "payAmount": 10,
                "paymentMethod": 1,
                …
            }, {…}, …
        ], …
    }, {…}, …
]
```  
  
> You can add **?withRooms** parameter. This way apart from `orderContents` another array will be loaded called `table` which will contain a `room`. 
  
```sh
"orderContents": [{…}, {…}, …],
"table": {
    "id"        : <table_id>,
    "name"      : <tableName>,
    "room_id"   : <room_id>,
    "room"      : {
        "id"    : <room_id>,
        "name"  : <roomName>
    },
}
```  

## Rooms

`GET Rooms`

```sh
GET https://revoxef.works/api/external/v2/rooms
```

> Response is paginated for every 20 rooms 

```sh
    "id": 1,
    "name": "Room 1",
    "order": "0",
    "active": "1",
```
  
> You can add `?withTables` parameter. This way apart from `rooms` another array will be loaded called `tables`, it will contain a tables array within each room. 

  
```sh  
"tables": [
    {
        "id": 1,
        "name": "Table 1",
        "x": "0",
        "y": "0",
        "width": "100",
        "height": "100",
        "baseX": "0",
        "baseY": "0",
        "isJoined": "0",
        "baseWidth": "100",
        "baseHeight": "100",
        "color": "#dddddd",
        "type_id": "2",
        "room_id": "1",
        "price_id": null,
    }
]
```  

## GiftCards

`POST GiftCards`

| Query parameter |          | Description                                                                           |
| ----------------|----------|---------------------------------------------------------------------------------------|
| balance         | required | Card amount                                                                           |
| uuid            | required | Unique identifier for the card                                                        |
| campaign_id     | optional | Campaign ID when giftcard should be linked with a campaign (get ID from backend)      |


```sh
GET https://revoxef.works/api/external/v2/giftCards
```

> Response

```sh
{
    "balance": 1.25,
    "uuid": "123456"
    "campaign_id": "3"
}
```

## Responses

Value | Meaning                    | Description
------|----------------------------|------------------------------------------------
201   | HTTP_CREATED               | Call succeed
422   | HTTP_UNPROCESSABLE_ENTITY  | Call failed
