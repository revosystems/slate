# Xef Catalog
Inside catalog there are many resources; items, categories, groups, modifiers and sellingFormats.

To explore those resources as a paginated list we can use a GET request with the following pattern. `GET https://revoxef.works/api/external/catalog/{resource}` replacing the desired resource on this url. 

## Prerequisistes

To be able to use the external api you need a `revo xef` account and a `access token`

1. Login into the desired account
2. Go to account management
3. Create a new [token](https://revoxef.works/account/tokens)


## Basic usage
The main URL for the external api is


`https://revoxek.works/api/external/catalog`

And you should provide the mandatory headers for the authentification


Header        | Value
--------------|----------
tenant        | {account-username}
Authorization | Bearer {the-token}


## Products

`GET Products`

```sh
GET https://revoxef.works/api/external/catalog/products
```

> Response is a products paginated array with the following fields:

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


## Categories

`GET Categories`

```sh
GET https://revoxef.works/api/external/catalog/categories
```

> Response is a categories paginated array with the following fields:

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
GET https://revoxef.works/api/external/catalog/groups
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
