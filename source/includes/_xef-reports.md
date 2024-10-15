# Xef Reports
## Authentication

```shell 
curl --header "tenant: {myaccount}" \
     --header "authorization: Bearer {token}" \
     --header 'client-token: {client-token}' \
https://revoxef.works/api/external/v3
```

Base endpoint

`https://revoxef.works/api/external/v3`

You should send the headers `tenant` with your account username and `authorization` with the token created at `account > tokens`

> The API has a data size límit of 2MB for each request.

> The API has a limit of 120 requests every minute so the best practice is to cache for x time the fetched information done with the `getXXX` actions.

> Get your token at [https://revoxef.works/account/tokens](https://revoxef.works/account/tokens)

## Request

```shell
curl --header "tenant: {myaccount}" --header "authorization: Bearer {token}" \ --header 'client-token: {client-token}' \
https://revoxef.works/api/external/v3/reports/{reportName}?start_date=2018-01-01&end_date=2018-01-29
```

`GET reports/{reportName}`

| Field        | Type       | Required | Description                                                               |
|--------------|------------|----------|---------------------------------------------------------------------------|
| `start_date` | YYYY-mm-dd | optional | The initial date for the report. Default: start of month.                 |
| `end_date`   | YYYY-mm-dd | optional | The final date for the report. Default: today.                            |
| `page`       | number     | optional | As the data is paginated, use this parameter to select the page to fetch. |
| `pagination` | number     | optional | Number of objects per page. The default value is **50** and the max allowed is **200**. |


### Response

```shell
{
    "current_page": 1,
    "data": {"HERE WILL GO THE SPECIFIC REPORT DATA"},
    "from": 1,
    "last_page": 4,
    "next_page_url": "https://revoxef.works/api/external/v3/reports/{reportName}?page=2",
    "path": "https://revoxef.works/api/external/v3/reports/{reportName}",
    "per_page": 50,
    "prev_page_url": null,
    "to": 50,
    "total": 190
}
```

All responses will have this header

## Available reports
Here is a list of all available reports

* actionLog
* categories
* contentDiscounts
* cookDurations
* customers
* discounts
* groupedInvoices
* hours
* channels
* invoices
* menuProducts
* menus
* openOrders
* orderContents
* orders
* payments
* presences
* products
* rooms
* receipts
* stocks
* stockMovements
* inventoryContents
* taxes
* turns
* tenantUsers
* warehouses
* allProducts
* canceledContents
* canceledOrders



### Filters

All filters must be used as a query parameters.

Filter        | Type       | Belongs to report/resource | Description
--------------|------------|---------------------|-------------
`start_date`  | YYYY-mm-dd | All                 | (required) The initial date for the report
`end_date`    | YYYY-mm-dd | All                 | (required) The final date for the report
`start_time`  | HH:mm      | All                 | The start time for the report
`end_time`    | HH:mm      | All                 | The end time for the report
`dayofweek`   | int        | All                 | Where Sunday is 1 and Saturday is 7
`room`        | int        | Orders              | Room id
`priceRate`   | int        | Contents            | Price rate id
`forTurn`     | int        | Orders              | Filter orders for turn_id (forTurn=turn_id). This filter already includes withInvoices and withPayments filters. Date filters (start_date and end_date) do nothing with forTurn filter
`table`       | int        | Orders / Contents   | Filter orders/contents for table_id (table=table_id).
`fullPrecision` | bool        | All   | Displays [all possible numeric fields](#fields-with-fullprecision) with their complete decimal precision when ?fullPrecision=true is applied
`withInvoices`| -          | Orders              | Append invoices resource
`withContents`| -          | Orders              | Append contents resource
`withDelivery` | -         | Orders              | Append delivery resource
`withPayments`| -          | Invoices            | Append payments resource
`withFiscal`  | -          | Invoices            | Append fiscal data resource
`withItem`    | -          | Contents            | Append item resource
`withSubContents`| -       | Contents            | Append subContents (menu contents) resource
`withModifiers` | -        | Contents            | Append modifier resource
`withPriceRate` | -        | Contents            | Append price rate resource ("type": 0(percentage)/1(price))
`withAppliedTaxes` | -     | Contents / Invoices | Append taxes information applied to the resource (JSON example below)

Example:

Using order reports endpoint, if you want PriceRates resource, as it belongs to Contents, you will also need withContents filter.

<aside class="notice">
Take into account that not all filters are available for all reports. To exactly know which filters fits on each report is recommended to visit every report at [RevoXef](https://revoxef.works/reports/summary) and take a look at the url paramteres that appear when filtering.
</aside>

```shell
?withAppliedTaxes
{
    ...
    "RESOURCE": [ // "invoices", "contents", "sub_contents"
        {
            ...
            "applied_taxes": [
                {
                    "id": 1, // register ID
                    "tax_id": 3, // tax ID
                    "tax_name": "IVA 21%",
                    "tax_percentage": 21.00,
                    "base_amount": 0.8264, // up to 4 decimal precision
                    "tax_amount": 0.1736 // up to 4 decimal precision
                }
            ],
            ...
        }
    ]
}
```

### Fields with fullPrecision

This section lists all fields that can have their decimal precision increased by using the "fullPrecision" [filter](#filters).

Field             | Resource  | Precision without flag   | Precision with flag
------------------|-----------|--------------------------|---------------------
`subtotal`        | Contents  | 2                        | 4
`tax`             | Contents  | 2                        | 4
`discount_amount` | Contents  | 2                        | 4


## Master
In case you have a master account you can create a token for the master account and use it to access all the chain reports as well as get a list of all the accounts within the master account

### Get Accounts V3

`GET https://revoxef.works/api/external/v3/accounts`

Using the optional query parameter `?withBusiness=true` in the endpoint, we can retrieve information about the business account.

```shell
V3 endpoint
{
    "data": [
        {
            "id": 1,
            "tenant": "accountname",
            "business": { // only with withBusiness flag
                "name": "Name",
                "legal_name": "Legal Name",
                "slogan": "Sloga",
                "phone": "123456789",
                "address": "Address",
                "city": "City",
                "state": "State",
                "country": "Country",
                "postal_code": "01234",
                "nif": "B123456789",
                "logo": "abcdefg123.png",
                "web": "https://www.revo.works",
                "tax_included": true, // bool
                "opening_time": "05:00:00",
                "cash_control_type": "SIMPLE", // "NONE", "SIMPLE", "FULL" or "AUTO"
                "blind_cashier": false, // bool
                "presence_control": true, // bool
                "currency": "EUR",
                "second_currency": "USD",
                "exchange_rate": 0,  // 4 decimal precision
                "currency2_cash_payment_method_id": 8,
                "currency2_card_payment_method_id": 9,
                "comma_decimal": true, // bool
                "languages": [ // "ca", "en", "es", "eu", "fr", "pt", "it", "zh", "de"
                    "ca"
                ]
            }
        },
        ...
    ]
}
```

### Get Accounts V2 (old)

`GET https://revoxef.works/api/external/v2/accounts`

```shell
V2 endpoint (old)
{
    "IDTenant": "Tenant/AccountName"
}
````

### Chain reports

`GET https://revoxef.works/api/external/v2/accounts/reports/{reportName}`

And you have to add a new header that defines the chain to connect to

Header        | Value          | Description
--------------|----------------|-----------
tenant        | account        | Master account name,
authorization | Bearer {token} | The [token](https://revoxef.works/account/tokens) created at backoffice
chain         | chain          |chain username returned in the accounts GET method


### Chain fetch order

`GET https://revoxef.works/api/external/v2/accounts/orders/{id}`

You can fetch one (or multiple orders) for an specific chain using the same header as in the previous example.

> The `id` field can be a single order id or multiple orders separated by , such as `id1,id2,id3`



