# Xef (Deprecated)

Event this `api` is still working, it will be deprecated and updated the `REST` in the following months

## Basic Request

```sh
    {
        "auth":{
            "tenant"  :"account",
            "password":"password"
        },
        "action":"action",
        "data":data
    }
```

To work with `Revo external api` a `POST` or `GET` request is required to the following url:

`https://revoxef.works/apiExternal`

With a variable named `message` that has to be a `json` with the following information


    
Field    | Type     | Description
---------|----------|---------------
account  | `string` | is the account (same name used to login)
password | `string` | is the [app password](/back/account#password) (not the backend password)
action   | `string` | the action to perform (explained below)
data     | `json`   | data required for the action (explained below) 
    
> The api has a limit of 60 requests every minute so the best practice is to cache for x time the fetched information done with the `getXXX` actions.


## Get

Returns the full data for an specific model and id.

```sh
"data":{
        "model":"model",
        "id":"id"
        }
```
            
Field    | Description
---------|---------------
model    | is the model to fetch within `MenuGroup` `MenuCategory` `MenuItem` `ModifierGroup` `ModifierCategory` `Modifier`
id       | is the id of the model to fetch


```sh
https://revoxef.works/apiExternal?message=
    {
        "auth"      : {"tenant":"account","password":"password"},
        "action"    : "get",
        "data"      : {"model":"MenuItem","id":"236"}
    }
```
    

## getMenuGroups

```sh
https://revoxef.works/apiExternal?message=
    {
        "auth":{"tenant":"account","password":"password"},
        "action":"getMenuGroups",
        "data":{}
    }
```

Since maybe we want to create an online shop that starts with menu groups, we can get them all using the action `getMenuGroups` with empty data.
This will return a list of all the groups (with their `ID`). With this Id we can [fetch the categories](#getChilds).


## getChildren

```sh
"data":{
    "model":"parentModel",
    "id":id
    }
```

This action returns all the children for an specific model
        
Field       | Type     | Description
------------|----------|---------------
parentModel | `string` | is the model we want its children. `MenuGroup` `MenuCategories` `ModifierCategory`
id          | `int`    | the id of the model we want its children.
        

```sh
https://revoxef.works/apiExternal?message=
    {
        "auth":{"tenant":"account","password":"password"},
        "action":"getChilds",
        "data":{"model":"MenuGroup","id":2}
    }
```
        
It will return all the childs of the `menuGroup` with `ID` 2.
    

## getInventory

```sh
 https://revoxef.works/apiExternal?message=
     {
         "auth":{"tenant":"account","password":"password"},
         "action":"getInventory",
         "data":{"warehouse_id":1,"items":[2,4,10]}
     }
```

Gets the current inventory for an Item in a Warehouse.


Field        | Type        | Description
-------------|-------------|---------------
warehouse_id | `int`       | Is the ID of the warehouse to get the quantity from
items        | `array int` | Array of the ids of the items to get the quantity


 
### decreaseInventory

Decreases the inventory for an Item in a Warehouse.
 
```sh
 https://revoxef.works/apiExternal?message=
     {
         "auth":{"tenant":"account","password":"password"},
         "action":"decreaseInventory",
         "data":{"warehouse_id":1,"item_id":2, "quantity":1}
     }
```
 

Field        | Type  | Description
-------------|-------|---------------
warehouse_id | `int` | Is the ID of the warehouse to decrease the quantity from
item_id      | `int` | Is the ID of the item to decrease
quantity     | `int` | Is the quantity to decrease


## Get images 
To get the image, you need to do a get request the ideal thing for the images is to store it in your server and only request the image if the path changed,

`https://a89f683ae563759322a9-3330e0d085d4ba4fd7f5395ee3f3cd7a.ssl.cf3.rackcdn.com/{account}/images/{image_path}`

or 

`https://a89f683ae563759322a9-3330e0d085d4ba4fd7f5395ee3f3cd7a.ssl.cf3.rackcdn.com/{account}/images/resized_100_{image_path}`

For a thumbnail

Field       | Type      | Description
------------|-----------|---------------
account     | `string`  | Is your account name, same name used for login into the app
image_path  | `string`  | is the photo variable of the model

## Revo Display Images

Returns an array of paths for each [Revo Display](/display) ads image.

To get the image

`https://a89f683ae563759322a9-3330e0d085d4ba4fd7f5395ee3f3cd7a.ssl.cf3.rackcdn.com/{account}/banners/{image_path}`

Field       | Type      | Description
------------|-----------|---------------
account     | `string`  | the account name
image_path  | `string`  | the image path returned in the array