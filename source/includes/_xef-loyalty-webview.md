# Xef Loyalty WebView

## Introduction
Since Revo Xef 4.1 we include a new integration that lets you manipulate the current order through a webview and a powerful Javascript Api.

You can enable it by adding the integration called `RevoLoyalty WebView` and providing your `URL`.
When this integration is enabled in the edit order screen, there will appear a new action that will open your site and inject the `RevoLoyaltyJs` framework for you to be able to interact with the current order.

This api includes a few methods described below

> To be able to interact with iOS all the function will have a callback parameter that will be called when the action is finished in the POS

## GET ORDER
Gets all the information of the current order

```
    RevoLoyalty.getOrder(function(json) {
        var order = JSON.parse(json)
        const total = order["total"]
    })
```

## ADD EXISTING DISCOUNT
Adds a discount that exists in the database to the whole order

```
    RevoLoyalty.addExistingDiscount({id}, function(error) {

    })
```

Parameter         | Type         | Description 
------------------|--------------|-------------------------
id                | `int`        | The id of the discount on the database

Result: Error if the discount does not exist

## ADD PERCENTAGE DISCOUNT
Adds a new discount with percentage to the whole order

```
RevoLoyalty.addPercentageDiscount({name}, {percentage}, function() {
    
})
```

Parameter         | Type         | Description 
------------------|--------------|-------------------------
name              | `string`     | The name of the discount to show on the ticket
percentage        | `number`     | The percentage for the discount

## ADD AMOUNT DISCOUNT
Adds a new discount with amount to the whole order

```
RevoLoyalty.addAmountDiscount({name}, {amount}, function() {

})
```

Parameter         | Type         | Description 
------------------|--------------|-------------------------
name              | `string`     | The name of the discount to show on the ticket
amount            | `number`     | The amount for the discount


## ADD EXISTING CONTENT DISCOUNT
Adds a discount that exists in the database to the whole order

```
    RevoLoyalty.addContentExistingDiscount({line}, {id}, function(error) {

    })
```

Parameter         | Type         | Description 
------------------|--------------|-------------------------
line              | `int`        | The index of the array of the content received in the getOrder to apply the discount
id                | `int`        | The id of the discount on the database

Result: Error if the discount does not exist

## ADD CONTENT PERCENTAGE DISCOUNT
Adds a new discount with percentage to the whole order

```
RevoLoyalty.addContentPercentageDiscount({line}, {name}, {percentage}, {withoutExtras}, function() {
    
})
```

Parameter         | Type         | Description 
------------------|--------------|-------------------------
line              | `int`        | The index of the array of the content received in the getOrder to apply the discount
name              | `string`     | The name of the discount to show on the ticket
percentage        | `number`     | The percentage for the discount
withoutExtras     | `bool`       | If the discount should be applied to the extras or not (ex: modifiers)

## ADD CONTENT AMOUNT DISCOUNT
Adds a new discount with amount to the whole order

```
RevoLoyalty.addContentAmountDiscount({line}, {name}, {amount}, {withoutExtras}, function() {

})
```

Parameter         | Type         | Description 
------------------|--------------|-------------------------
line              | `int`        | The index of the array of the content received in the getOrder to apply the discount
name              | `string`     | The name of the discount to show on the ticket
amount            | `number`     | The amount for the discount
withoutExtras     | `bool`       | If the discount should be applied to the extras or not (ex: modifiers)

## ADD PRODUCT
Adds a new content to the order, the product must me of type `standard` as no popups can be shown

```
RevoLoyalty.addProduct({id}, {price}, function(error) {
    
})
```

Parameter         | Type         | Description 
------------------|--------------|-------------------------
id                | `int`        | The id of the product
price             | `number`     | *optional* The price of the product, send null to use the database price

Error when the product does not exists or is not standard

## ADD Payment
Adds a payment to the order, it will create an invoice there isn't one yet

```
RevoLoyalty.addPayment({id}, {amount}, function(error) {
    
})
```

Parameter         | Type         | Description 
------------------|--------------|-------------------------
id                | `int`        | The id of the payment method
amount            | `number`     | The amount paid

error when the payment method does not exist


## CLOSE
Closes the webview

```
    RevoLoyalty.close()
``` 


