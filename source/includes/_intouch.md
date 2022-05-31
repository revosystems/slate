# InTouch

The Intouch Api is an api made for the Intouch App, an app made for customers to earn and spend loyalty points.

The Intouch Api there are two types of request.
The ones that use a Customer Token, and the ones that use 
an Api token authorization. 

#Api authorization type request
Revo-InTouch comes with an easy to use `REST` API Interface.
All requests will have the same structure.

With the autorization header when calling authenticated based resources

For this type of Authorization you need an Api token as seen in this example.


```sh
 POST/GET/PUT {username}.revointouch.works/api/v1/
```

```sh
Authorization: Bearer {Api token}
```

```sh
curl -X GET 
     --header "Authorization: Bearer 8J9lMscIvM88IZQjS3JA51ErsgcWO8KCGXsi7rJC0l1SrBuew5VCV94nLafQ"
     myaccount.revointouch.works/api/v1/stores
```
## Customers Points

`GET customers/{token}/points`

Returns the points of a customer + the points spent with the specified product.

Body Field  | Description
---------|---------
product    | **Json** id of the product in Json format.


```sh
{
    "data": {
        "leftPoints": 8000,
        "productPoints": []
    }
}
```

`POST customers/{token}/points`
Returns the left points of a customer + the points spent for a specified product.


Body Field  | Description
---------|---------
product    | **Json** id of the product in Json format.

```sh
{
    "data": {
        "spentPoints": 0,
        "leftPoints": 8000
    }
}
```
## Stores
`GET stores`
Returns a list of the stores from a specified user.

Body Field  | Description
---------|---------
username    | **json** username in json format.

```sh
{
    "data": [
        {
            "id": 1,
            "name": "An store"
        },
        {
            "id": 2,
            "name": "Test Store"
        }
    ]
}
```

## Store Status

`GET stores/{store}/status`
Returns the status of the specified store.

```sh
{
    "data": {
        "status": "paused"
    }
}
```

`POST stores/{store}/status`
Returns previous and current updated status after call.

Body Field  | Description
---------|---------
status    | **string** status to send.

```sh
// Response
{
    "data": {
        "previousStatus": "online",
        "currentStatus": "paused"
    }
}
```
#Customer Authorization type request

For this type of Authorization you need a Customer token .
The following requests until the end of the documentation are of this type.


```sh
 POST/GET/PUT {username}.revointouch.works/api/v1/customer
```

```sh
Authorization: Bearer {token}
```

```sh
curl -X GET 
     --header "Authorization: Bearer K2YBo90R2bil1mVKTkHh9cRlzstQYHIqYjX3oAYFT9HWluGcq9DaDh4LxVUI"
     myaccount.revointouch.works/api/v1/customer/cards 
```



## Images

`
https://a89f683ae563759322a9-3330e0d085d4ba4fd7f5395ee3f3cd7a.ssl.cf3.rackcdn.com/{xef-account}/images/{url}
`


## Login

`POST login`
Logs in with Email and specified password.

Parameters

Body Field  | Description
---------|---------
email    | **string** User email
password | **string** User password

```sh
// Response
{
  "data" : {
    "api_token" : "token",
    "info" : {
      "points" : 378,
      "id" : 1,
      "postalCode" : "08240",
      "email" : "jordi.p@revo.works",
      "name" : "Jordi Puigdellívol"
    }
  }
}
```


## Register

`POST register`
Registers user with the specified fields.
> Same response as login

Body Field   | Type       | Example
----------|------------|-----------------
name      | `string`   | Bruce
lastName  | `string`   | Wayne
password  | `string`   | batman
postalCode| `string`   | 08240
birthDate | `date`     | 1950-12-23
email     | `email`    | bruce@wayne.com
subscribe | `bool`     | 1

## UploadProfilePhoto
` POST uploadProfilePhoto`
Uploads specified profile photo to the current user.


Body Field   | Type       | Example
----------|------------|-----------------
Photo     | `photo`    | /9j/4AAQSkZJRgABA...



```sh
{
    "data": {
        "result": "OK",
        "photoName": "ovrA7mVcSQ.jpg"
    }
}
```



## Cards
` GET cards`
Returns all the cards data.

