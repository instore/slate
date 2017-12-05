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

Instore partner API docs.  v2.0

# A note on numbers

In most programming languages doing math with floating point numbers (floats, doubles, reals, etc) introduces small amounts of error over time.  
To avoid this problem the Instore API uses strings to pass any currency related numbers.  In the data specs you will see this referred to as a CURRENCY_STRING.

# Orders

## Create/submit an order

```shell
curl -X POST \
-H "api-key: your_key_here" \
-d @order.json
https://api-v2-staging.instoredoes.com/order
```

Used to provide a new order to Instore.  Can not be used to edit, only to create.

### HTTP Request

`POST https://api-v2-staging.instoredoes.com/order`

### POST body

See "Data Format -> Order"
