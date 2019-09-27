---
title: TOC API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - javascript

toc_footers:
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

search: true
---

# Introduction

API Documentation for Takeout Central's Restaurant App

<aside class="success">
Remember: API Examples are in JSON format but must be sent to the server as Content-Type: application/x-www-form-urlencoded
</aside>

# Login

## HTTP Request

```json
{ 
  "restLogin": "predefined username",
  "pass": "predefined password"
}
```

`POST https://takeoutcentral.com/restaurant-app/login`

| Parameter | Type   | Description               |
| --------- | ------ | ------------------------- |
| restLogin | string | Restaurant login username |
| pass      | string | associated password       |


## Return

> The above command returns JSON structured like this:

```json
{
  "restaurantID": "24",
  "sessionID": ""
}
```

| Parameter    | Type   | Description              |
| ------------ | ------ | ------------------------ |
| restaurantID | string | TOC ID for Restaurant    |
| sessionID    | string | TOC Defined API Auth Key |

<aside class="success">
sessionID's value needs to be used in each subsequent header's HTTP Request with the key "SESSIONID"
</aside>


# Get Orders

## HTTP Request

`GET https://takeoutcentral.com/restaurant-app/get-active-orders/<cartID>`

## Return

> The above command returns JSON structured like this:

```json
{
"Orders" : 
  [{ 
    "orderID": "65",
    "subtotal": "28.50",
    "tax":"2.14",
    "restaurantNotes" : "I'm vegan so please cook my food accordingly",
    "items": [
      {
        "whofor": "John",
        "itemid": "344514B",
        "price": "11.00",
        "qty": "1",
        "options": [
          {
            "optionid": "1552300",
            "price": " 2.50",
            "option": "Slab Bacon",
            "qty": "1"
          }
        ],
        "comments": "Testing special instructions",
        "item": "Apple & Bacon Bowl"
      },
      {
        "options": [
          {
            "option": "Veggie Burger",
            "qty": "1",
            "price": " 2.50",
            "optionid": "1552298"
          },
          {
            "option": "Egg",
            "qty": "1",
            "price": " 1.50",
            "optionid": "1552299"
          }
        ],
        "qty": "1",
        "item": "Apple & Bacon Bowl",
        "whofor": "Mike",
        "itemid": "344514A",
        "price": "11.00"
      }
    ]
  }]
}
```

| Parameter | Type  | Description                         |
| --------- | ----- | ----------------------------------- |
| Orders    | Array | Array of Order objects sorted by... |



### Orders


| Parameter       | Type   | Description                                              |
| --------------- | ------ | -------------------------------------------------------- |
| orderID         | string | TOC Defined order number from 00 to 99                   |
| items           | Array  | Array of Item objects, contains options and instructions |
| subtotal        | string | Total cost of the food in USD                            |
| tax             | string | Sales Tax for the food                                   |
| restaurantNotes | string | Notes for the restaurant as a whole                      |

### Item

| Parameter | Type   | Description                                                |
| --------- | ------ | ---------------------------------------------------------- |
| itemid    | string | TOC defined for item. Unique WRT the current cart          |
| qty       | string | quantity of current item in order                          |
| price     | string | price of item in USD                                       |
| item      | string | Human readable name of item                                |
| options   | Array  | Array of Option objects                                    |
| comment   | string | Special instructions for the item, this can be pretty long |
| whofor    | string | Name of the customer who ordered this                      |

### Option

| Parameter | Type   | Description                                            |
| --------- | ------ | ------------------------------------------------------ |
| optionid  | string | TOC defined id for option                              |
| qty       | string | quantity of option                                     |
| price     | string | price of option. About half the time this will be 0.00 |
| option    | Array  | Human readable name for the option                     |



# Confirm Order

## HTTP Request

`POST https://takeoutcentral.com/restaurant-app/confirm-order`

## Response

> Responses will look like this

```json
{
  "success": {
    "successMessage" : "0"
  }
}

```

Status 200 OK

| Parameter | Type   | Description                                  |
| --------- | ------ | -------------------------------------------- |
| success   | object | success object describing the success status |

### Success

| Parameter      | Type   | Description                 |
| -------------- | ------ | --------------------------- |
| successMessage | string | Success string. Usually "0" |



# Complete Order

## HTTP Request


`POST https://takeoutcentral.com/restaurant-app/order-ready`

## Response


> Responses will look like this

```json
{
  "success": {
    "successMessage" : "0"
  }
}

```

Status 200 OK

| Parameter | Type   | Description                                  |
| --------- | ------ | -------------------------------------------- |
| success   | object | success object describing the success status |

### Success

| Parameter      | Type   | Description                 |
| -------------- | ------ | --------------------------- |
| successMessage | string | Success string. Usually "0" |