```sh
{
  "data" : [
    {
      "active" : 1,
      "amount" : 0,
      "id" : 8,
      "uuid" : "58b194cc08bfa",
      "photo" : "hi5bOD",
      "customer_id" : 1,
      "name" : "My gift card",
    },
    {
      "active" : 1,
      "amount" : 0,
      "id" : 7,
      "uuid" : "5876009347306",
      "photo" : "O6Magz",
      "customer_id" : 1,
      "name" : "My gift card",
    },
  ]
}
```

### `GET cards/{card_uuid}`
Gets the specified card's data by the card_uuid
```sh
{
  "data" : {
    "active" : 1,
    "amount" : 0,
    "id" : 8,
    "uuid" : "58b194cc08bfa",
    "photo" : "hi5bOD",
    "customer_id" : 1,
    "name" : "My gift card",
  }
}
```

### `PUT cards/{card_uuid}` 
To reload a gift card


Body Field   | Description
----------------|-------------------------------------
amount          | **int** amount in cents
payment_token   | **string** (optional) the stripe payment token. If null or not present it will use customer's default credit card


```sh
{
  "data" : {
    "newAmount" : 1000
  }
}
```


### `POST cards`
Creates a new gift card with the desired name.

Body Field  | Description
------|-------------
name  | **string** the name of the new card


```sh
{
  "data" : {
    "amount" : 0,
    "id" : 9,
    "uuid" : "58c2b978bb6ca",
    "photo" : "IWtdZ6",
    "name" : "My gift card",
    "customer_id" : 1
  }
}
```

## CardAssociate

### `POST cards/{card}/associate`

Associates the specified card by uuid to the current customer.

Body Field  | Description
------|-------------
uuid  | **string** Id of the card to associate


```sh
{
    "data": {
        "id": 3,
        "active": 1,
        "uuid": "4JQXA9J32SLR",
        "name": "cardName",
        "photo": null,
        "amount": 0,
        "customer_id": 1,
        "created_at": "2022-05-23 14:30:10",
        "updated_at": "2022-05-23 14:30:10"
    }
}
```

## CardTransfer

### `POST cards/{card}/transfer`
Transfers the amount to the specified card by uuid.

Body Field  | Description
------|-------------
uuid  | **string** Id of the card to transfer
amount| **string** Amount to transfer to the card


```sh
{
    "data": {
        "id": 3,
        "active": 1,
        "uuid": "4JQXA9J32SLR",
        "name": "cardName",
        "photo": null,
        "amount": 100,
        "customer_id": 1,
        "created_at": "2022-05-23 14:30:10",
        "updated_at": "2022-05-23 14:30:10"
    }
```





## AssociateCustomerGroup
### `POST associateCustomerGroup`

Associates a customer to a customerGroup.

Body Field  | Description
------|-------------
id  | **string** Id of the group to associate


```sh
{
    "data": {
        "id": 2,
        "name": "Revo",
        "discount": 10000,
        "created_at": "2021-06-14 15:54:53"
    }
}
```

## Syncable models
Doing a get request with a `from` parameter will return the objects to sync from them. Ideally you can store those objects into your system to be more responsive and sync when something changes

## Stores

`GET stores`

Returns all the stores data.

```sh
{
  "data" : {
    "new" : [
      {
        "id" : 1,
        "phone" : "669 68 65 61",
        "geo" : "41.7268582,1.8211535",
        "address" : "Arquitecte Oms 2",
        "token" : "ctzHxM48wBbRzZFM",
        "city" : "Manresa",
        "username" : "user1",
        "country" : "Spain",
        "name" : "An store"
      }
    ],
    "updated" : null,
    "deleted" : null
  }
}
```

`GET stores`

Returns all the stores from a user specified by username.

Body Field    | Type         | Description
---------|--------------|-----------
username    | `json`       | The username in json format

```sh
{
    "data": [
        {
            "id": 1,
            "name": "An store"
        },
        {
            "id": 2,
            "name": "Test Store"
        }
    ]
}
```



## Categories

`GET categories`

Returns all categories data.


```sh
  "data" : {
    "new" : [
      {
        "deleted_at" : null,
        "id" : 1,
        "name" : "Category 1",
      },
      {
        "deleted_at" : null,
        "id" : 2,
        "name" : "Category 2",
      }
    ],
    "updated" : null,
    "deleted" : null
  }
}
```

## Shifts


`GET shifts`

