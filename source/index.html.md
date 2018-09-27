---
title: TOC API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - javascript

toc_footers:
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

search: true
---

# Introduction

API Documentation for TOC User Order Results page

# User

## Get User Order State

```javascript
  const socket = new WebSocket('wss://echo.websocket.org');

  // Handle any errors that occur.
  socket.onerror = error => console.log(`WebSocket Error: ${error}`);
  
  socket.onopen = event => console.log(`Connected to: ${event.currentTarget.url}`);
  
  // Handle messages sent by the server.
  socket.onmessage = event => console.log(event.data);

  // Show a disconnected message when the WebSocket is closed.
  socket.onclose = event => console.log(`Disconnected from WebSocket.`);
```

> The above command returns JSON structured like this:

```json
{
  "id": "JMSubs 10",
  "restaurants": [
    {
      "name": "Jersey Mikes",
      "icon": "/restlogos/rest20LogoOpen.gif",
      "latitude": "35.9328",
      "longitude": "-79.0260",
      "timeOfArrival": "2018-09-18 14:30",
      "stopNumber": "1"
    }
  ],
  "destination": {
    "latitude": "35.915160",
    "longitude": "-79.056470"
  },
  "status": {
    "ordered": "2018-09-18 12:30",
    "confirmed": "Null",
    "enRoute": "Null",
    "arrivedEstimate": "2018-09-18 15:30",
    "arrivedActual": "Null"
  }
}
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/toc/order`

### Return

Parameter | Type | Description
--------- | ------- | -----------
id | string | User Order ID
restaurants | array | Array of Restaurants
destination | object | Latitude/Longitude position
status | object | Order

### Restaurants

Parameter | Type | Description
--------- | ------- | -----------
stopNumber | string | Sort order
icon  | string | Restaurant's icon
latitude | string | Restaurant Location Information
longitude | string | Restaurant Location Information
timeOfArrival | datetime | Carrier's arrival time (UTC)

### Destination

Parameter | Type | Description
--------- | ------- | -----------
latitude | string | Customer's Location Information
longitude | string | Customer's Location Information

### Status

Parameter | Type | Description
--------- | ------- | -----------
ordered | datetime | Customer's ordered time  (UTC)
confirmed | bool | Order accepted
enRoute | bool | Carrier has begun route
arrivedEstimate | datetime | Customer's expected delivery time (UTC)
arrivedActual | datetime | Customer's actual delivery time

<aside class="success">
Remember — Websockets will update the state!
</aside>
