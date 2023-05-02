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


## Catalog structure

The main catalog structure is the following:

Groups $\Rightarrow$ Categories $\Rightarrow$ Items

*Items* cannot exist without *Categories*, and *Categories* cannot exists without *Groups*.

## Groups

Can list, show, create, update and delete Groups

GET `https://revoxef.works/api/external/v2/catalog/groups`

> Response is a groups paginated array with the following fields:

```sh
    'id',
    'name',
    'photo',
    'active',
    'tax_id',
    'printer_id',
    'printer_group_id',
    'super_group_id',
    'extra_id',
```

GET `https://revoxef.works/api/external/v2/catalog/groups/<group_id>`

POST `https://revoxef.works/api/external/v2/catalog/groups`

> Create a group (POST) `catalog/groups`

> All parameters are optional.

```sh
{
    "name": "Group 1", // default: "Category Group"
    "active": 1, // default: 1
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

 <!---
> Create multiple groups (POST) `catalog/groups`
  
```sh
{
    "groups": [
        {
            "name": "Group 1",
            "active": 1
        },
        {
            "name": "Group 2",
            "active": 1
        }
    ]
}
```
--->

DELETE `https://revoxef.works/api/external/v2/catalog/groups/<group_id>`

## Categories

Can list, show, create, update and delete Categories

GET `https://revoxef.works/api/external/v2/catalog/categories` or 

GET `https://revoxef.works/api/external/v2/catalog/groups/<group_id>/categories`

> Response for GET categories is a categories paginated array with the following fields:

```sh
    'id',
    'name',
    'photo',
    'active',
    'group_id',
    'tax_id',
    'printer_id',
    'printer_group_id',
    'modifier_category_id',
    'modifier_group_id',
    'super_group_id',
    'dish_order_id',
    'extra_id'
```

GET `https://revoxef.works/api/external/v2/catalog/categories/<category_id>`

POST `https://revoxef.works/api/external/v2/catalog/categories`

> Create a category (POST) `catalog/categories`

```sh
{
    "name": "Category 1", // default: "Menu Category"
    "active": 1, // default: 1
    "group_id": 1 // required
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

<!--
> Create multiple categories  (POST) `catalog/categories`

```sh
{"categories": [
    {"name": "Category 1", "active": 1, "group_id": 2},
    {"name": "Category 2", "active": 1, "group_id": 1}
]}
```
--->

DELETE `https://revoxef.works/api/external/v2/catalog/categories/<category_id>`

## Items

For the *Items*, there are the following types:

- *Normal Items*: *item.type = 0*
- *Menu Items*: *item.type = 1*
- *Selling format Items*: *item.type = 4* 

Can list, show, create, update and delete *Items*

GET `https://revoxef.works/api/external/v2/catalog/items` or 

GET `https://revoxef.works/api/external/v2/catalog/categories/<category_id>/items`

> Response for GET items is an items paginated array with the following fields:

```sh
    "id"
    "name"
    "price"
    "photo"
    "active"
    "info"
    "type"
    "hasInventory"
    "usesWeight"
    "category_id"
    "tax_id"
    "printer_id"
    "printer_group_id"
    "modifier_group_id"
    "modifier_category_id"
    "isMenu"
    "shouldAppearInMenuList"
    "super_group_id"
    "isOpen"
    "nameKitchen"
    "costPrice"
    "displayInventory"
    "isLinked"
    "allergies"
    "dish_order_id"
    "barcode"
    "unit_id"
    "extra_id"
    "useAverageCostPrice"
    "cookDuration"
    "buttonName"
    "minQuantity"
    "report_category_id"
    "config"
```

GET `https://revoxef.works/api/external/v2/catalog/items/<item_id>`

POST `https://revoxef.works/api/external/v2/catalog/items`

> Create a item (POST) `catalog/items`

```sh
{
    "name": "Product 1", // default: "New Item"
    "price": 8.50,
    "active": 1, // default: 1
    "category_id": 1 // required
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

<!---
> Create multiple items (POST) `catalog/items`

```sh
  {"items": [
        {"name": "Product 1", "active": 1, "category_id": 2},
        {"name": "Product 2", "active": 1, "category_id": 1}
    ]}
```
--->

DELETE `https://revoxef.works/api/external/v2/catalog/items/<item_id>`

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