Returns all shifts.

```sh
{
  "data" : {
    "new" : [
      {
        "end" : "23:59:00",
        "start" : "00:00:00",
        "id" : 1,
        "daysOfWeek" : [
          1,
          2,
          3,
          4,
          5,
          6,
          0
        ],
      }
    ],
    "updated" : null,
    "deleted" : null
  }
}
```

## DeliveryShifts
These only should be used when `store.use_delivery_shifts` is true, if it is false, use the standard shifts for deliveries as well


`GET deliveryShifts`

Returns all delivery shifts

```sh
{
  "data" : {
    "new" : [
      {
        "end" : "23:59:00",
        "start" : "00:00:00",
        "id" : 1,
        "daysOfWeek" : [
          1,
          2,
          3,
          4,
          5,
          6,
          0
        ],
      }
    ],
    "updated" : null,
    "deleted" : null
  }
}
```

## Products

`GET products` Returns all products.


```sh
{
  "data" : {
    "new" : [
      {
        "points" : 150,
        "product_category_id" : 1,
        "id" : 1,
        "price" : 1240,
        "tax" : 1000,
        "pointsRequired" : null,
        "deleted_at" : null,
        "photo" : "cGs3lw7I6H.png",
        "name" : "Product 1"
      },
      {
        "points" : 250,
        "product_category_id" : 1,
        "id" : 2,
        "price" : 652,
        "tax" : 1000,
        "pointsRequired" : null,
        "deleted_at" : null,
        "photo" : "JmXM9ft8aV.png",
        "name" : "Product 2"
      },
      {
        "points" : 350,
        "product_category_id" : 2,
        "id" : 3,
        "price" : 1840,
        "tax" : 1000,
        "pointsRequired" : null,
        "deleted_at" : null,
        "photo" : "aaHwaf1Bql.png",
        "name" : "Product 3"
      },
    ],
    "updated" : null,
    "deleted" : null
  }
}
```

## Menu Categories
`GET menuCategories`
Returns all menuCategories

```sh
{
    {
        "id" : 1
        "name" : "Drinks",
        "product_id" : 1,
        "order" : 2
    },
    {
        "name" : "Take away",
        "product_id" : 2,
        "order" : 1
    }
    
}

```

## Menu Products

`GET menuProducts`

Returns all menuproducts


```sh
{
    {
        "product_id" : 1,
        "menu_category_id" : 1
        "order" : 2,
        "price" : 0,
    },
    {
        "product_id" : 15,
        "menu_category_id" : 1
        "order" : 1,
        "price" : 1500,
    }
    
}

```



## Modifiers

`GET modifiers`
Returns all modifiers.

```sh
{
"data" : {
    "new" : [
      {
        "id" : 1,
        "price" : 0,
        "modifier_category_id" : 1,
        "name" : "Small"
      },
      {
        "id" : 2,
        "price" : 1050,
        "modifier_category_id" : 1,
        "name" : "XL"
      },
    ],
    "updated" : null,
    "deleted" : null
  }
}
```

## Modifier Categories

`GET modifierCategories`

Returns all modifier categories.


```sh
{
  "data" : {
    "new" : [
      {
        "id" : 1,
        "deleted_at" : null,
        "type" : 1,
        "name" : "Size",
        "isMandatory" : 1
      },
      {
        "id" : 2,
        "type" : 2,
        "name" : "Toppings",
        "isMandatory" : 0
      }
    ],
    "updated" : null,
    "deleted" : null
  }
}
```

## Product - Modifier Categories

`GET productModifierCategories`

Returns all productModifierCategories


```sh
{
  "data" : {
    "new" : [
      {
        "product_id" : 1,
        "id" : 1,
        "deleted_at" : null,
        "modifier_category_id" : 1,
      },
      {
        "product_id" : 1,
        "id" : 2,
        "deleted_at" : null,
        "modifier_category_id" : 1,
      }
    ],
    "updated" : null,
    "deleted" : null
  }
}

```
## Orders

###Create an order

`POST orders`

Parameters:

Body Field   | Type         | Description
---------|--------------|-----------
order    | `json`       | the order json following the format below
contents | `array json` | the array of contents json following the format below
card     | `string`     | the customer gift card to use to do the charge
store    | `int`        | (optional) the store id to to create the order (it can be ignored if there is only one store)
delivery | `json`       | (optional) The delivery information it should contain `address` `city` `phone` `geolocation` and `time` fields

