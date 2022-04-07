# Retail

## Prerequisistes

To be able to use the external api you need a `revo retail` account and an `access token`

1. Login into the desired account
2. Go to account management
3. Create a new [token](http://revoretail.works/admin/account/tokens)


## Authorization
The main URL for the external api is


`https://revoretail.works/api/external`

And you should provide the mandatory headers for the authentication


Header          | Value
----------------|------
`username`      | {account-username}
`Authorization` | Bearer {the-token}


## Generic list response

```sh
{
    "current_page"  : 1,
    "from"          : 1,
    "last_page"     : 2,
    "next_page_url" : "https://revoretail.works/api/external/[resource]?page=2",
    "path"          : "http://revoretail.works/api/external/[resource]",
    "per_page"      : 50,
    "prev_page_url" : null,
    "to"            : 50,
    "total"         : 10,
    "data": []
}
```


## Available resources

### Get list of groups
Get a list of groups


`GET https://revoretail.works/api/external/catalog/groups`

```sh
[
    {
        "id": 1,
        "name", "Shoes",
        "photo"     : "",
        "order"     : 0,
        "active"    : 1,
        "group_id"  : 1,
        "tax_id"    : 1
    },
    {
        "id": 2,
        "name", "Pants",
        "photo"     : "",
        "order"     : 0,
        "active"    : 1,
        "group_id"  : 1,
        "tax_id"    : 1
    }
]
```

### Get list of categories
Get a list of categories, can filter by group


`GET https://revoretail.works/api/external/catalog/categories?group=`

```sh
[
    {
        "id": 1,
        "name", "Trousers",
        "photo"     : "",
        "order"     : 0,
        "active"    : 1,
        "group_id"  : 1,
        "tax_id"    : 1
    },
    {
        "id": 2,
        "name", "Shorts",
        "photo"     : "",
        "order"     : 0,
        "active"    : 1,
        "group_id"  : 1,
        "tax_id"    : 1
    }
]
```


### Get list of products
Get a list of products, can filter by category


`GET https://revoretail.works/api/external/catalog/products?category=`

```sh
[
    {
        "id": 1,
        "name", "new nike xl"
        "reference"             : "",
        "info"                  : "",
        "shortInfo"             : "",
        "brand"                 : "",
        "season"                : "",
        "photo"                 : "",
        "order"                 : "0",
        "active"                : "1",
        "featured"              : "0",
        "isOpen"                : "0",
        "type"                  : "0",
        "price"                 : "",
        "costPrice"             : "",
        "barcode"               : "",
        "category_id"           : 1,
        "tax_id"                : 1,
        "main_product_id"       : null,
        "usesStockManagement"   : false,
        "usesWeight"            : false,
        "unit_id"               : "1"
    }, {
        "id": 2,
        "name", "new nike m"
        "reference"             : "",
        "info"                  : "",
        "shortInfo"             : "",
        "brand"                 : "",
        "season"                : "",
        "photo"                 : "",
        "order"                 : "0",
        "active"                : "1",
        "featured"              : "0",
        "isOpen"                : "0",
        "type"                  : "0",
        "price"                 : "",
        "costPrice"             : "",
        "barcode"               : "",
        "category_id"           : 1,
        "tax_id"                : 1,
        "main_product_id"       : null,
        "usesStockManagement"   : false,
        "usesWeight"            : false,
        "unit_id"               : "1"    }
]
```

If we add `withStocks` parameter we'll get an stocks list within each product.
For example:

```sh
[
    {
        "id": 1,
        "name", "new nike xl",
        ...
        "stocks": [{
            "id": 1,"warehouse_id": 1, "quantity": 10
        },{
            "id": 2,"warehouse_id": 3, "quantity": 15
        }]
    }
]

```


### Variants
RevoRetail uses variants to store product sizes, colors, etc..

It has 3 objects kinds to represent them

** 1. Variant Set **
Variant kind, for example, size or color.

`GET https://revoretail.works/api/external/catalog/variantSets`

```sh
[
    {"id": 1, "name": "Color"},
    {"id": 1, "name": "Size"}
]
```

** 2. Variant **
`Variants` are every variantSet variant, so blue, red, yellow,… for colors or L, XL,… for sizes. 

`GET https://revoretail.works/api/external/catalog/variants`

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

## Catalog generation
In order to generate a catalog via API we can create groups, group's categories and category's products.
in any of these cases we can send a dictionary with the desired object to create (group, category or product) or we can send a list of dictionaries to create multiple objects at the same time. e.g.:

`POST https://revoretail.works/api/external/catalog/groups`

```sh
// BODY Dictionary:
{
    "name": "Group 1",
    "active": 1,        // Optional, defaults to 1
    "tax_id": 1         // Optional, defaults to null
}
```

```sh
// BODY List of dictionaries:
[
    {
        "name": "Group 1",
        "active": 1,        // Optional, defaults to 1
        "tax_id": 1         // Optional, defaults to null
    }, {
        "name": "Group 2"
        "active": 1,        // Optional, defaults to 1
        "tax_id": 1         // Optional, defaults to null
    }
]
```

In any case its responses are a 200 JSON with 2 fields (success and errors): 

```sh
{
    "success": 1,                   // Amount of created objects
    "errors": [{"name": "Group 1"}] // List of object that couldn't be created
}
```

### Create a category

`POST https://revoretail.works/api/external/catalog/categories`

```sh
{
    "name": "Category 1",
    "group_id": 1,
    "active": 1,            // Optional, defaults to 1
    "tax_id": 1             // Optional, defaults to null
}
```

### Create a product

`POST https://revoretail.works/api/external/catalog/products`

```sh
{
    "name": "Product 1",
    "category_id": 1,
    "id":                   // Optional, defaults autoincrement
    "active": 1,            // Optional, defaults to 1
    "tax_id": 1,            // Optional, defaults to null
    "reference": "",        // Optional
    "info": "",             // Optional
    "shortInfo": "",        // Optional
    "brand": "",            // Optional
    "season": "",           // Optional
    "order": "",            // Optional
    "featured": "",         // Optional
    "isOpen": "",           // Optional
    "price": "",            // Optional
    "costPrice": "",        // Optional
    "barcode": "",          // Optional
    "main_product_id": "",  // Optional
    "usesStockManagement": "",  // Optional
    "usesWeight": "",       // Optional
    "unit_id": 1            // Optional
}
```
### Update a product

`PUT https://revoretail.works/api/external/catalog/products/{item_id}`

```sh
{
    "name": "Product 1",
    "price": 15,
    "info": "",
    "id":                   // Optional, defaults autoincrement
    "active": 1,            // Optional, defaults to 1
    "tax_id": 1,            // Optional, defaults to null
    "reference": "",        // Optional
    "shortInfo": "",        // Optional
    "brand": "",            // Optional
    "season": "",           // Optional
    "order": "",            // Optional
    "featured": "",         // Optional
    "isOpen": "",           // Optional
    "costPrice": "",        // Optional
    "barcode": "",          // Optional
    "main_product_id": "",  // Optional
    "usesStockManagement": "",  // Optional
    "usesWeight": "",       // Optional
    "unit_id": 1            // Optional
}
```
### Get stocks list
Get an stock list.


`GET https://revoretail.works/api/external/catalog/stocks`

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

## Sync chains

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
