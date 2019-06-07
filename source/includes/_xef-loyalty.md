# Xef Loyalty

## Base URL

`https://revoxef.works/api/loyalty/`


## Create an order

`POST orders`

Headers:

Parameter     | Value
--------------|-----------------
tenant        | account name
Authorization | Bearer {token}

Body:

Parameter | Type         | Description 
----------|--------------|-------------------------
order     | `json`       | Json encoded order   
customer  | `array`      | Of name and email `["name" => "The name", "email" => "theEmail@example.com"]`



```
Response:
{
    "total"     : 12.23,
    "taxAmount" : 1.23,
    "subtotal"  : 10,
    "sum"       : 10,
    "contents"  : [
        [
            "item_id"       : 1,
            "total"         : 7,
            "taxAmount"     : 2,
            "subtotal"      : 5,
            "itemPrice"     : 7,
            "taxPercentage" : 10,
        ], [
            "item_id"       : 2,
            "total"         : 7,
            "taxAmount"     : 2,
            "subtotal"      : 5,
            "itemPrice"     : 7,
            "taxPercentage" : 10,
        ]
    ]
}
```