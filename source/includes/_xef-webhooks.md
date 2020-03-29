# Xef Webhooks

## Introduction


Revo Xef comes with [webhooks](https://es.wikipedia.org/wiki/Webhook). You can add as many as you need in the `webhooks` page [https://revoxef.works/account/webhooks](https://revoxef.works/account/webhooks)

Once you create your first webhook a secret key will be generated and this will be used to create a hash that we will send in every webhook call so you can verify it is us who send it.

We will send the header.  

`X-Revo-Hmac-SHA256`

With the body hashed with the `SHA256` algorithm and using the `secret key` generated when creating the first webhook.

The payload will always have the keys

```sh
{
    "event" : "order.closed",
    "data" : {
        "id" : 298
        "table_id" : 12 
        "guests" : 4
    }
}
```

Key     | Description
--------|--------------------
event   | One of the available webhook events
data    | the data depending on the event


## Available webhooks

Model    | Events
---------|--------------------
Product  | updated, deleted
Customer | created, updated, deleted
Order    | created, closed

   
*There is the `{model}.*` event that means that you want to receive all the available events for the model*   


When creating a new `webhook` you will have the following options

Option   | Description
---------|---------------
Active   | If you want the event to be sent or not
Url      | The url it should call
Event    | The event that will triger it

## Product

```sh
// Updated
{
    "event" : "product.updated"
    "data" : {
        "id" : {id},
        "type" : {type},
        "name" : {name},
        "price" : {photo}
    }
}

// Deleted
{
    "event" : "product.deleted"
    "data":{
        "id" : {id}
    }
}
```


## Customer

```sh
// Created
{
    "event" : "customer.created"
    "data" : {
        "id" : {id},
        "active" : {active},
        "name" : {name},
        "address" : {address},
        "city" : {city},
        "state" : {state},
        "country" : {country},
        "postalCode" : {postalCode},
        "nif" : {nif},
        "web" : {web},
        "email" : {email},
        "phone" : {phone},
        "notes" : {notes},
        "extra_id" : {extra_id},
        "created_at" : {created_at},
        "updated_at" : {updated_at},
        "deleted_at" : {deleted_at},  
    }
}

// Updated
{
    "event" : "customer.updated"
    "data" : {
        "id" : {id},
        "active" : {active},
        "name" : {name},
        "address" : {address},
        "city" : {city},
        "state" : {state},
        "country" : {country},
        "postalCode" : {postalCode},
        "nif" : {nif},
        "web" : {web},
        "email" : {email},
        "phone" : {phone},
        "notes" : {notes},
        "extra_id" : {extra_id},
        "created_at" : {created_at},
        "updated_at" : {updated_at},
        "deleted_at" : {deleted_at},  
    }
}

// Deleted
{
    "event" : "customer.deleted"
    "data":{
        "id" : {id}
    }
}
```

## Order

```sh
// Created
{
    "event" : "order.created"
    "data" : {
        "id" : {id},
        "table_id" : {table_id},
        "total" : {total},       
        "guests": {guests} 
    }
}

// Closed
{
    "event" : "order.closed"
    "data":{
        "id" : {id},
        "table_id" : {table_id},
        "total" : {total},
        "guests": {guests} 
    }
}
```