Contents can have optional parameters such as `modifiers` and `menuContents`

Body Field     | Type         | Description
-------------|--------------|-----------------------
modifiers    | `array json` | the array of modifiers
menuContents | `array json` | the array of menuContents


```sh
order={
  "subtotal" : 1673,
  "id" : null,
  "total" : 1840,
  "taxAmount" : 167
}
```

```sh
delivery={
    "address" : "Central perk, 12",
    "city" : "New York",
    "phone" : "555-555-555",
    "time" : "12:30",
    "geolocation" : "41.5500976,2.0964411"
}

```

```sh
contents = [{
  "product_id" : 1,
  "quantity" : 1,
  "id" : null,
  "subtotal" : 0,
  "price" : 1240,
  "tax" : 1000,
  "pointsSpent" : 150,
  "total" : 0,
  "taxAmount" : 0
},{
  "product_id" : 3,
  "quantity" : 1,
  "id" : null,
  "subtotal" : 1673,
  "price" : 1840,
  "tax" : 1000,
  "pointsSpent" : null,
  "total" : 1840,
  "taxAmount" : 167
  "modifiers" : [
    [
     "name" : "topping 1",
     "price": 500
    ],
    [
     "name" : "topping 2",
     "price": 0
    ],
  ]
},{
  "product_id" : 1,
  "quantity" : 1,
  "id" : null,
  "subtotal" : 0,
  "price" : 1240,
  "tax" : 1000,
  "pointsSpent" : 150,
  "total" : 0,
  "taxAmount" : 0,
  "menuContents" : [
      "product_id" : 4,
      "price" : 120
      "modifiers" : []
  ]
}]
```
### Get an order
`GET orders`

Returns the orders of the current customer.

```sh

{
  "data" : 
    [
          {
              "id": 7041,
              "store_id": 18,
              "customer_id": 1,
              "delivery_id": 744,
              "subtotal": 210,
              "taxAmount": 20,
              "total": 230,
              "pointsEarned": 2,
              "closed_at": "2022-02-24 10:00:02",
              "created_at": "2022-02-23 08:54:56",
              "status": 5,
              "pos_id": 819,
              "discountAmount": 0,
              "contact": null,
              "paid": 0,
              "delivery":
              } {
                  "id": 744,
                  "address": "Carrer del Bruc, 23",
                  "city": "Manresa",
                  "phone": "678984278",
                  "time": "2022-02-23 09:30:00",
                  "geolocation": "41.722687,1.817833",
                  "created_at": "2022-02-23 08:54:56",
                  "tableId": null,
                  "job_id": null,
                  "notes": null,
                  "delivery_zone_id": null
              },
              "contents": [
                  {
                      "id": 2318,
                      "order_id": 7041,
                      "product_id": 913,
                      "modifiers": null,
                      "pointsSpent": null,
                      "price": 230,
                      "subtotal": 210,
                      "taxAmount": 20,
                      "tax": 1000,
                      "quantity": 1,
                      "total": 230,
                      "created_at": "2022-02-23 08:54:56",
                      "notes": null,
                      "menu_contents": []
                  }
              ]
          },
    ]
  }
```

### Update an Order

`PUT orders/{id}`
Updates the order with the specified id.

```sh
{
    "data": []
}

```



###Get Last Order

`GET lastOrder`

Returns the last order for the customer.

```sh
{
  "data" : {
    "lastContents" : [
      {
        "id" : 16,
        "quantity" : 1,
        "tax" : 1000,
        "total" : 1240,
        "order_id" : 102,
        "subtotal" : 1128,
        "price" : 1240,
        "product_id" : 1,
        "pointsSpent" : null,
        "customer_id" : 1,
        "taxAmount" : 112
      },
      {
        "id" : 11,
        "quantity" : 1,
        "tax" : 1000,
        "total" : 1240,
        "order_id" : 98,
        "subtotal" : 1128,
        "price" : 1240,
        "product_id" : 1,
        "pointsSpent" : null,
        "customer_id" : 1,
        "taxAmount" : 112
      },
    ],
    "lastOrder" : {
      "closed_at" : "2017-01-09 11:33:27",
      "id" : 102,
      "subtotal" : 1128,
      "created_at" : "2017-01-05 11:47:15",
      "contents" : [
        {
          "id" : 16,
          "quantity" : 1,
          "tax" : 1000,
          "created_at" : "2017-01-05 11:47:15",
          "total" : 1240,
          "order_id" : 102,
          "subtotal" : 1128,
          "price" : 1240,
          "product_id" : 1,
          "pointsSpent" : null,
          "taxAmount" : 112
        }
      ],
      "pointsEarned" : 124,
      "total" : 1240,
      "customer_id" : 1,
      "taxAmount" : 112
    }
  }
}
```

