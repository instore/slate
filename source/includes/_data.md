# Data Format

## AppliedDiscount

```shell
{
    "discountId": UUIDv4,
    "name": STRING,
    "totalDiscount": CURRENCY_STRING
}
```

AppliedDiscounts are supplied when you want to make the merchant aware that a coupon or discount was used.  This allows for things like reporting how many times a particular marketing campaign was effective.  They can be applied to either orders (10% off your order!), or specific order lines (buy one thing, get second thing at %50 off!).

### totalDiscount

Required.  Total amount discounted.

### name

Required. e.g. "happy hour -10% off", appears on receipt

### discountId

Optional. Identifier for an Instore discount.  Enables better reporting.

## AppliedModifier

```shell
{
    "modifierId": UUIDv4
    "name": STRING,
    "quantity": INTEGER,
    "sort": INTEGER,
    "unitPrice": CURRENCY_STRING,
}
```

An applied modifier is typically either a choice about a menu item (medium rare) or something extra you did to a menu item (add bacon).

### modifierId

Optional.  Identifier for an Instore modifier.  Enables more detailed reporting.

### name

Required.  Appears on receipt.  e.g. "Well done" or "Bacon"

### quantity

Optional.  Defaults to 1.  Appears on receipt when not 1.  e.g. "2&#215; bacon".

### sort

Required.  AppliedModifiers will be sorted by this (ascending) in all displays including the receipt.

### unitPrice

Required.  How much is charged for an unmodified one of these things.  The order line's price should have been raised by (unitPrice &#215; quantity) before any discounts or taxes are applied.

## AppliedTax

```shell
{
    "name": STRING,
    "amount": CURRENCY_STRING,
    "taxId": UUIDv4,
    "sort": INTEGER
}
```

### name

Required.  Customer facing.  e.g. "Sales Tax"

### amount

Required.  Total amount of tax (to be) collected.

### sort

Required.  Taxes will be sorted by this (ascending) in all displays including the receipt.

### taxId

Optional.  Identifier for an Instore tax.  Enables more detailed reporting.

## Order

```shell
{
    "address": {
        "streetOne": STRING,
        "streetTwo": STRING,
        "city": STRING,
        "stateOrRegion": STRING,
        "postalCode": STRING,
    }

    "customer": {
        "uniqueId": STRING,
        "email": STRING,
        "firstName": STRING,
        "lastName": STRING,
        "phone": STRING,
    }

    "customerFacingOrderNumber": STRING,
    "notes": STRING,
    "orderType": STRING,
    "orderDate": DATETIME,
    "orderSource": STRING,
    "orderTotal": CURRENCY_STRING,
    "uniqueId": STRING,

    "discounts": [ Array of AppliedDiscounts ],
    "orderLines": [ Array of OrderLines ],
    "payments": [ Array of Payments ],
    "taxes": [ Array of AppliedTaxes ]
}
```

### address

Required for delivery orders, otherwise optional.  The address to which the order will be delivered.  US ZIP codes are placed in the postalCode field.

### customer

Optional.  Information about the person that placed the order.

### customer.uniqueId

Optional.  An ID number used to identify this record across transactions.  Can be used to tie multiple orders to the same person over time.

### customer.phone

Optional.  Instore will strip all non numeral characters.

### customerFacingOrderNumber

Optional.  Will be printed on the receipt.  Any order identifier that was sent to the customer as part of the checkout process.  Useful in customer service situations as it allows the customer to identify the order over the phone.

### notes

Optional.  Is printed on receipt.  Any kind of high level notes e.g. "please knock on door around the side"

### orderType

Required.  One of [ 'forHere', 'delivery', 'takeout' ]

### orderDate

Required.  Date and time conforming to ISO 8601.  Used to determine age of order and to look order up by date in reporting.

### orderSource

Optional.  e.g. "Grubhub", helpful for customer service, understanding payment settlement, etc.

### orderTotal

Required.  Total owed by customer after nearly everything is considered including taxes, discounts, etc.  Basically: "the amount the customer owes for the order."  Does not include tips.

### uniqueId

Optional.  Similar to customerFacingOrderNumber, but not customer facing.  A good spot to put an integer or UUID that will uniquely identify the order for internal tracking purposes.

## OrderLine

```shell
{
    "unitPrice": CURRENCY_STRING,
    "itemId": UUIDv4,
    "itemName": STRING,
    "itemSizeId": UUIDv4,
    "itemSizeName": STRING,
    "note": STRING,
    "quantity": INTEGER,
    "sort": INTEGER,

    "discounts": [ See 'AppliedDiscount' ],
    "modifiers": [ See 'AppliedModifier' ],
    "taxes": [ See 'AppliedTax' ]
}
```

### unitPrice

Required.  How much is charged for a single unmodified one of these things before tax.  The order line's total price should have been (unitPrice &#215; quantity) before any discounts, taxes, or modifiers are applied.

### itemId

Optional.  Identifier for an Instore menu item.  Enables more detailed reporting.

### itemName:

Required.  Prints on receipts, kitchen tickets, etc. as the primary label for this order line e.g. "Blue Cheese Burger".

### itemSizeId:

Optional.  Identifier for an Instore menu item size.  Allows for more detailed reporting and is necessary for inventory counts to reflect this sale.

### itemSizeName

Optional.  Prints on receipt next to itemName as a subtitle.  Can be used for various things e.g. "medium" or "kilograms".

### note

Optional.  Prints next to this particular order line on receipts and kitchen tickets.  Can be used for special item instructions e.g. "hold the ketchup".

### quantity

Required.  

### sort

Required.  Order lines will be sorted by this (ascending) in all displays including the receipt.

## Payment

```shell
{
    "cardHolderFirstName": STRING,
    "cardHolderLastName": STRING,
    "cardMaskedPan": STRING,
    "tip": {
        "amount": CURRENCY_STRING,
    },
    "totalAmount": CURRENCY_STRING,
    "transactionData": STRING
}
```

The presence or absence of payments is how Instore determines whether an order is fully paid.  Fully paid orders should have an order.orderTotal equal to the sum of (payment.totalAmount - payment.tip.amount).

### cardHolderFirstName, cardHolderLastName

Optional.  Ideally the raw data from a card magnetic strip.  Should not be filled in with info from order.customer.

### cardMaskedPan

Optional.  Last 4 appears on receipt.  Best format has at least the first 4 and last 4 from the payment card with the rest obscured e.g. "4000••••••••1234".  Some payment processors obscure different numbers of digits, this is fine.  First 4 digits are used to determine card brand (Visa, etc.)  Last 4 are useful for identifying payments for customer service purposes.

### tip.amount

Optional.  Appears on receipt.

### totalAmount

Required.  Total amount charged on this payment.  Appears on receipt associated with the payment.  Includes tip amount.

### transactionData

Optional.  Used to store arbitrary data from the payment processor such as transaction numbers, result codes, etc.  This field often contains JSON.
