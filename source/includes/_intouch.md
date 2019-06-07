# InTouch
## Basic request

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

Revo-InTouch comes with an easy to use `REST` API Interface.
All requests will have the same structure. 

With the autorization header when calling authenticated based resources



## Images

`
https://a89f683ae563759322a9-3330e0d085d4ba4fd7f5395ee3f3cd7a.ssl.cf3.rackcdn.com/{xef-account}/images/{url}
`


## Login

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

`POST login` 

Parameters

Parameter| Description
---------|---------
email    | **string** User email
password | **string** User password


## Register

`POST register`

> Same response as login

Field     | Type       | Example
----------|------------|-----------------
name      | `string`   | Bruce
lastName  | `string`   | Wayne
password  | `string`   | batman
postalCode| `string`   | 08240
birthDate | `date`     | 1950-12-23
email     | `email`    | bruce@wayne.com
subscribe | `bool`     | 1



## Cards
` GET cards` 

```sh
{
  // GET cards
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

```sh
// GET cards/{card_uuid}
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

### `PUT cards/{card_uuid}` To reload a gift card


Field           | Description
----------------|-------------------------------------
amount          | **int** amount in cents
payment_token   | **string** (optional) the stripe payment token. If null or not present it will use customer's default credit card


```sh
// `PUT cards/{card_uuid}` To reload a gift card
{
  "data" : {
    "newAmount" : 1000
  }
}

```


### `POST cards`

Field | Description
------|-------------
name  | **string** the name of the new card


```sh
// `POST cards`
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

## Syncable models
Doing a get request with a `from` parameter will return the objects to sync from them. Ideally you can store those objects into your system to be more responsive and sync when something changes



## Stores

`GET stores`

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



## Categories

`GET categories`


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

`GET products`
 

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

## Modifier Categories

`GET modifierCategories`
 

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

## Modifiers

`GET modifiers`
 

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

## Product - Modifier Cateogires

`GET productModifierCategories`
 

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


## Credit Cards

`POST creditCards`

Parameters:

Field    | Description
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

## Orders

### Last Order

`GET lastOrder`


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

## Create an order

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

`POST orders`

Parameters:

Field    | Type         | Description
---------|--------------|-----------
order    | `json`       | the order json following the format below
contents | `array json` | the array of contents json following the format below
card     | `string`     | the customer gift card to use to do the charge
store    | `int`        | (optional) the store id to to create the order (it can be ignored if there is only one store)
delivery | `json`       | (optional) The delivery information it should contain `address` `city` `phone` `geolocation` and `time` fields

Contents can have optional parameters such as `modifiers` and `menuContents`

Field        | Type         | Description
-------------|--------------|-----------------------
modifiers    | `array json` | the array of modifiers
menuContents | `array json` | the array of menuContents




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