## Points

`GET points`
Returns points for the current customer.

```sh
{
    "data": {
        "points": 58013
    }
}
```


## Credit Cards

`POST creditCards`

Creates a credit card with the stripe card token.

Parameters:

Body Field  | Description
---------|---------------
token    | The stripe card token


```sh
{
  "data" : {
    "address_line1_check" : null,
    "dynamic_last4" : null,
    "last4" : "4242",
    "address_line2" : null,
    "metadata" : [

    ],
    "address_city" : null,
    "address_zip_check" : null,
    "address_zip" : null,
    "country" : "US",
    "object" : "card",
    "address_line1" : null,
    "address_state" : null,
    "brand" : "Visa",
    "cvc_check" : "pass",
    "exp_month" : 4,
    "name" : null,
    "fingerprint" : "R92NDa6Z6u42Srgc",
    "funding" : "credit",
    "id" : "card_19vovPJVS7gfpCD2mGY750mu",
    "tokenization_method" : null,
    "customer" : "cus_AGLc0pme4PvRyf",
    "address_country" : null,
    "exp_year" : 2024
  }
}
```
### Get Credit Cards
`GET creditCards`

Returns credit card with the stripe card token.

Body Field  | Description
---------|---------------
token    | The stripe card token


```sh
{
  "data" : {
    "address_line1_check" : null,
    "dynamic_last4" : null,
    "last4" : "4242",
    "address_line2" : null,
    "metadata" : [

    ],
    "address_city" : null,
    "address_zip_check" : null,
    "address_zip" : null,
    "country" : "US",
    "object" : "card",
    "address_line1" : null,
    "address_state" : null,
    "brand" : "Visa",
    "cvc_check" : "pass",
    "exp_month" : 4,
    "name" : null,
    "fingerprint" : "R92NDa6Z6u42Srgc",
    "funding" : "credit",
    "id" : "card_19vovPJVS7gfpCD2mGY750mu",
    "tokenization_method" : null,
    "customer" : "cus_AGLc0pme4PvRyf",
    "address_country" : null,
    "exp_year" : 2024
  }
}
```



## TableAvailable

`GET table/{id}/available`

Returns if the table with the specified id is available.


```sh
{
    "data": {
        "available": true
    }
}

```

## StoreCard

`Post cards`

Creates a card with the specified name.

Field    | Description
---------|---------------
name    | Name to create the card

Returns the created card.


```sh
{
    "data": {
        "uuid": "WJZ9KQXSTQCV",
        "name": "cardName",
        "amount": 0,
        "photo": null,
        "updated_at": "2022-05-23 10:02:15",
        "created_at": "2022-05-23 10:02:15",
        "id": 2,
        "customer_id": 1
    }
}
```

## ProductStore

`GET productStore`

Returns productStore for current customer.


```sh
{
  "data" : {
    "new" : [
      {
        "id": 3,
        "product_id": 2152,
        "store_id": 7,
        "active": 1,
        "price": 70,
        "deleted_at": null,
        "created_at": "2021-01-20T14:50:41.000000Z",
        "updated_at": "2022-01-03T09:12:39.000000Z",
        "config": null
      },
      {
        "id": 5,
        "product_id": 914,
        "store_id": 5,
        "active": 0,
        "price": 110,
        "deleted_at": null,
        "created_at": "2021-02-10T10:43:57.000000Z",
        "updated_at": "2021-10-29T09:26:10.000000Z",
        "config": null
      }
    ]
  }
}

```
## SellingFormatStore

`GET sellingFormatStore`

Returns sellingFormatStore for current customer.

