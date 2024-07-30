# RevoFlow

## Base URL  
`https://revoflow.works/api/v1/`


## Authorization
The authorization is send inside request headers using the following fields:

Field           | Value          | 
----------------|----------------|--
Tenant 			| {username}     |
Authorization 	| Bearer {token} | backend generated token

Response JSON: 

```
    {
        "ok": true/false, (shows wether request succeded or not).
        "message": "", (when error occurs, an error message is sent)
        "data": {}/[]
    }
```
 
## Bookings

Start and End dates must be sent. Response is an array of bookings inside requested dates.

`https://revoflow.works/api/v1/bookings?start=2017-01-01&end=2017-01-31`

```
    {
        "ok": true,
      "message": "",
      "data": [
        {
          "id": 1,
          "date": "2017-01-19",
          "time": "12:30:00",
          "duration": "02:00:00",
          "status": 1,
          "guests": 3,
          "shift_id": 1,
          "origin": 3,
          "customer_id": 1,
          "notes": "",
          "employee_id": null,
          "employeeUserName": "Widget",
          "booking_reference_id": null,
          "tables": "[1]",
          "isMailSent": false,
          "isOrderSent": false,
          "room_id": null,
          "cancelation_reason_id": null,
          "sentToRevo": null,
          "token": null,
          "total": "0.00",
          "worker_id": null,
          "order_id": null,
          "pre_assigned_tables": null,
          "payments": [
            {
              "id": 1,
              "name": "pepito",
              "amount": "12.50",
              "payment_method_id": 1,
              "booking_id": 1,
              "employee_id": 1
            }
          ],
          "products": [
            {
              "id": 1,
              "name": "Product 1",
              "product_id": 1,
              "revo_id": 1,
              "price": "12.50",
              "quantity": 1,
              "booking_id": 1,
              "employee_id": 1,
              "duration": null,
              "spacing": null,
              "type": null
            }
          ]
        },
        ...]
    }
```

Finds a booking by its ID (in that example, the booking id is 1)    
`https://revoflow.works/api/v1/bookings/1`


### Save Booking (POST)
`https://revoflow.works/api/v1/bookings`

```
    { 
        data: {   
            "booking": {
                "date": "2017-01-19",
                "time": "12:30:00",
                "duration": "02:00:00",
                "guests": 3,
                "shift_id": 1,
                "notes": ""
            }
            "customer": {
                "name": "John Tree",
                "email": "john@email.com",
                "phone": "458234923",
                "notes": "Notes"
            }
        }
    }
```


## Update booking (PUT)
`https://revoflow.works/api/v1/bookings/{id}`

```
    { 
        data: {   
            "booking": {
                "date": "2017-01-19",
                "time": "12:30:00",
                "duration": "02:00:00",
                "guests": 3,
                "shift_id": 1,
                "notes": "",
                ...
            }
        }
    }

```

## Customers 
Gets the a list of paginated customers.
The page and limit must be set in request.

`https://revoflow.works/api/v1/customers?page=1&limit=10`

```   
    {
        "ok": true,
        "message": "",
        "data": {
            "currentPage": "1",
            "customers": [
              {
                "id": 1,
                "name": "asdfa",
                "address": "Address",
                "city": "City",
                "state": "State",
                "country": "Country",
                "postalCode": "Postal Code",
                "nif": null,
                "web": null,
                "email": "eduard@revo.works",
                "phone": "asdfasdf",
                "notes": null,
                "password": null,
                "remember_token": null,
                "created_at": "2017-01-17 00:10:12",
                "updated_at": "2017-01-17 00:10:12",
                "deleted_at": null
             }
            ],
            "pages": 1,
            "left": 0
        }
    }
```


## Products
Get products list

`https://revoflow.works/api/v1/products`

```
    {
      "ok": true,
      "message": "",
      "data": [
        {
          "id": 1,
          "name": "Patatas Picantes",
          "revo_id": 956,
          "price": "4.50",
          "startDate": null,
          "endDate": null,
          "photo": "FlKi6IoWZ8.png",
          "duration": "00:00:00",
          "spacing": "00:00:00",
          "type": 0,
          "order": 0,
          "shifts": [1, 2]
        },
        ...
      ]
    }
```

