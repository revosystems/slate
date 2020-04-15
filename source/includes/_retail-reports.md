# Retail Reports

## Authentication 

```sh
curl --header "tenant: {myaccount}" \
     --header "authorization: Bearer {token}" \
     https://revoxef.works/api/external/reports
```

Base endpoint

`https://revoxef.works/api/external/reports`

You should send the headers `tenant` with your account username and `authorization` with the token created at `account > tokens`

> Get your token at [https://revoretail.works/admin/account/tokens](https://revoretail.works/admin/account/tokens)

Header        | Value
--------------|-----------
username      | {account}
authorization | Bearer {token}


The `token` it obtained at [Account managment](https://revoretail.works/admin/account/tokens) section of RevoRetail. You just need to create a new token givin it a descriptive name. For example `api acess`.


## Response format

```sh
{
    "current_page": 1,
    "data": [  [Report data will go here]  ],
    "from": 1,
    "last_page": 4,
    "next_page_url": "https://revoretail.works/api/external/reports/{reportName}?page=2",
    "path": "https://revoretail.works/api/external/reports/{reportName}",
    "per_page": 50,
    "prev_page_url": null,
    "to": 50,
    "total": 190
}
```

All requests will return the info following this template.       
This format is paginated to avoid the system collapse. The object `data` contains the lines of the requested report.

## Available reports

Here you have a list of the available filters.

* categories
* categorySales
* contentsDiscounts
* customerOrderContents
* customersList
* customers
* deliveryNoteContents
* fullProductSales
* groups
* hours
* inOuts
* inventories
* inventoryContents
* marketingAnswers
* orderDiscounts
* orderInvoices
* ordersByCustomer
* orders
* payments
* presences
* productList
* productSales
* products
* stockCategories
* stockGroups
* stockMovements
* stocks
* targets
* taxes
* turns
* vendorItems

## Available filters
A parte de `start_date` y `end_date` se puede filtrar por otros campos (siempre que tengan sentido con el informe). A continuación les dejamos una lista:

Filter        | Value      | Description
--------------|------------|--------------
`start_date`  | YYYY-MM-DD | *Required* The start date of the report 
`end_date`    | YYYY-MM-DD | *Required* The end date of the report
`start_time`  | HH:mm      | The start time of the report 
`end_time`    | HH:mm      | The end time of the report 
`employee`    | int        | Id of the employee to filter with
`room`        | int        | Id of the room to filter with
`dayofweek`   | int        | Where Sunday is 1 and Saturday is 7
`priceRate`   | int        | The id of the price rate
`cashier`     | int        | The id of the cashier
`discount`    | int        | The id of the cashier
`dateField`   | string     | The date field that'll be used on filters query (created_at, updated_at, closed_at, opened_at)


<aside class="notice">
Just note that all filters don't work on all reports. To know which ones work, we recomend to visit the reports in the [backend](https://revoretail.works/admin/reports/summary) and see what parameters are applied at the url query when filtering.
</aside>


