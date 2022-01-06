# Quick Start Guide

## Overview

In this guide, we'll explore the Shopware Store API using different examples. You will learn how to:

* Authenticate as an API user
* Register and log in a customer
* Perform product searches and fetch product listings
* Use the cart
* Create an order
* Handle payment transactions

If you want to follow along this guide, it is useful to use an API client like Postman or Insomnia. You can also go along writing your custom script in Javascript, PHP or whatever programming language you're familiar with.

## General

The Store API has a base route which is always relative to your Shopware instance host. Note that it might differ from your sales channel domain. Let's assume your Shopware host is

```text
https://shop.example.com/
```

then your Store API base route will be

```text
https://shop.example.com/store-api
```

>  Before Shopware Version 6.4, the API version was passed within the endpoint path (e.g. `/store-api/v3/product`).

The Store API offers a variety of functionalities referred to as _endpoints_ or _nodes_, where each has their own route. The endpoints mentioned subsequently are always relative to the API base route.

### Authentication and setup

The Store API doesn't have a real authentication - it is a public API - just as any shop frontend is public to its visitors. However we have to pass some type of identification so Shopware is able to determine the correct sales channel for the API call. This identification is the `sw-access-key` . It is sent as a HTTP header. You can find the correct access key within your admin panel's sales channel configuration in a section labeled _API Access_

![API Access section in the Admin sales channel configuration](../../../assets/images/quick-start-guide-access-key.png)

A typical Store API request including headers will look like this - switch through the tabs to see the different parameters and add your own server URL and access token in the "Headers" tab.

```json http
{
  "method": "post",
  "url": "http://localhost/store-api/product",
  "headers": {
    "Content-Type": "application/json",
    "Accept": "application/json",
    "sw-access-key": "SWSCDZH3WULKMDGXTMNQS2VRVG"
  },
  "body": {
    "associations": {
      "media": {}
    },
    "includes": {
      "media": ["url"]
    }
  }
}
```

Now that we've authenticated we're able to perform our first request

### Fetch the context

The `/context` endpoint gives some general information about the store and the user. 

**Try it out**

```json http
{
  "method": "get",
  "url": "http://localhost/store-api/context",
  "headers": {
    "Content-Type": "application/json",
    "Accept": "application/json",
    "sw-access-key": "SWSCDZH3WULKMDGXTMNQS2VRVG"
  }
}
```

The response looks similar to this - 

```javascript
{
  "token": "jDUPcIRg1Mi7WZQJAm1nFTqhoMc0Eqev",
  "currentCustomerGroup": { ... },
  "fallbackCustomerGroup": { ... },
  "currency": { ... },
  "salesChannel": { ... },
  "taxRules": [ ... ],
  "customer": null,
  "paymentMethod": { ... },
  "shippingMethod": { ... },
  "shippingLocation": { ... },
  "rulesIds": [ ... ],
  "rulesLocked": false,
  "permissions": [ ... ],
  "permisionsLocked": false,
  "apiAlias": "sales_channel_context"
}
```

The full response is too big to be included in here - but we already have a lot of things to work with. For example the current sales channel, the selected currency or the customer group.

In a later section we'll also see how we can make changes to the context - e.g. changing the selected shipping method or the language.

But for now let's leave it here - as you can see above, the `customer` field is still empty. Time to change that.

> Next Chapter: **[Register a customer](register-a-customer.md)**