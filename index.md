# Shipment API

## Structures

### ContactData

| Name                   | Type       | Description                             |
| ---------------------- | ---------- | --------------------------------------- |
| name | String(35) | |
| address | String(35) | |
| postalCode | String(5) | |
| city | String(35) | |
| countryCode | String(2) | |
| attention? | String(35) | |
| email | String | |
| phoneNumber? | String(35) | |
| mobilePhoneNumber? | String(35) |  |

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
| service | Int | |
| shipmentOptions? | Array | |
| requestOptions? | ShipmentRequestOptions | |
| orderNumber? | String | |
| shipmentNumber? | String | |
| packages | Array | Array of `Package` |
| sender | ContactData | |
| receiver | ContactData | |
| payer? | ContactData | |
| pickUp? | ContactData | |
| returnTo? | ContactData | |
| pickupDate? | Date | |
| pickupStart? | Date | |
| pickupEnd? | Date | |

### Package

| Name                   | Type       | Description                             |
| ---------------------- | ---------- | --------------------------------------- |
| content? | String | |
| weight? | Number | |
| volume? | Dimensions | |
| goodsType? | String | |
| items? | Array | Array of `PackageItem` |

### PackageItem

| Name                   | Type       | Description                             |
| ---------------------- | ---------- | --------------------------------------- |
| senderRef? | String | |
| receiverRef? | String | |
| brand? | String | |
| model? | String(49) | |
| serial? | String | |

### ShipmentRequestOptions

| Name                   | Type       | Description                             |
| ---------------------- | ---------- | --------------------------------------- |
| printLabel? | LabelPrintingOptions | |
| bundlingMode? | String | |

### LabelPrintingOptions

| Name                   | Type       | Description                             |
| ---------------------- | ---------- | --------------------------------------- |
| format | LabelFormat | |

### LabelFormat

Must be either `pdf`, `zpl` or `png`.

## Working Endpoints & Examples

Get label:
```
GET /v1/shipment/31afedef-5820-47a6-8b48-a0d55cc392ac/label?accessToken=my_key
```

Create request to PostNord Parcel

```
POST /v1/shipment/submit?accessToken=my_key
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
          "model": "iPhone 7+",
          "serial": "1234567890"
        }
      ]
    }
  ],
  "sender": {
    "name": "John Smith",
    "address": "Test st. 1",
    "postalCode": "1234",
    "city": "Vaxjo",
    "countryCode": "SE",
    "email": "john@test.com"
  },
  "receiver": {
    "name": "Sven Svensson",
    "address": "Other st. 2",
    "postalCode": "5678",
    "city": "Linkoping",
    "countryCode": "SE",
    "email": "sven@test.com"
  }
}
```

Output will be like this:
```
{
  "data": {
    "shpCsid": 178798,
    "packageId": 1,
    "shipmentNumber": 0,
    "pickupId": null,
    "pickupDate": "2017-06-06T00:00:00",
    "senderellaId": "084aa0d8-6bb5-40aa-80ca-fc9da642a9bc"
  }
}
```

Validate request

```
POST /v1/shipment/validate?accessToken=my_key
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
          "model": "iPhone 7+",
          "serial": "1234567890"
        }
      ]
    }
  ],
  "sender": {
    "name": "John Smith",
    "address": "Test st. 1",
    "postalCode": "1234",
    "city": "Vaxjo",
    "countryCode": "SE",
    "email": "john@test.com"
  },
  "receiver": {
    "name": "Sven Svensson",
    "address": "Other st. 2",
    "postalCode": "5678",
    "city": "Linkoping",
    "countryCode": "SE",
    "email": "sven@test.com"
  }
}
```

