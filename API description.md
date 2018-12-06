# Shipment API

## Structures

### ContactData

| Name                   | Type       | Description                             |
| ---------------------- | ---------- | --------------------------------------- |
| name<sup>*</sup> | String(35) | |
| address<sup>*</sup> | String(35) | |
| postalCode<sup>*</sup> | String(5) | |
| city<sup>*</sup> | String(35) | |
| countryCode<sup>*</sup> | String(2) | |
| attention | String(35) | |
| email<sup>*</sup> | String | |
| phoneNumber | String(35) | |
| mobilePhoneNumber | String(35) |  |
| customData | Object |  |

### Dimensions

Contains volume of the package or size of the box.

| Name                   | Type       | Description                             |
| ---------------------- | ---------- | --------------------------------------- |
| volume                 | Number     | m³                                      |

OR

| Name                   | Type       | Description                             |
| ---------------------- | ---------- | --------------------------------------- |
| width                  | Number     | m                                       |
| height                 | Number     | m                                       |
| depth                  | Number     | m                                       |

### Shipment

| Name                   | Type       | Description                             |
| ---------------------- | ---------- | --------------------------------------- |
| service<sup>*</sup> | Int | |
| shipmentOptions | Array | |
| requestOptions | ShipmentRequestOptions | |
| orderNumber | String | |
| shipmentNumber | String | |
| packages<sup>*</sup> | Array | Array of `Package` |
| sender<sup>*</sup> | ContactData | |
| receiver<sup>*</sup> | ContactData | |
| payer | ContactData | |
| pickUp | ContactData | |
| returnTo | ContactData | |
| pickupDate | Date | |
| pickupStart | Date | |
| pickupEnd | Date | |

### Package

| Name                   | Type       | Description                             |
| ---------------------- | ---------- | --------------------------------------- |
| content | String | |
| weight | Number | kg |
| volume | Dimensions | |
| goodsType | String | |
| items | Array | Array of `PackageItem` |

### PackageItem

| Name                   | Type       | Description                             |
| ---------------------- | ---------- | --------------------------------------- |
| senderRef | String | |
| receiverRef | String | |
| brand | String | |
| model | String(49) | |
| serial | String | |

### ShipmentRequestOptions

| Name                   | Type       | Description                             |
| ---------------------- | ---------- | --------------------------------------- |
| printLabel | LabelPrintingOptions | |
| bundlingMode | String | |

### LabelPrintingOptions

| Name                   | Type       | Description                             |
| ---------------------- | ---------- | --------------------------------------- |
| format | LabelFormat | |

### LabelFormat

Must be either `pdf`, `zpl` or `png`.

## Working Endpoints & Examples

### Get label:
```
GET https://senderella.io/v1/shipment/31afedef-5820-47a6-8b48-a0d55cc392ac/label?accessToken=my_key
```

### Create request to PostNord Parcel

```
POST /v1/shipment/submit?accessToken=my_key HTTP/1.1
Host: senderella.io
Accept: application/json
Content-Type: application/json
{
  "service": 1000,
  "shipmentOptions": {},
  "requestOptions": {
    "printLabel": {
      "format": "pdf"
    }
  },
  "packages": [
    {
      "weight": 1.5,
      "volume": {
        "volume": 0.1
      },
      "items": [
        {
          "senderRef": "item 1",
          "brand": "Apple",
          "model": "iPhone 7",
          "serial": "1234567890"
        }
      ]
    }
  ],
  "sender": {
    "name": "John Smith",
    "address": "Test st. 1",
    "postalCode": "35246",
    "city": "Växjö",
    "countryCode": "SE",
    "email": "test@test.test"
  },
  "receiver": {
    "name": "Sven Svensson",
    "address": "Receiver street 10",
    "postalCode": "589 41",
    "city": "Linköping",
    "countryCode": "SE",
    "email": "test@test.test"
  }
}
```

Output will be like this:
```
{
  "data": {
    "shpCsid": 179159,
    "packages": [
      {
        "packageId": 1,
        "packageNumber": "00370716483417464937"
      }
    ],
    "shipmentNumber": 0,
    "pickupId": null,
    "pickupDate": "2017-06-09T00:00:00",
    "senderellaId": "bec1f438-b4e0-4ad6-9266-d188c07de01f"
    "shipment": ...
  }
}
```

