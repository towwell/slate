---
title: SSOWMS API Documentation

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='http://www.satsaco.com/'>Docs Maintained by Satsaco IT</a>
  - <a href='http://www.e-lacak.com'>API Created by Elacak team</a>
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true

code_clipboard: true

meta:
  - name: SATSACO WMS API Documentation
    content: Documentation for Tunas X Satsaco API
---

# Introduction

Welcome to the Satsaco WMS API Documentation! Our API is used to retrieve client parameters, create sales orders and retrieve statuses for orders/inventory. If any difficulties are encountered, please send an email to the IT team [here](mailto:it@satsaco.com) *(it@satsaco.com)*

We currently support RESTful APIs with POST and GET requests.

Version |  Date | Changelog
--------- |  ----------- | -----------
**1.1** | 30/12/2021 | Deployed API for retrieving of Shipment Order Status
1.01 | 1/12/2021 | Added UPC field for better searching on `inventory_status` API
1.0 | 15/11/2021 | Deployed Live API Endpoint


# Authentication / Endpoints

> For every request, headers must be as below:

```shell
# For every API request, headers must be similar to this format
curl "api_endpoint_here"
  -H "Content_type: application/json"
  -H "api-id: meowmeowmeow"
  -H "api-key: 9cDe3Hv6L2s21"
  -H "Accept: application/json"
```
We provide a staging & live environment account for API users and each are unique. Ensure that api-id and api-key matches the environment (live / staging)

**Live Environment:** `https://api.ssowms.com/api`

**Staging Environment:** `http://api.elacak.com/api`

To start integration with our system, please request for an <b>api-id</b> and <b>api-key</b> that is provided by the admin. The system expects for the credentials to be included in all API requests to the server in a header that looks like the following:

`api-id: meowmeowmeow`

`api-key: 9cDe3Hv6L2s21`

<aside class="notice">
You must replace <code>meowmeowmeow</code> and <code>9cDe3Hv6L2s21</code>with your personal api-id and api-key.
</aside>

# Client Parameters

## Client API Parameters


```shell
curl "https://api.ssowms.com/api/client/params" 
```
> The above link returns JSON information structured like this:

```json
{
    "request_id": "ZLNGO_20200120153904_478721",
    "success": true,
    "errors": null,
    "message": "Client setting and default values.",
    "return_value": {
        "timestamp": 1579534744,
        "business_model": {
            "list": [
                "shipping_order",
                "fulfillment"
            ],
            "default_value": "shipping_order"
        },
        "ordertype": {
            "list": [
                "dropship",
                "fastdispatch",
                "crossdock"
            ],
            "default_value": ""
        },
        "shippingtype": {
            "list": [
                "REG",
                "SDS",
                "NDS",
                "ECO"
            ],
            "default_value": "REG"
        },
        "servicetype": {
            "list": [
                "REG",
                "PREMIUM"
            ],
            "default_value": "REG"
        },
        "payment_method": {
            "list": [
                "NON-COD",
                "COD"
            ],
            "default_value": "NON-COD"
        },
        "logistic_provider": {
            "list": [
                {
                    "code": "tnc-pik",
                    "name": "TNC-PIK",
                    "is_active": true,
                    "address": "TNC-PIK"
                }
            ],
            "default_value": ""
        }
    }
}
```

This endpoint retrieves information of clients setup to assist in the formation of other requests.
<aside class="warning">
Please note that this is an advanced way of getting parameters for API requests.
</aside>

<aside class="notice"> 
For requests with 1-3 unsure parameters, please check with Satsaco IT regarding parameters you require instead
</aside>

### HTTP Request

`POST  /client/params`

### Return Parameters
Parameter |  Description
--------- |  -----------
business_model | Can be Shipping or Fulfilment with default value according to setup
ordertype | Used for determining shipping order type
shippingtype | Service level for Last Mile delivery
servicetype | Priority of Order when dropped to system
payment_method | Available Payment methods allowed for client with default value according to setup
logistic_provider | All available Logistics Provider available for client use


