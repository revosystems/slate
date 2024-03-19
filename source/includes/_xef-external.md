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

> The API has a data size límit of 2MB for each request.

> The API has a limit of 120 requests every minute so the best practice is to cache for x time the fetched information done with the `getXXX` actions.

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
    Data must be sent as a array.
    Create a customer:          [{"name": "Customer 1", "active": 1}]
        "name" is required
    Update a customer:          [{"id": 2, "name": "Customer 1 updated", "active": 0}]
        "id" and "name" are required
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