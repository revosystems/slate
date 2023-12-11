# Retail Catalog
Inside catalog there are many resources: groups, categories and resources.

> The api has a limit of 40 requests every minute so the best practice is to cache for x time the fetched information done with the `getXXX` actions.

## Prerequisistes

To be able to use the external api you need a `REVO Retail`.

1. Login into the desired account
2. Go to account management
3. Create a new [token](http://revoretail.works/admin/account/tokens)


## Authorization

The main URL for the external API:

`https://revoretail.works/api/external/{resource}`

The URL for the integrations API environment:

`https://integrations.revoretail.works/api/external/{resource}`

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

### URL parameters

You can add the next URL parameters for the paginated responses:

| Key          | Type   | Required | Description                                                               |
|--------------|--------|----------|---------------------------------------------------------------------------|
| `page`       | number | optional | As the data is paginated, use this parameter to select the page to fetch. |
| `pagination` | number | optional | Number of objects per page. The default value is **50** and the max allowed is **200**. |

At the bottom of the request, it is specified information about the current page and pagination.

## Catalog structure

The main catalog structure is the following:

*Groups* => *Categories* => *Items*

*Items* cannot exist without *Categories*, and *Categories* cannot exists without *Groups*.

## Groups

Can list, show, create, update and delete Groups.

GET `https://revoretail.works/api/external/catalog/groups`

Response is a groups paginated array with the following fields:

| Field            | Type    | Required | Extra info                |
|------------------|---------|----------|---------------------------|
| id               | number  | *Only read* | autoincrement          |
| name             | string  | **required** |                       |
| photo            | string  | optional |                           |
| order            | number  | optional |                           |
| active           | boolean | optional | default: 1                |
| tax_id           | number  | optional |                           |
| tax              | object  | *Only read* |                        |

**tax object (optional):**

| Field            | Type    |
|------------------|---------|
| id               | number  |
| name             | string  |
| percentage       | string  |
| shouldStack      | boolean |

GET `https://revoretail.works/api/external/catalog/groups/<group_id>`

POST `https://revoretail.works/api/external/catalog/groups`

> Create groups (POST) `catalog/groups`

```sh
[
    {
        "name": "Test Group 1",
        ...
    },
    {
        "name": "Test Group 2",
        ...
    },
    ...
]
```

> Update a group (POST) `catalog/groups/<group_id>`

```sh
{
    "name": "Super Test Group 1",
    ...
}
```

DELETE `https://revoretail.works/api/external/catalog/groups/<group_id>`

## Categories

Can list, show, create, update and delete Categories.

GET `https://revoretail.works/api/external/catalog/categories`

Response is a groups paginated array with the following fields:

| Field            | Type    | Required | Extra info                |
|------------------|---------|----------|---------------------------|
| id               | number  | *Only read* | autoincrement          |
| name             | string  | **required** |                       |
| group_id         | number  | **required** |                       |
| photo            | string  | optional |                           |
| order            | number  | optional |                           |
| active           | boolean | optional | default: 1                |
| extra_id         | string  | optional |                           |
| tax_id           | number  | optional |                           |
| tax              | object  | *Only read* |                        |

**tax object (optional):**

| Field            | Type    |
|------------------|---------|
| id               | number  |
| name             | string  |
| percentage       | string  |
| shouldStack      | boolean |

GET `https://revoretail.works/api/external/catalog/categories/<category_id>`

POST `https://revoretail.works/api/external/catalog/categories`

> Create categories (POST) `catalog/categories`

```sh
[
    {
        "name": "Test Category 1",
        "group_id": 1,
        ...
    },
    {
        "name": "Test Category 2",
        "group_id": 1,
        ...
    },
    ...
]
```

> Update a category (POST) `catalog/category/<category_id>`

```sh
{
    "name": "Super Test Category 1",
    ...
}
```

DELETE `https://revoretail.works/api/external/catalog/categories/<category_id>`

## Products

Can list, show, create, update and delete Products.

GET `https://revoretail.works/api/external/catalog/products`

Response is a groups paginated array with the following fields:

| Field            | Type    | Required | Extra info                |
|------------------|---------|----------|---------------------------|
| id               | number  | *Only read* | autoincrement          |
| name             | string  | **required** |                       |
| category_id      | number  | **required** |                       |
| photo            | string  | optional |                           |
| order            | number  | optional |                           |
| active           | boolean | optional | default: 1                |
| extra_id         | string  | optional |                           |
| reference        | string  | optional |                           |
| info             | string  | optional |                           |
| shortInfo        | string  | optional |                           |
| brand            | string  | optional |                           |
| season           | string  | optional |                           |
| featured         | boolean | optional | default: 0                |
| isOpen           | boolean | optional | default: 0                |
| weight           | number  | optional | default: 0                |
| type             | number  | optional | 0 = NORMAL (default), 1 = KIT, 2 = TICKET, 3 = CONTAINER, 4 = GIFT_CARD, 5 = VARIANT_MASTER, 6 = VARIANT, 7 = MANAGEMENT_ONLY, 8 = VOUCHER |
| price            | decimal  | optional | default: 0.00            |
| costPrice        | decimal  | optional | default: 0.00            |
| barcode          | string   | optional | default: 0.00            |
| main_product_id  | number   | optional |                          |
| complementary_product_id | number   | optional |                  |
| discountinued_at | date   | optional | YYYY-MM-DD                 |
| traceability     | boolean   | optional | default: 0              |
| usesStockManagement | boolean   | optional | default: 0           |
| usesWeight       | boolean   | optional | default: 0              |
| unit_id          | number  | optional |                           |
| tax_id           | number  | optional |                           |
| tax              | object  | *Only read* |                        |
| variant_master   | object  | *Only read* | Same product structure (optional) |
| ecommerce_info   | object  | *Only read* | Showed with filter *withECommerce*. |
| stocks   | array (objects) | *Only read* | Showed with filter *withStocks* or ECommerce module enabled + filter *withECommerceStocks*. |
| variant_info   | object  | *Only read* | Showed with filter *withVariantInfo* or ECommerce module enabled + filter *withECommerceStocks*.. |

**tax object (optional):**

| Field            | Type    |
|------------------|---------|
| id               | number  |
| name             | string  |
| percentage       | string  |
| shouldStack      | boolean |

**ecommerce_info object (optional):**

| Field            | Type    |
|------------------|---------|
| id               | number  |
| product_id       | number  |
| active           | boolean  |
| weight           | number |
| sizes            | object (width, height and depth) |
| price            | string |
| discount_amount  | string |
| discount_percentage | string |

**stocks array (optional):**

| Field            | Type    |
|------------------|---------|
| id               | number  |
| quantity         | string  |
| alert            | boolean  |
| warehouse_id     | number |
| item_id          | number |
| unit_id          | number |

**variant_info object (optional):**

| Field            | Type    |
|------------------|---------|
| id               | number  |
| product_id       | number  |
| variant_1_id     | number  |
| variant_2_id     | number  |

GET `https://revoretail.works/api/external/catalog/products/<product_id>`

POST `https://revoretail.works/api/external/catalog/products`

> Create products (POST) `catalog/products`

```sh
[
    {
        "name": "Test Category 1",
        "category_id": 1,
        ...
    },
    {
        "name": "Test Category 2",
        "category_id": 1,
        ...
    },
    ...
]
```

> Update a product (POST) `catalog/products/<product_id>`

```sh
{
    "name": "Super Test Product 1",
    ...
}
```

DELETE `https://revoretail.works/api/external/catalog/products/<product_id>`