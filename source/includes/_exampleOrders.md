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
    "quantity": 1,
  }],
  "taxes": [{
    "amount": "$0.50"
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
  }
  "customer": {
      "uniqueId": "123456",
      "email": "example@example.com",
      "firstName": "Bob",
      "lastName": "Boberson",
      "phone": "555.555.5555"
  }
  "customerFacingOrderNumber": "Order 123-abc-Q",
  "locationId": "7a77834e-6bd6-4e64-a932-2ada8f2f1916",
  "notes": "Onion allergy, please no onions.",
  "orderType": "delivery",
  "orderDate": "2017-01-01T18:00:00Z",
  "orderSource": "foodwebsite.com",
  "orderTotal": "$3.78",
  "uniqueId": "d0cbd097-e82c-4614-81a1-dd6c37b5003e"
  "discounts": [{
      "totalDiscount": "-$0.40",
      "name": "%10 percent off Tuesdays",
      "discountId": "8245056d-9a14-4baf-b4c7-20ceab82096a"
  }],
  "orderLines": [{
      "itemId": "8298a33d-c4f5-43d9-bfe3-de8c1c682cfb",
      "itemName": "Dollar Burger",
      "itemSizeId": "2a9e748a-be58-4d35-8893-55c3c3b56f34",
      "itemSizeName": "Extra Huge",
      "note": "Ketchup on the side",
      "quantity": 2,
      "unitPrice": "$1.00",
      "discounts": [ ],
      "modifiers": [{
          "modifierId": "d16de64b-011a-45a7-929c-07717657ece4"
          "name": "Lettuce",
          "quantity": 1,
          "unitPrice": "$0.00",
      },
      {
          "modifierId": "1d256f5f-54a0-4ddc-9969-3328dfb05527"
          "name": "Tomato",
          "quantity": 1,
          "unitPrice": "$0.00",
      },
      {
          "modifierId": "3ccd8a77-ae68-4105-b573-fbee3fbad11d"
          "name": "Bacon",
          "quantity": 1,
          "unitPrice": "$1.00",
      }],
  },
  {
      "unitPrice": "$5.00",
      "itemId": "05ac1153-10b6-4a1c-a1ce-754dd6e665d3",
      "itemName": "Hat full o' fries",
      "itemSizeId": "4819f34f-a413-4b69-b710-9778048b7782",
      "itemSizeName": "Really big",
      "note": "Don't skimp on the salt!",
      "quantity": 1,
      "discounts": [{
          "totalDiscount": "$5.00"
          "name": "Free fries!"
          "discountId": "e029bf88-8378-4714-992e-8bdda89660c9"
      }],
      "modifiers": [{
          "modifierId": "4d3974c0-e05d-4fb6-a947-83ea92f7f518",
          "name": "Wedges",
          "unitPrice": "$0.00",
      }],
  }],
  "payments": [{
      "cardHolderFirstName": "Robert",
      "cardHolderLastName": "Bobberson",
      "cardMaskedPan": "4000********1234",
      "tip": {
          "amount": "$1.00",
      },
      "totalAmount": "$6.00",
      "transactionData": "{ \"someTransactionNumber\" : 1234, \"someBatchNumber\" : 4321 }"
  },
  {
      "cardHolderFirstName": "Roberta",
      "cardHolderLastName": "Bobberson",
      "cardMaskedPan": "4000********4321",
      "totalAmount": "$5.00",
      "transactionData": "234567"
  }],
  "taxes": [{
      "name": "%5 Sales Tax",
      "amount": "$0.18",
      "taxId": "85821765-be37-4771-a92d-a0bbf55fedbd",
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
