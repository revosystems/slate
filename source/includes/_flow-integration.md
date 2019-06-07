# RevoFlow integration API

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
 
 `https://admin.revo.works/apiFlow`
 
 With a variable named `message` that has to be a `json` with the following information
 

     

Field        | Type       | Description
-------------|------------|---------------------------
 account     | `string`   | is the account (same name used to login)
 password    | `string`   | is the [app password](/back/account#password) (not the backend password)
 action      | `string`   | the action to perform (explained below)
 data        | `json`     | data required for the action (explained below) 
     
 > The api has a limit of 60 requests every minute so the best practice is to cache for x time the fetched information done with the `getXXX` actions.
 
 
## Sync
 
 Returns the full data for an specific model and id.
 
```sh
"data":{
    "model":"TableTable"
    "fromDate:"yyyy/mm/dd HH:mm:ss",
}
 ```
             
Field    | Description
---------|---------------------------
model    | is the model to sync in `TableRoom` `TableTable`
fromDate | the date from which changes to sync
 
```sh
 https://admin.revo.works/apiFlow?message=
{
"auth":{
    "tenant"  :"user1",
    "password":"user1"
    },
    "action":"sync", 
    "data":{
        "model":"TableRoom",
        "fromDate":"2016/12/01 16:05:20"
    }
}
     
 ```
 
## searchProducts

This action searches and returns the products where the name is like the text you ask for

```sh    
"data": {
  "text": "burger"
}
```     
 
> Text needs to be at least of 3 characters
 
## updateTables

```sh    
"data": {
  "tables": [
    {'id':1,'x':10,'y':10,'width':100,'height':100,'isJoined':false},
    {'id':2,'x':20,'y':20,'width':100,'height':100,'isJoined':true},
    {'id':3,'x':30,'y':30,'width':100,'height':100,'isJoined':false}
  ]
}
 ```   

This action updates the tables Layout (not the original one)

 
## newOrder
 
```sh
"data": {
    "table_id": 3,
    "guests": 4,
    "contents": [
      {
        "itemPrice": 10.2,
        "itemName": "product name",
        "item_id": 5,
        "quantity": 2
      },
      {
        "itemPrice":7,
        "itemName": "product name 2",
        "item_id": 2,
        "quantity": 4
      }
    ]
    "payments":[
      {
        "payment_method_id:1,
        "amount":10.10,
      }
    ]
}
```
  
## updateOrder
```sh
"data": {
    "table_id": 2,
    "guests": 4,
    "payments":[
        {
            "payment_method_id:1,
            "amount":10.10,
        }
    ]
}
```

Use this function to create a new Order to Revo,
You can send payments where `payment_method_id` is `1` (Card) or `2` (Cash)
