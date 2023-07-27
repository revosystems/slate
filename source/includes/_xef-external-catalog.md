# Xef Catalog
Inside catalog there are many resources; items, categories, groups, modifiers and sellingFormats.

To explore those resources as a paginated list we can use a GET request with the following pattern:

- `GET https://revoxef.works/api/external/v2/catalog/{resource}` replacing the desired resource on this url. 

## Prerequisistes
To be able to use the external api you need a `revo xef` account and a `access token`

1. Login into the desired account
2. Go to account management
3. Create a new [token](https://revoxef.works/account/tokens)


## Basic usage
The main URL for the external API:

`https://revoxef.works/api/external/v2/catalog`

The URL for the integrations API environment:

`https://integrations.revoxef.works/api/external/v2/catalog`

And you should provide the mandatory headers for the authentication


Header        | Value
--------------|----------
tenant        | {account-username}
Authorization | Bearer {the-token}
client-token  | {client-token}
Content-Type  | application/json

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

Can list, show, create, update and delete Groups

GET `https://revoxef.works/api/external/v2/catalog/groups`

Response is a groups paginated array with the following fields:

| Field            | Type    | Required | Extra info                |
|------------------|---------|----------|---------------------------|
| id               | number  | -        | autoincrement             |
| name             | string  | **required** |                           |
| photo            | string  | optional |                           |
| active           | boolean | optional | default: 1                |
| tax_id           | number  | optional |                           |
| printer_id       | number  | optional |                           |
| printer_group_id | number  | optional |                           |
| super_group_id   | number  | optional |                           |
| extra_id         | string  | optional |                           |


GET `https://revoxef.works/api/external/v2/catalog/groups/<group_id>`

POST `https://revoxef.works/api/external/v2/catalog/groups`

> Create a group (POST) `catalog/groups`

> All parameters are optional.

```sh
{
    "name": "Group 1",
    "active": 1
    ...
}
```

> Update a group (POST) `catalog/groups`

```sh
{
    "id": 2, // required to update
    "name": "Group 1 updated",
    "active": 0
    ...
}
```

> Create/update múltiple groups (POST) `catalog/groups`

```sh
{
    "groups": [
        {
            "id": 1, // only required to update
            "name": "Group 1",
            ...
        },
        {
            "id": 2, // only required to update
            "name": "Group 2",
            ...
        }
        ...
    ]
}
```

DELETE `https://revoxef.works/api/external/v2/catalog/groups/<group_id>`

## Categories

Can list, show, create, update and delete Categories

GET `https://revoxef.works/api/external/v2/catalog/categories` or 

GET `https://revoxef.works/api/external/v2/catalog/groups/<group_id>/categories`

Response for GET categories is a categories paginated array with the following fields:

| Field                | Type    | Required | Extra info    |
|----------------------|---------|----------|---------------|
| id                   | number  | -        | autoincrement |
| name                 | string  | **required** |               |
| photo                | string  | optional |               |
| active               | boolean | optional | default: 1    |
| group_id             | number  | **required** |               |
| tax_id               | number  | optional |               |
| printer_id           | number  | optional |               |
| printer_group_id     | number  | optional |               |
| modifier_category_id | number  | optional |               |
| modifier_group_id    | number  | optional |               |
| super_group_id       | number  | optional |               |
| dish_order_id        | number  | optional |               |
| extra_id             | string  | optional |               |

GET `https://revoxef.works/api/external/v2/catalog/categories/<category_id>`

POST `https://revoxef.works/api/external/v2/catalog/categories`

> Create a category (POST) `catalog/categories`

```sh
{
    "name": "Category 1",
    "active": 1,
    "group_id": 1
    ...
}
```

> Update a category  (POST) `catalog/categories`

```sh
{
    "id": 1, // required to update
    "name": "Category 1 updated",
    "active": 0
    ...
}
```

> Create/update múltiple categories (POST) `catalog/categories`

```sh
{
    "categories": [
        {
            "id": 1, // only required to update
            "name": "Category 1",
            ...
        },
        {
            "id": 2, // only required to update
            "name": "Category 2",
            ...
        }
        ...
    ]
}
```

DELETE `https://revoxef.works/api/external/v2/catalog/categories/<category_id>`

## Items

For the *Items*, there are the following types:

- *Normal Items*: *item.type = 0*
- *Menu Items*: *item.type = 1*
- *Selling format Items*: *item.type = 4* 

Can list, show, create, update and delete *Items*

GET `https://revoxef.works/api/external/v2/catalog/items` or 

GET `https://revoxef.works/api/external/v2/catalog/categories/<category_id>/items`

Response for GET items is an items paginated array with the following fields:

| Field                  | Type    | Required | Extra info                               |
|------------------------|---------|----------|------------------------------------------|
| id                     | number  | -         | autoincrement                            |
| name                   | string  | **required** |                                          |
| price                  | decimal | **required** |                                          |
| photo                  | string  | optional |                                          |
| active                 | boolean | optional | default: 1                               |
| info                   | string  | optional |                                          |
| type                   | number  | **required** | normal = 0, menu = 1, selling format = 4 |
| hasInventory           | boolean | optional | default: 0                               |
| usesWeight             | boolean | optional | default: 0                               |
| category_id            | number  | **required** |                                          |
| tax_id                 | number  | optional |                                          |
| printer_id             | number  | optional |                                          |
| printer_group_id       | number  | optional |                                          |
| modifier_group_id      | number  | optional |                                          |
| modifier_category_id   | number  | optional |                                          |
| isMenu                 | boolean | optional | default: 0                               |
| shouldAppearInMenuList | boolean | optional | default: 0                               |
| super_group_id         | number  | optional |                                          |
| isOpen                 | boolean | optional | default: 0                               |
| nameKitchen            | string  | optional |                                          |
| costPrice              | decimal | **required** |                                          |
| displayInventory       | boolean | optional | default: 0                               |
| isLinked               | boolean | optional | default: 0                               |
| allergies              | string  | optional | [see Allergies](#allergies)              |
| dish_order_id          | number  | optional |                                          |
| barcode                | string  | optional |                                          |
| unit_id                | number  | optional |                                          |
| extra_id               | string  | optional |                                          |
| useAverageCostPrice    | boolean | optional | default: 0                               |
| cookDuration           | number  | optional |                                          |
| buttonName             | string  | optional |                                          |
| minQuantity            | number  | optional |                                          |
| report_category_id     | number  | optional |                                          |
| config                 | string  | optional |                                          |

### Allergies

There are the following allergies:

| Allergies   | Value |
|-------------|-------|
| eggs        | 1     |
| sesame      | 2     |
| lupin       | 3     |
| mustard     | 4     |
| fish        | 5     |
| celery      | 6     |
| crustaceans | 7     |
| peanuts     | 8     |
| mollusks    | 9     |
| soy         | 10    |
| nuts        | 11    |
| sulfites    | 12    |
| dairy       | 13    |

To save (POST) the allergies, you must send the values split by ';' `Example: "2;3;6;10"`

GET `https://revoxef.works/api/external/v2/catalog/items/<item_id>`

POST `https://revoxef.works/api/external/v2/catalog/items`

> Create a item (POST) `catalog/items`

```sh
{
    "name": "Product 1",
    "price": 8.50,
    "active": 1,
    "category_id": 1
    ...
}
```

> Update a item (POST) `catalog/items`

```sh
{
    "id": 1, // required to update
    "name": "Category 1 updated",
    "active": 0
    ...
}
```

> Create/update múltiple items (POST) `catalog/items`

```sh
{
    "items": [
        {
            "id": 1, // only required to update
            "name": "Item 1",
            ...
        },
        {
            "id": 2, // only required to update
            "name": "Item 2",
            ...
        }
        ...
    ]
}
```

DELETE `https://revoxef.works/api/external/v2/catalog/items/<item_id>`

## Selling Formats

Can list, show, create, update and delete Selling Formats

GET `https://revoxef.works/api/external/v2/catalog/sellingFormats`

Response for GET selling formats is a selling formats paginated array with the following fields:

| Field  | Type    | Required | Extra info    |
|--------|---------|----------|---------------|
| id     | number  | -         | autoincrement |
| name   | string  | **required** |               |
| photo  | string  | optional |               |
| order  | number  | optional |               |
| active | boolean | optional | default: 1    |

GET `https://revoxef.works/api/external/v2/catalog/sellingFormats/<format_id>`

POST `https://revoxef.works/api/external/v2/catalog/sellingFormats`

> Create a selling format (POST) `catalog/sellingFormats`

```sh
{
    "name": "Selling format 1",
    "active": 1
    ...
}
```

> Update a selling format (POST) `catalog/sellingFormats`

```sh
{
    "id": 1, // required to update
    "name": "Selling format 1 updated",
    "active": 0
    ...
}
```

> Create/update múltiple selling formats (POST) `catalog/sellingFormats`

```sh
{
    "sellingFormats": [
        {
            "id": 1, // only required to update
            "name": "Selling format 1",
            ...
        },
        {
            "id": 2, // only required to update
            "name": "Selling format 2",
            ...
        }
        ...
    ]
}
```

DELETE `https://revoxef.works/api/external/v2/catalog/sellingFormats/<format_id>`


### Selling Format Pivots

An *Item* can be of *Selling format* type.

This can be distinguished by *item.type* = 4.

The *Selling format item* can manage different *Selling formats*, with different prices and quantities, for its TPV selling.

Can list, show, create, update and delete Selling Format Pivots

GET `https://revoxef.works/api/external/v2/catalog/sellingFormatPivots`

Response for GET selling format pivots is a selling format pivots paginated array with the following fields:

| Field                | Type    | Required | Extra info     |
|----------------------|---------|----------|----------------|
| id                   | number  | -         | autoincrement  |
| quantity             | decimal | **required** |                |
| price                | decimal | **required** |                |
| order                | number  | optional |                |
| format_id            | number  | **required** | selling format |
| item_id              | number  | **required** | item.type = 4  |
| combination_group_id | number  | optional |                |
| unit_id              | number  | optional |                |

GET `https://revoxef.works/api/external/v2/catalog/sellingFormatPivots/<format_pivot_id>`

POST `https://revoxef.works/api/external/v2/catalog/sellingFormatPivots`

> Create a selling format pivot (POST) `catalog/sellingFormatPivots`

```sh
{
    "price": 5.00,
    "quantity": 1,
    "format_id": 1,
    "item_id": 1,
    ...
}
```

> Update a selling format pivot (POST) `catalog/sellingFormatPivots`

```sh
{
    "id": 1, // required to update
    "quantity": 2
    ...
}
```

> Create/update múltiple selling format pivots (POST) `catalog/sellingFormatPivots`

```sh
{
    "sellingFormatPivots": [
        {
            "id": 1, // only required to update
            "quantity": 1,
            "format_id": 1,
            "item_id": 1,
            ...
        },
        {
            "id": 2, // only required to update
            "quantity": 2,
            "format_id": 2,
            "item_id": 2,
            ...
        }
        ...
    ]
}
```

DELETE `https://revoxef.works/api/external/v2/catalog/sellingFormatPivots/<format_pivot_id>`

## Modifiers

The *Modifiers* structure is the following:

*Modifier groups* (<= *Modifier pivot* =>) *Modifier categories* => *Modifiers*.

*Modifiers* must exist in a *Modifier category*, and *Modifier categories* can exist independently or in a *Modifier group*.

Both *Modifier groups* and *Modifier categories* can be linked to catalog *Categories* and *Items*.

Can list, show, create, update and delete Modifiers

GET `https://revoxef.works/api/external/v2/catalog/modifiers`

Response for GET modifiers is a modifiers paginated array with the following fields:

| Field        | Type    | Required | Extra info        |
|--------------|---------|----------|-------------------|
| id           | number  | -         | autoincrement     |
| name         | string  | **required** |                   |
| price        | decimal | **required** |                   |
| photo        | string  | optional |                   |
| order        | number  | optional |                   |
| category_id  | number  | **required** | modifier category |
| cookDuration | number  | optional |                   |
| active       | boolean | optional | default: 1        |
| extra_id     | string  | optional |                   |

GET `https://revoxef.works/api/external/v2/catalog/modifiers/<modifier_id>`

POST `https://revoxef.works/api/external/v2/catalog/modifiers`

> Create a modifier (POST) `catalog/modifiers`

```sh
{
    "name": "Modifier 1",
    "price": 1.00,
    "category_id": 1,
    "active": 1
    ...
}
```

> Update a modifier (POST) `catalog/modifiers`

```sh
{
    "id": 1, // required to update
    "price": 2.00
    ...
}
```

> Create/update múltiple modifiers (POST) `catalog/modifiers`

```sh
{
    "modifiers": [
        {
            "id": 1, // only required to update
            "name": "Modifier 1",
            ...
        },
        {
            "id": 2, // only required to update
            "name": "Modifier 2",
            ...
        }
        ...
    ]
}
```

DELETE `https://revoxef.works/api/external/v2/catalog/modifiers/<modifier_id>`

### Modifier Categories

Can list, show, create, update and delete Modifier Categories

GET `https://revoxef.works/api/external/v2/catalog/modifierCategories`

Response for GET modifier categories is a modifier categories paginated array with the following fields:

| Field      | Type    | Required     | Extra info                                        |
|------------|---------|--------------|---------------------------------------------------|
| id         | number  | -             | autoincrement                                     |
| name       | string  | **required** |                                                   |
| order      | number  | optional     |                                                   |
| isChoice   | boolean | optional     | 0 for select one, 1 (default) for multiple choice |
| isOptional | boolean | optional     | default: 1                                        |
| active     | boolean | optional     | default: 1                                        |


GET `https://revoxef.works/api/external/v2/catalog/modifierCategories/<modifier_category_id>`

POST `https://revoxef.works/api/external/v2/catalog/modifierCategories`

> Create a modifier category (POST) `catalog/modifierCategories`

```sh
{
    "name": "Modifier category 1"
    ...
}
```

> Update a modifier category (POST) `catalog/modifierCategories`

```sh
{
    "id": 1, // required to update
    "isChoice": 0
    ...
}
```

> Create/update múltiple modifier categories (POST) `catalog/modifierCategories`

```sh
{
    "modifierCategories": [
        {
            "id": 1, // only required to update
            "name": "Modifier category 1",
            ...
        },
        {
            "id": 2, // only required to update
            "name": "Modifier category 2",
            ...
        }
        ...
    ]
}
```


DELETE `https://revoxef.works/api/external/v2/catalog/modifierCategories/<modifier_category_id>`

### Modifier Groups

Can list, show, create, update and delete Modifier Groups

GET `https://revoxef.works/api/external/v2/catalog/modifierGroups`

Response for GET modifier groups is a modifier groups paginated array with the following fields:

| Field                   | Type    | Required     | Extra info    |
|-------------------------|---------|--------------|---------------|
| id                      | number  | -             | autoincrement |
| name                    | string  | **required** |               |
| order                   | number  | optional     |               |
| dont_show_automatically | boolean | optional     | default: 0    |

GET `https://revoxef.works/api/external/v2/catalog/modifierGroups/<modifier_group_id>`

POST `https://revoxef.works/api/external/v2/catalog/modifierGroups`

> Create a modifier group (POST) `catalog/modifierGroups`

```sh
{
    "name": "Modifier group 1"
    ...
}
```

> Update a modifier group (POST) `catalog/modifierGroups`

```sh
{
    "id": 1, // required to update
    "name": "Modifier group updated 1"
    ...
}
```

> Create/update múltiple modifier groups (POST) `catalog/modifierGroups`

```sh
{
    "modifierGroups": [
        {
            "id": 1, // only required to update
            "name": "Modifier group 1",
            ...
        },
        {
            "id": 2, // only required to update
            "name": "Modifier group 2",
            ...
        }
        ...
    ]
}
```


DELETE `https://revoxef.works/api/external/v2/catalog/modifierGroups/<modifier_group_id>`

### Modifier Pivot

The next pivot endpoints define the (optional) relationship between *Modifier Categories* and *Modifier Groups*.

Can list, show, create, update and delete Modifier Pivots

GET `https://revoxef.works/api/external/v2/catalog/modifierPivots`

Response for GET modifier pivots is a modifier pivots paginated array with the following fields:

| Field       | Type   | Required | Extra info        |
|-------------|--------|----------|-------------------|
| id          | number | -         | autoincrement     |
| order       | number | optional |                   |
| group_id    | number | **required** | modifier group    |
| category_id | number | **required** | modifier category |

GET `https://revoxef.works/api/external/v2/catalog/modifierPivots/<modifier_pivot_id>`

POST `https://revoxef.works/api/external/v2/catalog/modifierPivots`

> Create a modifier pivot (POST) `catalog/modifierPivots`

```sh
{
    "group_id": 1,
    "category_id": 1
    ...
}
```

> Update a modifier pivot (POST) `catalog/modifierPivots`

```sh
{
    "id": 1, // required to update
    "group_id": 2
    ...
}
```

> Create/update múltiple modifier pivots (POST) `catalog/modifierPivots`

```sh
{
    "modifierPivots": [
        {
            "id": 1, // only required to update
            "category_id": 1,
            ...
        },
        {
            "id": 2, // only required to update
            "category_id": 1,
            ...
        }
        ...
    ]
}
```


DELETE `https://revoxef.works/api/external/v2/catalog/modifierPivots/<modifier_pivot_id>`

## Menu Categories

An *Item* can be of Menu type.

This can be distinguished by item.type = 1.

Each *Menu Item* can contain different *Menu Categories* and each *Menu Category* can have multiple *Normal Items (item.type = 0)*:

*Menu Item* => *Menu Categories* => *Normal Items*

Can list, show, create, update and delete *Menu Categories*

GET `https://revoxef.works/api/external/v2/catalog/menuMenuCategories`

Response for GET menu categories is a menu categories paginated array with the following fields:

| Field            | Type   | Required     | Extra info                                                                                                                |
|------------------|--------|--------------|---------------------------------------------------------------------------------------------------------------------------|
| id               | number | -             | autoincrement                                                                                                             |
| name             | string | **required** |                                                                                                                           |
| order            | number | optional     |                                                                                                                           |
| isMultipleChoice | number | optional     | Select One: 0, Multiple choice: 1 (default), Select one mandatory: 2, Select by default (min-max): 3, Custom (min-max): 4 |
| max              | number | optional     | default: 1                                                                                                                |
| min              | number | optional     | default: 0                                                                                                                |
| item_id          | number | **required** | item.type = 1                                                                                                             |
| dish_order_id    | number | optional     |                                                                                                                           |

GET `https://revoxef.works/api/external/v2/catalog/menuMenuCategories/<menu_category_id>`

POST `https://revoxef.works/api/external/v2/catalog/menuMenuCategories`

> Create a menu category (POST) `catalog/menuMenuCategories`

```sh
{
    "name": "First dishes",
    "item_id": 1
    "isMultipleChoice": 2,
    ...
}
```

> Update a menu category (POST) `catalog/menuMenuCategories`

```sh
{
    "id": 1, // required to update
    "isMultipleChoice": 2
    ...
}
```

> Create/update múltiple menu menu categories (POST) `catalog/menuMenuCategories`

```sh
{
    "menuMenuCategories": [
        {
            "id": 1, // only required to update
            "name": "Menu category 1",
            "item_id": 1,
            ...
        },
        {
            "id": 2, // only required to update
            "name": "Menu category 2",
            "item_id": 2,
            ...
        }
        ...
    ]
}
```


DELETE `https://revoxef.works/api/external/v2/catalog/menuMenuCategories/<menu_category_id>`

### Menu Item-Category Pivot

This endpoint links the *Normal Items*, from the menu, with their *Menu Category*.

Can list, show, create, update and delete *Menu Item-Category Pivot*

GET `https://revoxef.works/api/external/v2/catalog/menuMenuItemCategoryPivots`

Response for GET menu item-category pivots is a menu item-category pivots paginated array with the following fields:

| Field             | Type    | Required     | Extra info    |
|-------------------|---------|--------------|---------------|
| id                | number  | -             | autoincrement |
| active            | boolean | optional     | default: 1    |
| order             | number  | optional     |               |
| price             | decimal | **required** |               |
| addModifiersPrice | boolean | optional     | default: 1    |
| item_id           | number  | **required** | item.type = 0 |
| category_id       | number  | **required** | menu category |
| separatorName     | string  | optional     |               |
| modifier_group_id | number  | optional     |               |
| format_pivot_id   | number  | optional     |               |

GET `https://revoxef.works/api/external/v2/catalog/menuMenuItemCategoryPivots/<menu_item_category_pivot_id>`

POST `https://revoxef.works/api/external/v2/catalog/menuMenuItemCategoryPivots`

> Create a menu item-category pivot (POST) `catalog/menuMenuItemCategoryPivots`

```sh
{
    "item_id": 1,
    "category_id": 1
    ...
}
```

> Update a menu item-category pivot (POST) `catalog/menuMenuItemCategoryPivots`

```sh
{
    "id": 1, // required to update
    "price": 1.00
    ...
}
```

> Create/update múltiple menu item-category pivots (POST) `catalog/menuMenuItemCategoryPivots`

```sh
{
    "menuMenuItemCategoryPivots": [
        {
            "id": 1, // only required to update
            "item_id": 1,
            "category_id": 1
            ...
        },
        {
            "id": 2, // only required to update
            "item_id": 2,
            "category_id": 1
            ...
        }
        ...
    ]
}
```


DELETE `https://revoxef.works/api/external/v2/catalog/menuMenuItemCategoryPivots/<menu_item_category_pivot_id>`

## Warehouses

GET Warehouses `https://revoxef.works/api/external/v2/warehouses`

> Response is a warehouses paginated array with the following fields:

```sh
    'id',
    'name',
    'order',
```

GET Warehouses stocks `https://revoxef.works/api/external/v2/warehouses/<warehouse_id>`

> Response for a warehouse is a stocks paginated array with the following fields:

```sh
    'id',
    'quantity',
    'defaultQuantity',
    'alert',
    'warehouse_id',
    'item_id',
    'unit_id'
```


POST `https://revoxef.works/api/external/v2/warehouses/<warehouse_id>/addStock`

> Add a stock to a warehouse (POST) `warehouses/<warehouse_id>/addStock`

```sh
{"item_id": <item_id>, "quantity": <quantity_to_add>}
```

POST `https://revoxef.works/api/external/v2/warehouses/<warehouse_id>/setStock`

> Set a item stock to a warehouse (POST) `warehouses/<warehouse_id>/setStock`

```sh
{"item_id": <item_id>, "quantity": <quantity_to_set>}
```

> Set multiple stocks to a warehouse (POST) `warehouses/<warehouse_id>/setStock`

```sh
{"stocks": [
    {"item_id": <item_id>, "quantity": <quantity_to_set>},
    {"item_id": <item_id>, "quantity": <quantity_to_set>},
]}
```


## Purchases

GET Purchase orders `https://revoxef.works/api/external/v2/purchaseOrders`

> Response is a purchase orders paginated array with the following fields:

```
    'id',
    'vendor_order_id',
    'reference',
    'subtotal',
    'tax',
    'total',
    'status',
    'vendor_id',
    'vendorName',
    'contentsArray'
```

GET Purchase order contents `https://revoxef.works/api/external/v2/purchaseOrders/{purchase_order_id}`

> Response is a purchase order contents paginated array with the following fields:

```
    'id',
    'status',
    'quantity',
    'received',
    'price',
    'subtotal',
    'tax',
    'total',
    'order_id',
    'item_vendor_id',
    'itemName',
    'itemBarcode',
    'item_id'
```

## Vendors

GET `https://revoxef.works/api/external/v2/vendors`

> GET ...vendors

```sh
{
    "current_page": 1,
    "data": [
        {
            "id": 1,
            "name": "RevoVendor",
            "address": "Address",
            "city": "City",
            "state": "State",
            "country": "Country",
            "postalCode": "00000",
            "nif": "000000000B",
            "web": null,
            "email": null,
            "phone": "111222333",
            "notes": null,
            "shouldBeNotified": 0
        },
        ...
```


GET `https://revoxef.works/api/external/v2/vendors/{vendor_id}`

> GET ...vendors/{vendor_id}

```sh
{
    "id": 1,
    "name": "RevoVendor",
    "address": "Address",
    "city": "City",
    "state": "State",
    "country": "Country",
    "postalCode": "00000",
    "nif": "000000000B",
    "web": null,
    "email": null,
    "phone": "111222333",
    "notes": null,
    "shouldBeNotified": 0
}
```


POST `https://revoxef.works/api/external/v2/vendors`

> POST ...vendors

```sh
{
    "name": "RevoVendor", // required
    "address": "Address", // required
    "city": "City",
    "state": "State",
    "country": "Country",
    "postalCode": "00000",
    "nif": "000000000B", // required | unique
    "web": null,
    "email": null,
    "phone": "111222333",
    "notes": null,
    "shouldBeNotified": 0
}
```

PUT `https://revoxef.works/api/external/v2/vendors/{vendor_id}`

> PUT ...vendors/{vendor_id}

```sh
{
    "name": "RevoVendor",
    "address": "Address",
    "city": "City",
    "state": "State",
    "country": "Country",
    "postalCode": "00000",
    "nif": "000000000B", // unique
    "web": null,
    "email": null,
    "phone": "111222333",
    "notes": null,
    "shouldBeNotified": 0
}
```

DELETE `https://revoxef.works/api/external/v2/vendors/{vendor_id}`

## Vendor Items

GET `https://revoxef.works/api/external/v2/vendors/{vendor_id}/items`

> GET ...items

```sh
{
    "current_page": 1,
    "data": [
        {
            "item_id": 1,
            "reference": "",
            "costPrice": "1.00",
            "unit_id": 1,
            "pack": 1,
            "tax_id": null
        },
        ...
```

POST `https://revoxef.works/api/external/v2/vendors/{vendor_id}/items`

> POST ...items

> With this endpoint you can create or update, if the vendor.item_id already exists.

```sh
{
    "items": [
        {
            "item_id": 1, // required
            "costPrice": "1.00",  // required
            "pack": 2,  // required
            "tax_id": 2,
            "unit_id": 1
        },
        {
            "item_id": 2, // required
            "costPrice": "2.00",  // required
            "pack": 2,  // required
            "reference": "Reference"
        }
    ]
}
```

DELETE `https://revoxef.works/api/external/v2/vendors/{vendor_id}/items`

> DELETE ...items

> All items_id must exist to delete their relation to vendor.

```sh
{ 
	"items": [ item1_id, item3_id, item10_id ]
}
```