### Validate request

```
POST /v1/shipment/validate?accessToken=my_key HTTP/1.1
Host: senderella.io
Accept: application/json
Content-Type: application/json
{
  "service": 1000,
  "shipmentOptions": {},
  "requestOptions": {
    "printLabel": {
      "format": "pdf"
    }
  },
  "packages": [
    {
      "weight": 1.5,
      "volume": {
        "volume": 0.1
      },
      "items": [
        {
          "senderRef": "item 1",
          "brand": "Apple",
          "model": "iPhone 7",
          "serial": "1234567890"
        }
      ]
    }
  ],
  "sender": {
    "name": "John Smith",
    "address": "Test st. 1",
    "postalCode": "35246",
    "city": "Växjö",
    "countryCode": "SE",
    "email": "test@test.test"
  },
  "receiver": {
    "name": "Sven Svensson",
    "address": "Receiver street 10",
    "postalCode": "589 41",
    "city": "Linköping",
    "countryCode": "SE",
    "email": "test@test.test"
  }
}
```

### Get the shipment

```
GET /v1/shipment/c2a00938-99b5-4e87-aff5-8e67761423d1
content-type: application/json
accept: application/json
```
Output:

```js
{
  "data": {
    "createdAt": "2018-02-15T09:06:23.514Z",
    "deliveredByCarrierAt": null,
    "finalized": false,
    "guid": "c2a00938-99b5-4e87-aff5-8e67761423d1",
    "id": 3182,
    "packages": [
      {
        "guid": "1b02a162-a101-4945-acf6-607819297cf6",
        "items": [
          {
            "activityNumber": 123123,
            "brand": "LG",
            "guid": "559d770a-9dca-4edb-854d-4811ac42f8f7",
            "model": "test",
            "senderRef": "test1"
          }
        ]
      }
    ],
    "pickupDate": "2018-03-06",
    "receivedByCarrierAt": null,
    "receiver": {
      "address": "Låsblecksgatan 7",
      "city": "Linköping",
      "countryCode": "SE",
      "email": "kc.se@infocareworkshop.com",
      "mobilePhoneNumber": "0776700303",
      "name": "InfoCare CS AB",
      "phoneNumber": "0776700303",
      "postalCode": "58941"
    },
    "sender": {
      "address": "test",
      "city": "test",
      "countryCode": "SE",
      "email": "test@test.com",
      "mobilePhoneNumber": "12345670",
      "name": "test test",
      "phoneNumber": "12345670",
      "postalCode": "1234"
    },
    "shipmentData": {
      "packages": [
        {
          "packageId": 1,
          "packageNumber": "00370716483417837595"
        }
      ],
      "pickupDate": "2018-03-06T00:00:00",
      "pickupId": null,
      "shipmentNumber": 0,
      "shpCsid": 213828
    },
    "shipmentType": {
      "id": 1000,
      "name": "Post Nord Parcel"
    },
    "shipmentTypeId": 1000,
    "userId": 5
  }
}
```

### Find shipment items

```
POST /v1/shipment/items/find
content-type: application/json
accept: application/json
{
  "filters": {
    "brand ": "TEST"
  },
  "pagination": {
    "offset": 0,
    "limit": 1
  }
}
```

Output

