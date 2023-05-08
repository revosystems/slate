# Xef

## Prerequisites

To be able to use the external api you need a `revo xef` account and a `access token` and your `integrator token`

1. Login into the desired account
2. Go to account management
3. Create a new [token](https://revoxef.works/account/tokens)
4. To get your integration token, [contact us](https://business.revo.works/altaleads/integrations)


## Basic usage
The main URL for the external api is


`https://revoxef.works/api/external/v2/`

And you should provide the mandatory headers for the authentication


Header        | Value
--------------|----------
tenant        | {account-username}
Authorization | Bearer {the-token}
client-token  | {your-integrator-token}


> Contact us to get your *client-token*


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

| Field        | Type       | Required | Description                                                                 |
| -------------|------------|----------|-----------------------------------------------------------------------------|
| `from`       | YYYY-mm-dd | required | The starting date of the orders fetched.                                     |
| `to`         | YYYY-mm-dd | required | The ending date of the orders fetched.                                       |
| `page`       | number     | optional | As the orders are paginated, use this parameter to select the page to fetch. |
| `pagination` | number     | optional | Number of objects per page. The default value is **50** and the max allowed is **200**. |

  
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


## Use Gift Card

> Response

```sh
{
    "balance": 1.25,
    "customer_id": "3"
}
```

`POST GiftCards/{uuid}`



| Query parameter |          | Description                                                                           |
| ----------------|----------|---------------------------------------------------------------------------------------|
| uuid            | required | Unique identifier for the card                                                        |


| Post parameter  |          | Description                                                                            |
| ----------------|----------|---------------------------------------------------------------------------------------|
| amount          | required | The amount to discount                                                                |




## Customers

`GET Customers`

```sh
GET https://revoxef.works/api/external/v2/customers
GET https://revoxef.works/api/external/v2/customers/<customer_id>
POST https://revoxef.works/api/external/v2/customers
    Create a customer:          {"name": "Customer 1", "active": 1}
    Update a customer:          {"id":2, "name": "Customer 1 updated", "active": 0}
    Create multiple customers:  [
            {"name": "Customer 1", "active": 1},
            {"name": "Customer 2", "active": 1}
        ]
DELETE https://revoxef.works/api/external/v2/customers/<customer_id>
```

> Response is a groups paginated array with the following fields:

```
    'id',
    'name',
    'active',
    'tax_id',
    'photo',
    'super_group_id',
    'printer_id',
    'printer_group_id',
```

## Others

## Get images 

To get the image, you need to do a get request the ideal thing for the images is to store it in your server and only request the image if the path changed.

Normal size:

`https://storage.googleapis.com/revo-cloud-bucket/xef/{account}/images/{image_path}`


Thumbnail size:

`https://storage.googleapis.com/revo-cloud-bucket/xef/{account}/images/resized_100_{image_path}`

Field       | Type      | Description
------------|-----------|---------------
account     | `string`  | Is your account name, same name used for login into the app
image_path  | `string`  | is the photo variable of the model

The image_path is the photo name (with the extension). You can get the photos in the [catalog](https://api.revo.works/#xef-catalog).