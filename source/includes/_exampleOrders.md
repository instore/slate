# Example Orders

## Simple Order

```shell
{
  "locationId": "7a77834e-6bd6-4e64-a932-2ada8f2f1916",
  "orderType": "takeout",
  "orderDate": "2017-01-01T18:00:00Z",
  "orderTotal": "$10.50",
  "orderLines": [{
    "unitPrice": "$10.00",
    "itemName": "Cheeseburger",
    "quantity": "1"
  }],
  "taxes": [{
    "amount": "$0.50",
    "name": "%5 Sales Tax"
  }]
}
```

A very basic order designed to show how simple things can be.  This is an unpaid order (it will be paid upon pickup) for a single item.

## Complex Order

```shell
{
  "address": {
      "streetOne": "123 Customer Street.",
      "streetTwo": "Unit A",
      "city": "Pizza Town",
      "stateOrRegion": "CA",
      "postalCode": "98765"
  },
  "customer": {
      "uniqueId": "123456",
      "email": "example@example.com",
      "firstName": "Bob",
      "lastName": "Boberson",
      "phone": "555.555.5555"
  },
  "customerFacingOrderNumber": "Order 123-abc-Q",
  "locationId": "48b63b2b-ca18-4634-8516-f464d008e646",
  "notes": "Onion allergy, please no onions.",
  "orderType": "delivery",
  "orderDate": "2017-01-01T18:00:00Z",
  "orderSource": "foodwebsite.com",
  "orderTotal": "$3.78",
  "uniqueId": "d0cbd097-e82c-4614-81a1-dd6c37b5003e",
  "discounts": [{
      "totalDiscount": "$0.40",
      "name": "%10 percent off Tuesdays"
  }],
  "orderLines": [{
      "categoryName": "Burgers",
      "itemName": "Dollar Burger",
      "itemSizeName": "Extra Huge",
      "note": "Ketchup on the side",
      "quantity": "2",
      "unitPrice": "$1.00",
      "discounts": [ ],
      "modifiers": [{
          "name": "Lettuce",
          "quantity": 1,
          "unitPrice": "$0.00"
      },
      {
          "name": "Tomato",
          "quantity": 1,
          "unitPrice": "$0.00"
      },
      {
          "name": "Bacon",
          "quantity": 1,
          "unitPrice": "$1.00"
      }]
  },
  {
      "unitPrice": "$5.00",
      "itemName": "Hat full o' fries",
      "itemSizeName": "Really big",
      "note": "Don't skimp on the salt!",
      "quantity": "1",
      "discounts": [{
          "totalDiscount": "$5.00",
          "name": "Free fries!"
      }],
      "modifiers": [{
          "name": "Wedges",
          "unitPrice": "$0.00"
      }]
  }],
  "payments": [{
      "cardHolderFirstName": "Robert",
      "cardHolderLastName": "Bobberson",
      "cardMaskedPan": "4000********1234",
      "tip": {
          "amount": "$1.00"
      },
      "totalAmount": "$2.78",
      "transactionData": "{ \"someTransactionNumber\" : 1234, \"someBatchNumber\" : 4321 }"
  },
  {
      "cardHolderFirstName": "Roberta",
      "cardHolderLastName": "Bobberson",
      "cardMaskedPan": "4000********4321",
      "totalAmount": "$2.00",
      "transactionData": "234567"
  }],
  "taxes": [{
      "name": "%5 Sales Tax",
      "amount": "$0.18"
  }]
}
```

This order has multiple complex order lines, and multiple payments.  Here's a walkthrough of calculating the order total.

Amount | Reason
-------|---------
  2.00 | Dollar Burger (1.00 x 2)
  2.00 | bacon on burger (1.00 x 2)
  5.00 | Hat o' Fries
- 5.00 | Free Fries discount (order line level)
------ | ------
  4.00 | Bill total
- 0.40 | %10 Tuesday's discount (order level)
  0.18 | %5 Sales tax (order level)
------ | ------
  3.78 | Order Total