```sh
{
  "data" : {
    "new" : [
      {
        "id": 2,
        "selling_format_id": 182,
        "store_id": 5,
        "active": 1,
        "price": 200,
        "deleted_at": null,
        "created_at": "2021-04-09T08:30:47.000000Z",
        "updated_at": "2021-04-09T08:30:49.000000Z"
      },
      {
        "id": 3,
        "selling_format_id": 330,
        "store_id": 5,
        "active": 1,
        "price": 1000,
        "deleted_at": null,
        "created_at": "2021-04-12T15:41:53.000000Z",
        "updated_at": "2021-10-29T09:26:15.000000Z"
      }
    ]
  }
}

```

## Pullers

`GET pullers`


```sh
{
    "data": {
        "new": [],
        "updated": [],
        "deleted": []
    }
}

```

## PointShifts

`GET pointsShifts`

Returns PointShifts.

```sh
{
    "data": {
        "new": [],
        "updated": [],
        "deleted": []
    }
}

```

## Kiosk

`GET kiosk`

Returns kiosk Data.

```sh
{
    "data": {
        "new": [
            {
                "id": 1,
                "pin": null,
                "payment_modes": [
                    "1",
                    "5"
                ],
                "languages": null,
                "payment_gateway_type": 3,
                "payment_gateway_config": {
                    "adyen": {
                        "poiid": "S1F2-000158213300423"
                    }
                },
                "theme": null,
                "created_at": "2021-11-15 11:42:35",
                "name": "Kiosk",
                "printer_ip": "10.0.1.236",
                "store_id": 7,
                "printer_driver": 1,
                "config": "{\"stop_service_on_print_error\":\"0\"}"
            },
            {
                "id": 2,
                "pin": null,
                "payment_modes": [
                    "1",
                    "5"
                ],
                "languages": null,
                "payment_gateway_type": 3,
                "payment_gateway_config": {
                    "adyen": {
                        "poiid": "S1F2-000158213300423"
                    }
                },
                "theme": null,
                "created_at": "2022-03-04 12:20:47",
                "name": "KIOSK OSKAR",
                "printer_ip": "10.0.1.236",
                "store_id": 11,
                "printer_driver": 2,
                "config": "{\"stop_service_on_print_error\":\"0\"}"
            }
        ],
        "updated": [],
        "deleted": []
    }
}

```

## Theme

`GET theme`

Returns app theme.

```sh
{
    "data": {
        "theme": {
            "account": "revosupport",
            "appName": "Awesome App",
            "accent": "#d7771d",
            "accentInverse": "#fafafa",
            "onlyPayWithGiftCard": "no",
            "payment_method": 1,
            "payment_method_config": {
                "stripe": {
                    "key": "sk_test_Cme93TuYaEI8bieyEInbVoW1002vB0Oix4",
                    "publishable_key": null
                },
                "sipay": {
                    "key": null,
                    "resource": null,
                    "secret": null
                }
            },
            "stripe_publishable_key": null,
            "delivery_agency": 1,
            "delivery_agency_config": null,
            "rewards_enabled": "no",
            "online_ordering": "no",
            "has_slides": "no",
            "enable_barcode_scan": "no",
            "only_show_near_store": "no",
            "minimum_delivery_price": 100,
            "required_delivery_product_id": "",
            "disable_gift_card": "no",
            "points_to_money": "no",
            "rewards_order": "asc",
            "enable_invitations": "no",
            "tables": {
                "5": {
                    "3": "12   ",
                    "4": "13   ",
                },
                "6": {
                    "3": "12   ",
                    "4": "13   ",
                },
                "7": {
                    "3": "12   ",
                    "4": "13   ",
                },
                "9": {
                    "1": "Table 1",
                    "2": "Table 2",
                },
                "10": [],
                "11": {
                    "3": "12   ",
                    "4": "13   ",
                }
            }
        }
    }
}

```
{% comment %}
## Other routes

Verb     | Route
---------|------------   
GET      | /
PUT      | /
GET      | cards
GET      | cards/{uuid}
POST     | cards
PUT      | cards/{uuid}
POST     | cards/{card}/associate
POST     | cards/{card}/transfer
GET      | orders
POST     | orders
PUT      | orders/{id}
GET      | points
GET      | creditCards
POST     | creditCards
DELETE   | creditCards/{id}
PUT      | creditCards/{id}
POST     | invoices/{id}/points/obtain

{% endcomment %}