# Sales Order Creation
## Create a Sales Order
> Example Request Body:

```json
curl:"https://api.ssowms.com/api/order/create_sales"
{
    "orderid": "TRGAPIORDER001",
    "orderdate": 1588667092,
    "paymenttype": "NON-COD",
    "shippingtype": "REG",
    "insurance": "no",
    "shipment_provider": "ninjavan",
    "order_note": null,
    "sender_detail": {
        "addressee": "S.E.T Logistics",
        "address1": "5 Changi Districentre, Changi South Lane",
        "country": "Singapore",
        "postalcode": "486045",
        "phone": "65420171"
    },
    "receiver_detail": {
        "addressee": "Alice",
        "address1": "Yishun Street 72",
        "postalcode": "000000",
        "country": "Singapore",
        "phone": "90001000",
        "email": "test@mymail.com"
    },
    "items": [
        {
            "itemid": "123456780",
            "qty": 2,
            "uom": "pcs",
            "retailprice": 0,
            "sellingprice": 0
        },
        {
            "itemid": "4260521262332",
            "qty": 1,
            "retailprice": 100000,
            "sellingprice": 100000
        }
    ]
}

```

> Example Success Response:

```json
{
    "request_id": "id-liq-001_20200505112640_068823",
    "success": true,
    "errors": null,
    "message": "Order created successfully.",
    "return_value": {
        "timestamp": 1588678000,
        "order_id": "CVN_TEST_20200505_1",
        "reference_id": "LIQ_CVN_TEST_20200505_1-1"
    }
}

```

> Example Error Response (500):

```json
{
    "request_id": "id-liq-001_20200505112047_931793",
    "success": false,
    "errors": null,
    "message": "Internal Server Error. Please contact Support with given request_id.",
    "return_value": {
        "timestamp": 1588677647
    }
}
```

> Example Error Response (400):

```json
{
    "request_id": "TRNG_20211016041519_635504",
    "success": false,
    "errors": {
        "order": [
            "receiver_detail must be filled",
            "sender_detail must be filled",
            "shipment_provider must be filled",
            "shippingtype must be filled with REG / SDS / NDS / ECO",
            "orderid must be filled"
        ],
        "item": [
            "items must be filled"
        ]
    },
    "message": "Validation Error.",
    "return_value": {
        "timestamp": 1634357719
    }
}
```


This endpoint creates a Sales Order for fulfilment.</br>
There are multiple fields that can be left blank that will retrieve the data from client `default` instead

### HTTP Request

`POST /order/create_sales`

### Query Parameters

