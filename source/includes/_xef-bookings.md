# Flow Api

## Prerequisistes

To be able to use the Flow api you need a `revo xef` account and a `access token`

1. Login into the desired account
2. Go to account management
3. Create a new [token](https://revoxef.works/account/tokens)


## Basic usage
The main URL for the external api is


`https://revoxef.works/apiFlow`


## Creating an order with customer

`POST`

```shell script
POST message={
             	"auth":{
             		"tenant":"account",
             		"token":"account_token"
             	},
             	"action":"newOrder",
             	"data":{
             		"customer":{
             			"name":"customer name",
             			"email":"email2@example.com",
                        "notes":"{\"note1\":\"text1\",\"note2\":\"text2\"...}
             		},
             		"table_id":[
             			1,
             			2,
             			3
             		],
             		"guests":"8",
             		"contents":[
             		],
             		"payments":[
             		]
             	}
             }
```

> Response with the created order identifier 

```shell script
  {
        "result"        : true,
        "errorMessage" : null,
        "data"         : {
            "id" : 1
        }
  }
```
