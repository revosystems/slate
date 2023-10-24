# Xef (Deprecated)

Even this `api` is still working, it has been deprecated in favor of the new `REST` one
See [Xef Catalog](#xef-catalog) section one instead

## Basic Request

```sh
    {
        "auth":{
            "tenant"  :"account",
            "password":"password"
        },
        "action":"action",
        "data":data
    }
```

To work with `Revo external api` a `POST` or `GET` request is required to the following url:

`https://revoxef.works/apiExternal`

With a variable named `message` that has to be a `json` with the following information


    
Field    | Type     | Description
---------|----------|---------------
account  | `string` | is the account (same name used to login)
password | `string` | is the [app password](/back/account#password) (not the backend password)
action   | `string` | the action to perform (explained below)
data     | `json`   | data required for the action (explained below) 
    
> The api has a limit of 120 requests every minute so the best practice is to cache for x time the fetched information done with the `getXXX` actions.


## Get

Returns the full data for an specific model and id.

```sh
"data":{
        "model":"model",
        "id":"id"
        }
```
            
Field    | Description
---------|---------------
model    | is the model to fetch within `MenuGroup` `MenuCategory` `MenuItem` `ModifierGroup` `ModifierCategory` `Modifier`
id       | is the id of the model to fetch


```sh
https://revoxef.works/apiExternal?message=
    {
        "auth"      : {"tenant":"account","password":"password"},
        "action"    : "get",
        "data"      : {"model":"MenuItem","id":"236"}
    }
```
    

## getMenuGroups

```sh
https://revoxef.works/apiExternal?message=
    {
        "auth":{"tenant":"account","password":"password"},
        "action":"getMenuGroups",
        "data":{}
    }
```

Since maybe we want to create an online shop that starts with menu groups, we can get them all using the action `getMenuGroups` with empty data.
This will return a list of all the groups (with their `ID`). With this Id we can [fetch the categories](#getChilds).


## getChildren

```sh
"data":{
    "model":"parentModel",
    "id":id
    }
```

This action returns all the children for an specific model
        
Field       | Type     | Description
------------|----------|---------------
parentModel | `string` | is the model we want its children. `MenuGroup` `MenuCategories` `ModifierCategory`
id          | `int`    | the id of the model we want its children.
        

```sh
https://revoxef.works/apiExternal?message=
    {
        "auth":{"tenant":"account","password":"password"},
        "action":"getChilds",
        "data":{"model":"MenuGroup","id":2}
    }
```
        
It will return all the childs of the `menuGroup` with `ID` 2.
    

## getInventory

```sh
 https://revoxef.works/apiExternal?message=
     {
         "auth":{"tenant":"account","password":"password"},
         "action":"getInventory",
         "data":{"warehouse_id":1,"items":[2,4,10]}
     }
```

Gets the current inventory for an Item in a Warehouse.


Field        | Type        | Description
-------------|-------------|---------------
warehouse_id | `int`       | Is the ID of the warehouse to get the quantity from
items        | `array int` | Array of the ids of the items to get the quantity


 
### decreaseInventory

Decreases the inventory for an Item in a Warehouse.
 
```sh
 https://revoxef.works/apiExternal?message=
     {
         "auth":{"tenant":"account","password":"password"},
         "action":"decreaseInventory",
         "data":{"warehouse_id":1,"item_id":2, "quantity":1}
     }
```
 

Field        | Type  | Description
-------------|-------|---------------
warehouse_id | `int` | Is the ID of the warehouse to decrease the quantity from
item_id      | `int` | Is the ID of the item to decrease
quantity     | `int` | Is the quantity to decrease


## Revo Display Images

Returns an array of paths for each [Revo Display](/display) ads image.

To get the image

`https://a89f683ae563759322a9-3330e0d085d4ba4fd7f5395ee3f3cd7a.ssl.cf3.rackcdn.com/{account}/banners/{image_path}`

Field       | Type      | Description
------------|-----------|---------------
account     | `string`  | the account name
image_path  | `string`  | the image path returned in the array

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