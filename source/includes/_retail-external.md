# Retail

## Prerequisistes

To be able to use the external api you need a `REVO Retail`.

1. Login into the desired account
2. Go to account management
3. Create a new [token](http://revoretail.works/admin/account/tokens)


## Authorization

The main URL for the external API:

`https://revoretail.works/api/external`

The URL for the integrations API environment:

`https://integrations.revoretail.works/api/external`

And you should provide the mandatory headers for the authentication


Header        | Value
--------------|----------
username      | {account-username}
Authorization | Bearer {token}
Content-Type  | application/json


## Generic list response

```sh
{
    "current_page": 1,
    "data": [  [Response data will go here]  ],
    "first_page_url": "https://revoretail.works/api/external/{resource}?page=1",
    "from": 1,
    "last_page": 2,
    "last_page_url": "https://revoretail.works/api/external/{resource}?page=2",
    "next_page_url": "https://revoretail.works/api/external/{resource}?page=2",
    "path": "https://revoretail.works/api/external/{resource}",
    "per_page": 50,
    "prev_page_url": null,
    "to": 50,
    "total": 75
}
```


## Available resources

### Variants
RevoRetail uses variants to store product sizes, colors, etc..

It has 3 objects kinds to represent them

** 1. Variant Set **
Variant kind, for example, size or color.

`GET https://revoretail.works/api/external/catalog/variantSets`

> GET variantSets

```sh
[
    {"id": 1, "name": "Color"},
    {"id": 1, "name": "Size"}
]
```

** 2. Variant **
`Variants` are every variantSet variant, so blue, red, yellow,… for colors or L, XL,… for sizes. 

`GET https://revoretail.works/api/external/catalog/variants`

> GET variants

```sh
[
   {
        "id": 1,
        "name": "Red",
        "order": 2,
        "variant_set_id": 1,
        "color": null
    },
    {
        "id": 2,
        "name": "Blue",
        "order": 1,
        "variant_set_id": 1,
        "color": null
    },
    {
        "id": 3,
        "name": "L",
        "order": 1,
        "variant_set_id": 2,
        "color": null
    },
    {
        "id": 4,
        "name": "XL",
        "order": 1,
        "variant_set_id": 2,
        "color": null
    }
]
```

** 3. Product Variants **
Then we have product variants that are variants linked to products.

`GET https://revoretail.works/api/external/catalog/productsVariants`

> GET productsVariants

```sh
[
    {
        "id": 1,
        "product_id": 23,
        "variant_1_id": 1,
        "variant_2_id": 3
    },
    {
        "id": 2,
        "product_id": 23,
        "variant_1_id": 2,
        "variant_2_id": 3
    },
    {
        "id": 3,
        "product_id": 25,
        "variant_1_id": 2,
        "variant_2_id": 4
    },
    {
        "id": 4,
        "product_id": 25,
        "variant_1_id": 1,
        "variant_2_id": 4
    }
]
```

### Get list of taxes
Get a list of available taxes

`GET https://revoretail.works/api/external/config/taxes`

> GET taxes

```sh
[
    {
        "id": 1,
        "name": "10%",
        "taxPercentage": 10.00
    },
    {
        "id": 2,
        "name": "21%",
        "taxPercentage": 21.00
    }
]
```

### Get stocks list
Get an stock list.


`GET https://revoretail.works/api/external/catalog/stocks`

> GET stocks

```sh
[
    {
        "id": 1,
        "item_id": "1",
        "warehouse_id": "1",
        "quantity": 10.00
    },
    {
        "id": 2,
        "item_id": "1",
        "warehouse_id": "2",
        "quantity": 20.00
    },
    {
        "id": 3,
        "item_id": "2",
        "warehouse_id": "2",
        "quantity": 15.00
    }
]
```


### Generate stock movement.
We can generate stock movements with the following POST (required parameters: warehouse id, product id and quantity to move)


`POST https://revoretail.works/api/external/stocks/add`

> POST stocks/add

```sh
{
    "product_id": 1,
    "warehouse_id": 1,
    "quantity": 22.5
}
```

The answer is a 200 JSON with the current warehouse stock  `quantity`.


## Create Order
Store an order.


`POST https://revoretail.works/api/external/orders`

> POST orders 

```sh
{
    "notes": "",
    "shippingAmount": 0,
    "sum": 71.5,
    "subtotal": 59.09,
    "discount": 17.5,
    "orderDiscount": 6.5,
    "tax": 1,
    "total": 65,
    "discountObject": {
        "amount": 1.1,
        "id": null,
        "isPercentage": true,
        "name": "Without iva"
    },
    "customer_id": null,
    "shipping_id": null,
    "employee_id": 1,
    "closed_at": "2017-12-31 13:00:00",
    "order_contents": [
        {
            "name": "Dark shoes XL", 
            "quantity": 2, 
            "weight": null,
            "price": 27,
            "sum": 54,
            "discount": 11,
            "subtotal": 40,
            "tax": 4,
            "total": 44,
            "taxPercentage": 10,
            "discountObject": {
                "amount": 1.1,
                "id": null,
                "isPercentage": false,
                "name": "2nd unit 50%"
            },
            "notes": "",
            "margin": 0,
            "product_id": 1
        }, {
            "name": "Red shoes M", 
            "quantity": 1, 
            "weight": null,
            "price": 27.5,
            "sum": 27.5,
            "discount": 0,
            "subtotal": 25,
            "tax": 2.75,
            "total": 27.5,
            "taxPercentage": 10,
            "discountObject": {},
            "notes": "",
            "margin": 0,
            "product_id": 2
        }
    ]
}
```

This will return an order. As relevant fields we do get the order id which will be used to make payments and create an invoice.

`{"id": 1, "status": 0, ...}`


### Create an order payment
Create order payments with its invoice.

`POST https://revoretail.works/api/external/orders/{order_id}/invoices`

> POST orders/{order_id}/invoices 

```sh
{
    "payments": [
    {
        "amount": 65,
        "change": 0,
        "extra": 0,
        "payment_method_id": 1
    }
    ]
}
```

This will return an order invoice. As new relevant fields we can get the id.

`{"id": 1, "number": "E-1", ...}`



### Get list of Payments Methods
Get a list of available payment methods (card, cash, others...)


`GET https://revoretail.works/api/external/config/payment_methods`

> GET payment_methods

```sh
[
    {
        "id": 1,
        "name": "Card",
    },
    {
        "id": 2,
        "name": "Cash",
    }
]
```

### Customers

#### GET Customers

Get a list of paginated customers

`GET https://revoretail.works/api/external/config/customers`

> GET customers

```sh
Response:
{
    "current_page": 1,
    "data": [
        {
            "id": 1,
            "name": "Name",
            "address": "Address",
            "city": "City",
            "state": "State",
            "country": "ES",
            "postalCode": "00008",
            "nif": "12345678A",
            "web": null,
            "email": "test@test.test",
            "phone": "123456789",
            "notes": null,
            "maxCredit": "0.00",
            "extra_id": null,
        }
    ],
    "first_page_url": "https://revoretail.works/api/external/config/customers?page=1",
    "from": 1,
    "last_page": 1,
    "last_page_url": "https://revoretail.works/api/external/config/customers?page=1",
    "next_page_url": null,
    "path": "https://revoretail.works/api/external/config/customers",
    "per_page": 50,
    "prev_page_url": null,
    "to": 1,
    "total": 1
}
```

#### POST Customers

Create a customer

`POST https://revoretail.works/api/external/config/customers`

> POST customers

```sh
{
    "name": "Name", // required
    "address": "Address", // required
    "city": "City",
    "state": "State",
    "country": "ES",
    "postalCode": "00008",
    "nif": "12345678A",
    "web": null,
    "email": "test@test.test",
    "phone": "123456789",
    "notes": null,
    "maxCredit": "0.00",
    "extra_id": null,
}
```

> Error for required params:

```sh
{
    "error": "The given data was invalid."
    "code": 0,
    "data": []
}
```


#### PATCH Customers

Update a customer

`POST https://revoretail.works/api/external/config/customers/{customer_id}`

> PATCH customers/{customer_id}

```sh
{
    "name": "Name Updated"
}
```

### Sync chains

`Sync account chains data`

```sh
POST https://revoretail.works/api/external/chains/sync
```

> Response

```sh
{
    "data": {"message": Synced},
}
```
