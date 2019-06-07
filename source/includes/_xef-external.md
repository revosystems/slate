# Xef External

## Prerequisistes

To be able to use the external api you need a `revo xef` account and a `access token`

1. Login into the desired account
2. Go to account management
3. Create a new [token](https://revoxef.works/account/tokens)


## Basic usage
The main URL for the external api is


`https://revoxek.works/api/external/v2/`

And you should provide the mandatory headers for the authentification


Header        | Value
--------------|----------
tenant        | {account-username}
Authorization | Bearer {the-token}


## Payment Methods

`GET PaymentMethods`

**Example call**

`GET https://revoxef.works/api/external/v2/paymentMethods`

**Response**

Response is an array of payment methods with the following fields:

```
"id": 1,
"name": "Payment method name",
"active": true,
```


## Orders

`GET Orders`

| Query parameter | Description                                                                 |
| ----------------|-----------------------------------------------------------------------------|
| from (optional) | The starting date of the orders fetched                                     |
| to (optional)   | The ending date of the orders fetched                                       |
| page (optional) | As the orders are paginated, use this parameter to select the page to fetch |


`GET https://revoxef.works/api/external/v2/orders?from=2017-01-02&to=2017-01-04`

Response

Response is paginated for every 50 


```
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
  
  You can add `?withPayments` parameter. This way apart from `orderContents` another array will be loaded called `orderInvoices`, it will contain an array of invoices and each invoice will contain an array of `orderPayments`. 
  
```   
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
  
  You can add **?withRooms** parameter. This way apart from `orderContents` another array will be loaded called `table` which will contain a `room`. 
  
```
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

## GiftCards

`POST GiftCards`

| Query parameter           | Description                                                                           |
| --------------------------|---------------------------------------------------------------------------------------|
| balance                   | Card amount                                                                           |
| uuid                      | Unique identifier for the card                                                        |
| campaign_id   (optional)  | Campaign ID when giftcard should be linked with a campaign (get ID from backend)      |


`GET https://revoxef.works/api/external/v2/giftCards`
``` 
{
    "balance": 1.25,
    "uuid": "123456"
    "campaign_id": "3"
}
```


## Responses

----|---------------------------------|------------------------------------------------
201 | HTTP_CREATED (201)              | giftcard could be created
422 | HTTP_UNPROCESSABLE_ENTITY (422) | giftcard already exists or could not be created