You can also filter products by shift ids.

> Filter by shift ids (JSON body):

```
{
  "shifts", [{shift_id1}, {shift_id2}, ...]
}
```

Get product by its ID
`https://revoflow.works/api/v1/products/1`


## Shifts
Get shifts list
`https://revoflow.works/api/v1/shifts`

```
    {
      "ok": true,
      "message": "",
      "data": [
        {
          "id": 1,
          "name": "Comida 1",
          "startTime": "12:00:00",
          "slotTime": "02:00:00",
          "endTime": "14:00:00",
          "weekdays": "[1,1,1,1,1,1,1]",
          "deleted_at": null,
          "weekday": null,
          "shifts": [1, 2]
        },
        ...
      ]
    }
```   

## Tables
Get tables by day and shift.
`https://revoflow.works/api/v1/tables?date=2017-02-01&shift_id=1`

```
    {
      "ok": true,
      "message": "",
      "data": [
            {"id":1,"name":"Table 1","isJoined":"0","room_id":"1"},
            {"id":2,"name":"Table 2","isJoined":"0","room_id":"1"},
        }
      ]
    }    
 ```   
    

## Availabillity
Get the availabillity between requested dates.
`https://revoflow.works/api/v1/availability?from=2017-02-01&to=2017-02-15`

```
    {
      "ok": true,
      "message": "",
      "data": [
        {
          "2017-01-30": {
            "1": {
              "capacity": 114,
              "available": 114,
              "shift_id": 1
            },
            "2": {
              "capacity": 114,
              "available": 114,
              "shift_id": 2
            },
            "3": {
              "capacity": 114,
              "available": 114,
              "shift_id": 3
            }
          }
        }
      ]
    }
```

## Time Available
Get the available time on each shift.
`https://revoflow.works/api/v1/shiftsTime?date=2017-02-01&guests=5`

```
    {
      "ok": true,
      "message": "",
      "data": [
        {
            "name": "Dinner",
            "id": 1,
            "time" : [
                "20:00:00", 
                "20:15:00", 
                "20:30:00", 
                "20:45:00", 
                "21:00:00", 
                "21:15:00", 
                "21:30:00", 
                "21:45:00", 
                "22:00:00"
            ]
        }
      ]
    }
```    
    

## Next bookings
Get next bookings inside interval ["-1h" - "+2h"].

`https://revoflow.works/api/v1/nextBookings`

```
    {
      "ok": true,
      "message": "",
      "data": [
        {
          "id": 1,
          "date": "2017-01-19",
          "time": "12:30:00",
          "duration": "02:00:00",
          "status": 1,
          "guests": 3,
          "shift_id": 1,
          "origin": 3,
          "customer_id": 1,
          "notes": "",
          "employee_id": null,
          "employeeUserName": "Widget",
          "booking_reference_id": null,
          "tables": "[1]",
          "isMailSent": false,
          "isOrderSent": false,
          "room_id": null,
          "cancelation_reason_id": null,
          "sentToRevo": null,
          "token": null,
          "total": "0.00",
          "worker_id": null,
          "order_id": null,
          "pre_assigned_tables": null,
          "payments": [
            {
              "id": 1,
              "name": "pepito",
              "amount": "12.50",
              "payment_method_id": 1,
              "booking_id": 1,
              "employee_id": 1
            }
          ],
          "products": [
            {
              "id": 1,
              "name": "Product 1",
              "product_id": 1,
              "revo_id": 1,
              "price": "12.50",
              "quantity": 1,
              "booking_id": 1,
              "employee_id": 1,
              "duration": null,
              "spacing": null,
              "type": null
            }
          ],
          "start": "2018-02-03 17:00:00"
          "end": "2018-02-03 19:00:00"
        },
        ...]
    }
```    
    

## Close Booking
Closes booking by Order ID
`https://revoflow.works/api/v1/bookings/closeWithOrder/{id}`

```
    {
      "ok": true,
      "message": "",
      "data": []
    }
```

Closes booking by ID
`https://revoflow.works/api/v1/bookings/close/{id}`

```
    {
      "ok": true,
      "message": "",
      "data": []
    }
``