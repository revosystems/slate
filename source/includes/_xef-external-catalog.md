# Xef Catalog
Inside catalog there are many resources; items, categories, groups, modifiers and sellingFormats.

To explore those resources as a paginated list we can use a GET request with the following pattern. `GET https://revoxef.works/api/external/v2/catalog/{resource}` replacing the desired resource on this url. 

## Prerequisistes
To be able to use the external api you need a `revo xef` account and a `access token`

1. Login into the desired account
2. Go to account management
3. Create a new [token](https://revoxef.works/account/tokens)


## Basic usage
The main URL for the external api

`https://revoxef.works/api/external/v2/catalog`

And you should provide the mandatory headers for the authentication


Header        | Value
--------------|----------
tenant        | {account-username}
Authorization | Bearer {the-token}
client-token  | {client-token}


## Items

Can list, show, create, update and delete Items`

GET `https://revoxef.works/api/external/v2/catalog/items` or 

GET `https://revoxef.works/api/external/v2/catalog/categories/<category_id>/items`

> Response for GET items is an items paginated array with the following fields:

```sh
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

> Create a item (POST) `catalog/items`

```sh
{"name": "Product 1", "active": 1, "category_id": 2}
```

> Update a item (POST) `catalog/items`

```sh
{"id":2, "name": "Product 1 updated", "active": 0, "category_id": 2}
```

> Create multiple items (POST) `catalog/items`

```sh
  {"items": [
        {"name": "Product 1", "active": 1, "category_id": 2},
        {"name": "Product 2", "active": 1, "category_id": 1}
    ]}
```

DELETE `https://revoxef.works/api/external/v2/catalog/items/<item_id>`


## Categories

Can list, show, create, update and delete Categories

GET `https://revoxef.works/api/external/v2/catalog/categories` or 

GET `https://revoxef.works/api/external/v2/catalog/groups/<group_id>/categories`

> Response for GET categories is a categories paginated array with the following fields:

```sh
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

GET `https://revoxef.works/api/external/v2/catalog/categories/<category_id>`

POST `https://revoxef.works/api/external/v2/catalog/categories`

> Create a category (POST) `catalog/categories`

```sh
{"name": "Category 1", "active": 1, "group_id": 2}
```

> Update a category  (POST) `catalog/categories`

```sh
{"id":2, "name": "Category 1 updated", "active": 0, "group_id": 2}
```          

> Create multiple categories  (POST) `catalog/categories`

```sh
{"categories": [
    {"name": "Category 1", "active": 1, "group_id": 2},
    {"name": "Category 2", "active": 1, "group_id": 1}
]}
```  

DELETE `https://revoxef.works/api/external/v2/catalog/categories/<category_id>`


## Groups

Can list, show, create, update and delete Groups

GET `https://revoxef.works/api/external/v2/catalog/groups`

> Response is a groups paginated array with the following fields:

```sh
    'id',
    'name',
    'active',
    'tax_id',
    'photo',
    'super_group_id',
    'printer_id',
    'printer_group_id',
```

GET `https://revoxef.works/api/external/v2/catalog/groups/<group_id>`

POST `https://revoxef.works/api/external/v2/catalog/groups`

> Create a group (POST) `catalog/groups`

```sh
{"name": "Group 1", "active": 1}
```

> Update a group (POST) `catalog/groups`

```sh
{"id":2, "name": "Group 1 updated", "active": 0}
```

> Create multiple groups (POST) `catalog/groups`
  
```sh
{"groups": [
            {"name": "Group 1", "active": 1},
            {"name": "Group 2", "active": 1}
        ]}
```

DELETE `https://revoxef.works/api/external/v2/catalog/groups/<group_id>`

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

