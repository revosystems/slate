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
        "opened" : {opened},
        "closed" : {closed},
        "merged" : {merged},
        "guests" : {guests},
        "orderDiscountAmount" : {orderDiscountAmount},
        "sum" : {sum},
        "discountAmount" : {discountAmount},
        "subtotal" : {subtotal},
        "taxAmount" : {taxAmount},
        "total" : {total},
        "alreadyPaid" : {alreadyPaid},
        "tenantUser_id" : {tenantUser_id},
        "tenantUserName" : {tenantUserName},
        "discount" : {discount},
        "customer_id" : {customer_id},
        "table_id" : {table_id},
        "tableName" : {tableName},
        "status" : {status},
        "notes_sent" : {notes_sent},
        "delivery_id" : {delivery_id},
        "margin" : {margin},
        "gratuity" : {gratuity},
        "offline_id" : {offline_id},
        "notes" : {notes},
        "refunded_order_id" : {refunded_order_id},
        "orderContents" : [
          {
           "id" : {id},
           "order_id" : {order_id},
           "item_id" : {item_id},
           "dishOrder" : {dishOrder},
           "seat" : {seat},
           "quantity" : {quantity},
           "weight" : {weight},
           "itemPrice" : {itemPrice},
           "alreadyPaidQuantity" : {alreadyPaidQuantity},
           "alreadyPrintedQuantity" : {alreadyPrintedQuantity},
           "modifiers" : [
            {
             "category_id" : {category_id},
             "id" : {id},
             "order" : {order},
             "name" : {name},
             "photo" : {photo},
             "price" : {price},
            }, {
             "category_id" : {category_id},
             "id" : {id},
             "order" : {order},
             "name" : {name},
             "photo" : {photo},
             "price" : {price},
           },
         ],
         "discount" : null,
         "discountAmount" : "0.00",
         "taxAmount" : "0.10",
         "taxPercentage" : "10.00",
         "total" : "1.15",
         "extrasAmount" : "0.00",
         "subtotal" : "1.05",
         "priceWithExtrasIndividual" : "1.15",
         "notes" : null,
         "cancelReason" : null,
         "created_at" : "2016-10-25 12:43:25",
         "updated_at" : "2016-10-25 13:08:34",
         "itemName" : "Café",
         "optional_modifiers" : null,
         "format_pivot_id" : null,
         "margin" : null,
         "price_id" : null,
         "tenantUser_id" : null,
         "menuMenuContents" : [],
         "orderContentCombination" : null,

          }
        ],
        "orderInvoices" : [
          {
            "id" : {id},
            "order_id" : {order_id},
            "customer_id" : {customer_id},
            "customerJSON" : {customerJSON},
            "date" : {date},
            "paymentMode" : {paymentMode},
            "sum" : {sum},
            "discountAmount" : {discountAmount},
            "orderDiscountAmount" : {orderDiscountAmount},
            "taxAmount" : {taxAmount},
            "subtotal" : {subtotal},
            "total" : {total},
            "cancelReason" : {cancelReason},
            "number" : {number},
            "notAccountable" : {notAccountable},
            "orderPayments" : [
              {
                "id" : {id},
                "invoice_id" : {invoice_id},
                "tenantUser_id" : {tenantUser_id},
                "tenantUserName" : {tenantUserName},
                "date" : {date},
                "tipAmount" : {tipAmount},
                "payAmount" : {payAmount},
                "totalCollected" : {totalCollected},
                "change" : {change},
                "paymentMethod" : {paymentMethod},
                "voidReason" : {voidReason},
                "turn_id" : {turn_id},
                "cancelReason" : {cancelReason},
                "gratuity" : {gratuity},
                "provider_id" : {provider_id},
              },
            ]
          }
        ],
        "delivery": null  
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

