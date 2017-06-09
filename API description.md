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
| weight | Number | |
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

Get label:
```
GET https://senderella.io/v1/shipment/31afedef-5820-47a6-8b48-a0d55cc392ac/label?accessToken=my_key
```

Create request to PostNord Parcel

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
  }
}
```

Validate request

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
