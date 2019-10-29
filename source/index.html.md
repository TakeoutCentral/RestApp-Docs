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

<aside class="success">
When testing, the endpoint should be directed to the subdomain https://scratch.takeoutcentral.com. Be sure that's what's in use before reporting errors. 
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

### Successful Login

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

### Incorrect Login

> This is the format and message for an invalid log in attempt

```json
{
  "error": {
    "errorType": "Invalid Credentials"
  }
}
```

Status 200 OK

| Parameter | Type   | Description                                    |
| --------- | ------ | ---------------------------------------------- |
| error     | object | error object describing the error that occured |

| Parameter | Type   | Description                                            |
| --------- | ------ | ------------------------------------------------------ |
| errorType | string | "Invalid Credentials" will always be the return value. |

# Onesignal Registration

```kotlin
import com.onesignal.OneSignal
.
.
.
OneSignal.sendTag("restID", restaurantID)
```

Upon a successful log in, the restaurantID will be returned to the client. At this point the app should register with OneSignal using the code seen to the right.

This will allow Takeout Central servers to send push notifications to any apps with that associated restaurantID tag.

<aside class='success'>
Further documentation for sendTag can be found <a href='https://documentation.onesignal.com/docs/android-native-sdk#section--sendtag-' target='blank' >here.</a>
</aside>

<aside class='success'>More info on integrating OneSignal can be found <a href='https://documentation.onesignal.com/docs/android-sdk-setup' target='blank'> here.</a>
</aside>

# Get Orders

## HTTP Request

`GET https://takeoutcentral.com/restaurant-app/get-active-orders/`

## Return

> The above command returns JSON structured like this:

```json
{
  "Orders": [
    {
      "orderNo": "65",
      "subtotal": "28.50",
      "tax": "2.14",
      "sendTime": "2019-09-15T15:53:00",
      "restaurantNotes": "I'm vegan so please cook my food accordingly",
      "orderID": "1KL91EUR2JI445D8VOD8L7HWQ1885",
      "status": "new",
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
    }
  ]
}
```

| Parameter | Type  | Nullable?                                                       | Description                         |
| --------- | ----- | --------------------------------------------------------------- | ----------------------------------- |
| Orders    | Array | Yes or No value specifying whether the value can be null or not | Array of Order objects sorted by... |

### Orders

| Parameter       | Type              | Nullable? | Description                                                                           |
| --------------- | ----------------- | --------- | ------------------------------------------------------------------------------------- |
| orderNo         | string            | No        | TOC Defined order number from 00 to 99                                                |
| orderID         | string            | No        | TOC Defined orderID string.                                                           |
| sendTime        | ISO-8601 DateTime | No        | Formatted Datetime for when the order was sent to the restaurant                      |
| items           | Array             | No        | Array of Item objects, contains options and instructions                              |
| subtotal        | string            | No        | Total cost of the food in USD                                                         |
| tax             | string            | No        | Sales Tax for the food                                                                |
| total           | string            | No        | subtotal + tax values in USD                                                          |
| restaurantNotes | string            | Yes       | Notes for the restaurant as a whole                                                   |
| status          | string            | No        | One of three values describing order state in order list: [new, confirmed, completed] |

### Item

| Parameter | Type   | Nullable? | Description                                                |
| --------- | ------ | --------- | ---------------------------------------------------------- |
| itemid    | string | No        | TOC defined for item. Unique WRT the current cart          |
| qty       | string | No        | quantity of current item in order                          |
| price     | string | No        | price of item in USD                                       |
| item      | string | No        | Human readable name of item                                |
| options   | Array  | No        | Array of Option objects                                    |
| comment   | string | Yes       | Special instructions for the item, this can be pretty long |
| whofor    | string | Yes       | Name of the customer who ordered this                      |

### Option

| Parameter | Type   | Nullable? | Description                                            |
| --------- | ------ | --------- | ------------------------------------------------------ |
| optionid  | string | No        | TOC defined id for option                              |
| qty       | string | No        | quantity of option                                     |
| price     | string | No        | price of option. About half the time this will be 0.00 |
| option    | Array  | No        | Human readable name for the option                     |

# Confirm Order

Confirming an order on the app moves it into the stage: 'In the Kitchen'.

## HTTP Request

> Requests need to contain a valid orderID

```json
{
  "orderID": "1KL91EUR2JI445D8VOD8L7HWQ1885"
}
```

`POST https://takeoutcentral.com/restaurant-app/confirm-order/`

## Response

> Responses will look like this

```json
{
  "success": {
    "successMessage": "0"
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

Completing an order on the app moves it into the stage: 'Ready for Pickups'.

## HTTP Request

> Requests need to contain a valid orderID

```json
{
  "orderID": "1KL91EUR2JI445D8VOD8L7HWQ1885"
}
```

`POST https://takeoutcentral.com/restaurant-app/order-ready/`

## Response

> Responses will look like this

```json
{
  "success": {
    "successMessage": "0"
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

# Responses

## Success

### Status Codes

| Status Code | Description                                                                                                                                |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| 200         | Every authenticated request recieved by the server and responded to without internal server error should return this                       |
| 401         | This will be returned if Session ID has been invalidated server side or deleted client side. App should return to log-in screen after this |
| 422         | This will be returned in the case of a client-side error such as invalid login credentials, missing orderID, or invalid formats            |

## Error

> Errors will look like this

```json
{
  "error": {
    "errorType": "Missing orderID"
  }
}
```

Nominal errors such as incorrect log-in credentials, incorrect orderIDs for confirm/complete will return with a status code of 200 but with an error object defined below.

Status: 422 Unprocessable Entity

| Parameter | Type   | Description                                    |
| --------- | ------ | ---------------------------------------------- |
| error     | object | error object describing the error that occured |

| Parameter | Type   | Description                                                                                 |
| --------- | ------ | ------------------------------------------------------------------------------------------- |
| errorType | string | String offering an explanation for error. Used in debugging but shouldn't be shown to users |

# Version Updates

> Responses will look like this

```json
{
  "minimum_supported_version": "1.0.0.1",
  "minimum_recommended_version": "1.7.0.1",
  "blocked_versions": ["1.1.1"]
}
```

`GET https://takeoutcentral.com/restaurant-app/supported_versions`

Status: 200 OK

| Parameter                   | Type         | Description                                                                                            |
| --------------------------- | ------------ | ------------------------------------------------------------------------------------------------------ |
| minimum_supported_version   | string       | minimum supported version. User can't use app if it's lower than this                                  |
| minimum_recommended_version | string       | recommended version. User can skip this and keep using app                                             |
| blocked_versions            | string Array | Array of versions that are blocked. Same functionality as if app was below the minimum_support_version |
