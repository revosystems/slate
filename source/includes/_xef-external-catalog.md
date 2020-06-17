# Xef Catalog
Inside catalog there are many resources; items, categories, groups, modifiers and sellingFormats.

To explore those resources as a paginated list we can use a GET request with the following pattern. `GET https://revoxef.works/api/external/v2/catalog/{resource}` replacing the desired resource on this url. 

## Prerequisistes
To be able to use the external api you need a `revo xef` account and a `access token`

1. Login into the desired account
2. Go to account management
3. Create a new [token](https://revoxef.works/account/tokens)


## Basic usage
The main URL for the external api is

`https://revoxef.works/api/external/v2/catalog`

And you should provide the mandatory headers for the authentication


Header        | Value
--------------|----------
tenant        | {account-username}
Authorization | Bearer {the-token}


## Items

Can list, create, update and delete Items`

GET `https://revoxef.works/api/external/v2/catalog/items`
or 

GET `https://revoxef.works/api/external/v2/catalog/categories/<category_id>/items`

> Response for GET items is an items paginated array with the following fields:
```
    'id',
    'name',
    'nameKitchen',
    'buttonName',
    'price',
    'photo',
    'order',
    'info',
    'isOpen',
    'cookDuration',
    'type',
    'hasInventory',
    'usesWeight',
    'super_group_id',
    'extra_id',
    'printer_id',
    'useAverageCostPrice',
    'displayInventory',
    'allergies',
    'barcode',
    'minQuantity',
    'dish_order_id'
```

GET `https://revoxef.works/api/external/v2/catalog/items/<item_id>`

POST `https://revoxef.works/api/external/v2/catalog/items`

> Create a item
```sh
{"name": "Product 1", "active": 1, "category_id": 2}
```
> Update a item
```
{"id":2, "name": "Product 1 updated", "active": 0, "category_id": 2}
```
> Create multiple items
```
  {"items": [
        {"name": "Product 1", "active": 1, "category_id": 2},
        {"name": "Product 2", "active": 1, "category_id": 1}
    ]}
```
DELETE `https://revoxef.works/api/external/v2/catalog/items/<item_id>`


## Categories

`GET Categories`

```sh
GET https://revoxef.works/api/external/v2/catalog/categories
GET https://revoxef.works/api/external/v2/catalog/groups/<group_id>/categories
GET https://revoxef.works/api/external/v2/catalog/categories/<category_id>
POST https://revoxef.works/api/external/v2/catalog/categories
    Create a category:          {"name": "Category 1", "active": 1, "group_id": 2}
    Update a category:          {"id":2, "name": "Category 1 updated", "active": 0, "group_id": 2}
    Create multiple categories:  {"categories": [
            {"name": "Category 1", "active": 1, "group_id": 2},
            {"name": "Category 2", "active": 1, "group_id": 1}
        ]}
DELETE https://revoxef.works/api/external/v2/catalog/categories/<category_id>
```

> Response for GET categories is a categories paginated array with the following fields:

```
    'id',
    'name',
    'group_id',
    'active',
    'tax_id',
    'photo',
    'super_group_id',
    'printer_id',
    'printer_group_id',
    'modifier_category_id',
    'modifier_group_id',
    'dish_order_id'
```

## Groups

`GET Groups`

```sh
GET https://revoxef.works/api/external/v2/catalog/groups
GET https://revoxef.works/api/external/v2/catalog/groups/<group_id>
POST https://revoxef.works/api/external/v2/catalog/groups
    Create a group:          {"name": "Group 1", "active": 1}
    Update a group:          {"id":2, "name": "Group 1 updated", "active": 0}
    Create multiple groups:  {"groups": [
            {"name": "Group 1", "active": 1},
            {"name": "Group 2", "active": 1}
        ]}
DELETE https://revoxef.works/api/external/v2/catalog/groups/<group_id>
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

## Warehouses

`GET Warehouses`

```sh
GET https://revoxef.works/api/external/v2/warehouses
```

> Response is a warehouses paginated array with the following fields:

```
    'id',
    'name',
    'order',
```


`GET Warehouses stocks`

```sh
GET https://revoxef.works/api/external/v2/warehouses/<warehouse_id>
POST https://revoxef.works/api/external/v2/warehouses/<warehouse_id>/addStock  {"item_id": <item_id>, "quantity": <quantity_to_add>}
POST https://revoxef.works/api/external/v2/warehouses/<warehouse_id>/setStock  
    set a item stock to a warehouse     => {"item_id": <item_id>, "quantity": <quantity_to_set>}
    set multiple stocks to a warehouse  => {"stocks": [
        {"item_id": <item_id>, "quantity": <quantity_to_set>},
        {"item_id": <item_id>, "quantity": <quantity_to_set>},
    ]}
```

> Response for a warehouse is a stocks paginated array with the following fields:

```
    'id',
    'quantity',
    'defaultQuantity',
    'alert',
    'warehouse_id',
    'item_id',
    'unit_id'
```

## Purchase

`GET Purchase orders`

```sh
GET https://revoxef.works/api/external/v2/purchaseOrders
```

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

`GET Purchase order contents`

```sh
GET https://revoxef.works/api/external/v2/purchaseOrders/{purchase_order_id}
```

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