```js
{
  "data": [
    {
      "brand": "TEST",
      "model": "test",
      "guid": "5ac3ef63-904c-45cc-bcf2-561f94606afa",
      "shipment": {
        "id": 3183,
        "guid": "6ca802d4-37bf-4419-a819-2f2ab928a2fc",
        "shipmentTypeId": 1000,
        "userId": 5,
        "shipmentType": {
          "id": 1000,
          "name": "Post Nord Parcel"
        },
        "sender": {
          "city": "test",
          "name": "test test",
          "email": "test@test.com",
          "address": "test",
          "postalCode": "1234",
          "countryCode": "SE",
          "phoneNumber": "12345670",
          "mobilePhoneNumber": "12345670"
        },
        "receiver": {
          "city": "Linköping",
          "name": "InfoCare CS AB",
          "email": "kc.se@infocareworkshop.com",
          "address": "Låsblecksgatan 7",
          "postalCode": "58941",
          "countryCode": "SE",
          "phoneNumber": "0776700303",
          "mobilePhoneNumber": "0776700303"
        },
        "pickupDate": "2018-03-06",
        "shipmentData": {
          "shpCsid": 213899,
          "packages": [
            {
              "packageId": 1,
              "packageNumber": "00370716483417838660"
            }
          ],
          "pickupId": null,
          "pickupDate": "2018-03-06T00:00:00",
          "shipmentNumber": 0
        },
        "packages": [
          {
            "guid": "8f87bcda-2add-4805-aea4-2900271d7e12",
            "items": [
              {
                "brand": "LG",
                "model": "test",
                "guid": "9330f1cf-8a1d-4245-8696-826c2db9064b"
              },
              {
                "brand": "TEST",
                "model": "test",
                "guid": "5ac3ef63-904c-45cc-bcf2-561f94606afa"
              }
            ]
          }
        ],
        "createdAt": "2018-02-15T14:17:29.375Z",
        "receivedByCarrierAt": null,
        "deliveredByCarrierAt": null
      }
    }
  ],
  "pagination": {
    "offset": 0,
    "count": 2
  }
}
```

### Edit shipment items

```
POST /v1/shipment/6ca802d4-37bf-4419-a819-2f2ab928a2fc/items
content-type: application/json
accept: application/json
```

```js
[
  // Add new item (no guid and package required)
  { 
    "brand": "Apple",
    "model": "iPhone",
    "package": "8f87bcda-2add-4805-aea4-2900271d7e12"
  },
  // Edit existing item (guid required, package is optional)
  { 
    "brand": "TEST",
    "model": "T1000",
    "customData": "info",
    "guid": "5ac3ef63-904c-45cc-bcf2-561f94606afa"
  }  
]
```

Output: modified shipment

Also you can add GET parameter `forced=1` to modify shipment that has been already finalized.

### Delete shipment items
```
DELETE /v1/shipment/c2a00938-99b5-4e87-aff5-8e67761423d1/items
content-type: application/json
accept: application/json

["5ac3ef63-904c-45cc-bcf2-561f94606afa"]
```

Output: modified shipment

### Edit packages

```
POST /v1/shipment/:shipmentGuid/packages
content-type: application/json
accept: application/json
```

```js
[
  { // Update existing package, guid required
    "guid": "<existing package guid>",
    "<package custom field1>": "<package custom value>",
    "<package custom field2>": "<package custom value>"
    // ...
    "items": [ // Optional list of items
      { // Modify existing item / move from another package
        "guid": "<existing item guid>",
        "<item custom field>": "<item custom value>"
        // ...
      },
      { // Create a new item
        "<item custom field>": "<item custom value>"
        // ...
      }
    ]
  },
  {} // Create a new empty package
]
```

Output: modified shipment

You must specify all custom package fields otherwise they will be removed.

You must specify all custom item fields otherwise they will be removed.

You can omit packages of shipment that don't require modification.

You can omit items of package that don't require modification.

Also you can add GET parameter `forced=1` to modify shipment that has been already finalized.


### Finalize shipment

```
POST /v1/shipment/c2a00938-99b5-4e87-aff5-8e67761423d1/finalize
content-type: application/json
accept: application/json
```

Output: modified shipment, `finalized` will be `true`

### Get picking list

```
GET /v1/shipment/c2a00938-99b5-4e87-aff5-8e67761423d1/picking-list
```

Parameters:

| Name                   | Type       | Description                             |
| ---------------------- | ---------- | --------------------------------------- |
| template | String | `default.html` is default |
| format | String | either `html` or `pdf`, `html` is default   |
| locale | String | locale, `sv-SE` is default   |

### Get barcode

```
GET /v1/barcode/{type}
```
| Name                   | Type       | Description                             |
| ---------------------- | ---------- | --------------------------------------- |
| type | String | |
| text | String | |
| scale | Int | Scaling factor |
| height | Int | Bar height, in millimeters. Default – 10 |
| includetext | Int | 1 or nothing |
| textxalign | String | Default – `center` |
