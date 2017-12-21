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

Instore partner API docs.  v1.0.1

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

# Orders

## Create/submit an order

```shell
curl -X POST \
-H "api-key: your_key_here" \
-d @order.json
https://some.instore.server.com/incomingOrder
```

Used to provide a new order to Instore.  Can not be used to edit, only to create.

### HTTP Request

`POST https://some.instore.server.com/incomingOrder`

### POST body

See "Data Format -> Order"
