---
title: Instore API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
#  - JSON
#  - ruby
#  - python
#  - javascript

toc_footers:
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
- data
- exampleOrders
- errors

search: true
---

# Introduction

Instore partner API docs.  v1.0.6

# API docs changelog

* v1.0.0
* v1.0.1
    * Clairified section on currency strings.
    * Changes for clairity and correctness for incomingOrders endpoint.
    * Fixed formatting of JSON order examples.    
* v1.0.2
    * Fixed a few small errors in the curl examples.
* v1.0.3
    * Added incoming order status endpoint.
* v1.0.4
    * Removed instore id UUIDs from examples, as anyone trying to use those examples directly will get errors for not having matching menu items etc.
    * Fixed complex order example to add up correctly to the order total.
* v1.0.5
    * Clairified information on the orderDate field with examples in the correct format.
* v1.0.6
    * Added OrderLine.categoryName
    * Clarified that supplied names override names that can be inferred from ids (itemId, itemSizeId, etc.)

# A note on numbers and currency

In most programming languages doing math with floating point numbers (floats, doubles, reals, etc) introduces small amounts of error over time.  To avoid this problem the Instore API uses strings to pass any currency related numbers.  In the data specs you will see this referred to as a CURRENCY_STRING or a FLOAT_STRING.

Currently the Instore API only accepts amounts in US dollars.

Correct CURRENCY_STRING | Do not use
------------------------|----------
Numerals | More than two digits after the decimal point.
An optional decimal point |
Optional commas used as thousands separators |
Currency symbols such as $ are optional |

Correct examples:

* "12.34"
* "1,234.56"
* "1234.56"
* "0.89"

# Orders endpoints

## Create/submit an order

```shell
curl -X POST \
-H "api-key: [- your_key_here -]" \
-H "Content-Type: application/json" \
-d @order.json \
https://some.instore.server.com/incomingOrders
```

Used to provide a new order to Instore.  Can not be used to edit, only to create.

### HTTP Request

`POST https://some.instore.server.com/incomingOrders`

### POST body

See "Data Format -> Order"

## Check order status

```shell
curl -X GET \
-H "api-key: [- your_key_here -]" \
-H "Content-Type: application/json" \
https://some.instore.server.com/incomingOrders/summary/:locationId?date=YYYY-MM-DD
```

```shell
Example output
[
    {
        "customerFacingOrderNumber": null,
        "hasReachedIPad": true,
        "instoreId": "cb96ad8e-37dc-4fe8-8774-9fdcebba3d95",
        "instoreReceivedAt": "2017-01-01T19:58:20.025Z",
        "orderDate": "2017-01-01T18:00:00Z",
        "orderSource": null,
        "orderTotal": "$10.50",
        "uniqueId": null
    }
]
```

Will return a list of all of the orders submitted via the incomingOrders endpoint to a given location on a given day.  The day will be in the location's local time zone.

The "date" argument is optional and refers to the "instoreReceivedAt" field.  It defaults to "today".

### Example URL

`https://some.instore.server.com/incomingOrders/summary/48b63b2b-ca18-4634-8516-f464d008e646?date=2017-01-01`

### instoreId

A unique ID number assigned to all incoming orders by Instore.

### instoreReceivedAt

The date/time at which Instore received the order.  Typically the moment a POST call to /incomingOrders was processed.

### hasReachedIPad

This is true once the order has been delivered to the merchant's iPad.  Typically this means the order will have printed from a receipt printer.  If this value is false Instore is actively attempting to deliver the order to the iPad, which may be sleeping or out of Wi-Fi range etc.

Note that the order will not appear in Instore Office until this is true.

### customerFacingOrderNumber, orderDate, orderSource, orderTotal, uniqueId

Values taken from the submitted order itself.