Parameter| Type | Description | Mandatory
--------| --------- | ----------- | -----
orderid | String | Sales Order ID | Yes 
orderdate | Number | Timestamp in UTC+0. `Default` is timestamp on endpoint hit | No 
paymenttype | String | Can be **'COD'** or **'NON-COD'**| Yes
codamount | Number | COD Amount | Yes **IF** payment type is COD 
shippingtype | String | Used for deciding service level for shipping.</br> `Default` is **'REG'** (1-3 Day delivery) </br> **'SDS'** - Same Day Delivery </br> **'NDS'** - Next Day Delivery </br> **'REG'** - Regular Delivery | Yes
shipment_provider | String | Last Mile Partner</br>*Please check with Satsaco IT for parameter*| Yes 
shipment_provider_awb | String | Airwaybill Number from last mile delivery. If filled, system will not request for a new airwaybill | No 
shipment_provider_awb_label | String | AWB Label URL for `shipment_provider_awb` | No
order_note | String | Sales Order Remarks / Comments | No
insurance | String | Can be **'yes'** or **'no'**. `Default` is **'no'** | No
sender_detail | Object | Refer to [Sender Detail Object](#sender-detail-object) | Yes
receiver_detail | Object | Refer to [Receiver Detail Object](#receiver-detail-object) | Yes
items | Array of Objects | Array of Item Objects. Refer to [Item Object](#item-object) | Yes

### Sender Detail Object
<aside class="notice"> 
For sender & receiver objects, province, city, district and subdistrict is on a need to use basis depending on the country.</br> For Singapore, no extra fields are used </br> For Malaysia, province and city are mandatory
</aside>
Parameter| Type | Description | Mandatory (SG) | Mandatory (MY)
--------| --------- | ----------- | ------------- | ---------------
addressee | String | Sender name | Yes | Yes
address1 | String | Sender address | Yes | Yes
address2 | String | Additional sender address info | No | No
province | String | Address Province / State | No | **Yes**
city | String | Address City | No | **Yes**
district | String | Address District | No | No
subdistrict | String | Address Sub-District | No | No
postalcode | String | Address Postal Code | Yes | Yes
country | String | Sender Country | Yes | Yes
phone | String | Sender Phone number | Yes | Yes
email | String | Sender Email | No | No

### Receiver Detail Object
Parameter| Type | Description |  Mandatory (SG) | Mandatory (MY)
--------| --------- | ----------- | ------------- | ---------------
addressee | String | Receivers name | Yes | Yes
address1 | String | Shipment address | Yes | Yes
address2 | String | Additional receiver address info | No | No
province | String | Address Province / State | No | **Yes**
city | String | Address City | No | **Yes**
district | String | Address District | No | No
subdistrict | String | Address Sub-District | No | No
postalcode | String | Address Postal Code | Yes | Yes
country | String | Receiver Country | Yes | Yes
phone | String | Receiver Phone number | Yes | Yes
email | String | Sender Email | No | No

### Item Object
Parameter| Type | Description | Mandatory
--------| --------- | ----------- | -----
itemid | String | Item ID or SKU | Yes **IF** itemupc is empty
itemupc | String | Item UPC Code | Yes **IF** itemid is empty
uom | String | Unit of Measurement of Item. `Default` is **pcs** | No
batchnumber | String | Batch number | No
qty | Number | Quantity of item | Yes
unitretailprice | Number | Unit retail price of item | No
unitsellingprice | Number | Unit selling price of item | No

# Retrieve Status

## Retrieve Fulfilment Status

```json
curl "https://api.ssowms.com/api/order/sales_track" \
{
    "order_ids": [
        "TEST001","TEST002"
    ]
}
```

> Example Response:

```json
{
    "request_id": "9_20211016042326_404806",
    "success": true,
    "errors": [
        {
        "order_id": "TEST002",
        "error": "Order id not found."
        }
    ],
    "message": "Fetch status success.",
    "return_value": {
        "timestamp": 1634358206,
        "record_found": 1,
        "record_details": [
            {
                "order_id": "TEST001",
                "reference_id": "SD_TEST001-1",
                "awb_no": "",
                "status": "RETURN"
            }
        ]
    }
}

```

This endpoint retrieves fulfilment status of order IDs provided

### HTTP Request

`POST /order/sales_track`

### Query Parameters

Parameter| Type | Description | Mandatory
--------| --------- | ----------- | -----
order_ids | Array | Array of order ids, max 100 order ids in 1 request | Yes

### Response Parameters

Status | Description
---------- | -------------
OPEN | Order had just been created
NONE_ALLOCATED | Order Items have no available stock
PARTIAL_ALLOCATED | Order Items have some available stock
PENDING_FULFILLMENT | Order Fulfillment is being processed
CLOSE | Order Fulfillment is on hold
CANCEL | Order is cancelled
INTRANSIT | Order has been handovered to Shipment Distribution
OUT_FOR_DELIVERY | Order package is on delivery by Shipment Distribution Courier
DELIVERED | Order Delivered
FAILED_TO_DELIVER | Order Failed to deliver
RETURN | Order is returned


## Retrieve Shipment Status

```json
curl "https://api.ssowms.com/api/track/awb_no?=TRP-030201" \
{
}
```

> Example Response:

```json
{
    "request_id": "TEST_20211230072638_090039",
    "success": true,
    "errors": null,
    "message": "Order history retrieved.",
    "return_value": {
        "timestamp": 1640849198,
        "awb": "TRP-030201",
        "reference_id": "TRP-030201",
        "client_legal_name": "TEST_CUSTOMER",
        "history": [
            {
                "step": "order_received",
                "status": "complete",
                "datetime": "2021-12-10 07:05:40",
                "datetime_timestamp": 1639094740,
                "note": "AWB : TRP-030201"
            },
            {
                "step": "inbound",
                "status": "complete",
                "datetime": "2021-12-10 07:05:40",
                "datetime_timestamp": 1639094740,
                "note": ""
            },
            {
                "step": "intransit",
                "status": "intransit",
                "datetime": "2021-12-10 07:05:40",
                "datetime_timestamp": 1639094740,
                "note": "SG Hub SET Logistics"
            },
            {
                "step": "intransit",
                "status": "intransit",
                "datetime": "2021-12-10 07:05:40",
                "datetime_timestamp": 1639094740,
                "note": "SG Hub SET Logistics"
            },
            {
                "step": "delivery",
                "status": "success",
                "datetime": "2021-12-11 13:09:47",
                "datetime_timestamp": 1639202987,
                "note": null
            }
        ]
    }
}

```

### HTTP Request

`POST /track/awb_no?=`

### Query Parameters
Parameter| Type | Description | Mandatory
--------| --------- | ----------- | -----
awb_no | String | Only 1 AWB No at a time. Used in endpoint | Yes

### Response Parameters
Status | Description
---------- | -------------
order_received | Shipment Order recieved in system
inbound | Shipment Order is pending pickup
intransit | Shipment Order has been handed over
out_for_delivery | Shipment Order is being delivered by 3PL
delivery | Shipment Order has been successfully delivered
failed_delivery | Shipment Order has failed delivery

# Retrieve Inventory

## Retrieve Inventory Status

```json
curl "https://api.ssowms.com/api/client/inventory_status" \
{
    "skus": [
        "4029054335537","88888888881"
    ],
    "upcs": []
}
```

> Example Response:

```json
{
    "request_id": "9_20211023044533_077354",
    "success": true,
    "errors": null,
    "message": "Fetch status success.",
    "return_value": {
        "timestamp": 1634964333,
        "record_found": 2,
        "record_details": [
            {
                "sku": "4029054335537",
                "upc_1": "PSO20019",
                "description": "1-Carl 10221152 01, 00, 402, 44",
                "quantity_onhand": 15,
                "quantity_available": 12,
                "quantity_commited": 0,
                "quantity_backorder": 0,
                "quantity_onshelf": 15
            },
            {
                "sku": "88888888881",
                "upc_1": "SND0039",
                "description": "ICE CREAM 1",
                "quantity_onhand": 0,
                "quantity_available": 0,
                "quantity_commited": 0,
                "quantity_backorder": 0,
                "quantity_onshelf": 0
            }
        ]
    }
}
```

This endpoint retrieves inventory quantity for SKUs requested

### HTTP Request

`POST /client/inventory_status`

### Query Parameters

Parameter| Type | Description | Mandatory
--------| --------- | ----------- | -----
skus | Array | Array of skus, max 100 skus in 1 request | Yes **IF** upcs is empty. Leave empty array if unused
upcs | Array | Array of upcs, max 100 upcs in 1 request | Yes **IF** skus is empty. Leave empty array if unused

### Response Parameters

Quantity Type | Description
---------- | -------------
quantity_onhand | Quantity currently in warehouse and not handed over
quantity_available | Quantity available for fulfilment
quantity_committed | Quantity assigned to sales orders but picking has not started
quantity_backorder | Quantity that is not available and needed for order fulfilment
quantity_onshelf | Quantity that is currently physically on shelving (Not picked yet)
quantity_onfulfill | Quantity that is reserved for order but fulfilment process has not started