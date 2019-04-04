# API basics

## Naming convention

### Endpoints

All endpoints are named with following schema:

`https://senderella.io/version/action` or `https://senderella.io/version/entity/action`

Currently only `v1` is a valid `version`. Words inside path string must be separated with `-` sign.

## Input

### Charset

Only `utf-8` charset is supported.

### Encoding

You can pass `gzip` or `deflate` inside `Accept-Encoding` header, or omit it.

`
Accept-Encoding: gzip
`

### Parameters

All parameters inside query string are notation indifferent. You can use either `camelCase`, `snake_case` or even `kebab-case`. Any of these notations are valid. Next in this document only camelCase will be used. Any unrecognized parameters (not described in this document) will throw validation error and `401` status code.

For GET requests parameters should be passed with GET parameters inside query string.

Example:

`https://senderella.io/v1/shipment/31afedef-5820-47a6-8b48-a0d55cc392ac/label?accessToken=my_key` to get a shipping label

For POST requests only `accessToken` is allowed inside GET parameters. Anything other must be stored inside request body.


### Accept data types

You can pass `Accept` HTTP header. Following types are valid:

* `text/html`
* `application/json`

Response will contain the same `Content-type` as request's `Accept`. When `text/html` is selected, output will be wrapped inside simple HTML page (This is useful for debug).

## Output

Currently only JSON and HTML output is supported. *XML might be added in the future*

### JSON

Each response is wrapped with object contained `code` and `data` fields. Payload is stored inside `data` field.
When everything is ok response body will be like this:

```js
{
  "data": [ // Here can be any payload
    { "id": 1, name: "Something" },
    { "id": 2, name: "Something other" }
  ]
}
```

When an error is occurred response body will be like this:

```js
{
  "code": "error", // Code is always "error"
  "error": { // errors
    "message": "Validation error"
  },
  "data": null // Delete data ?
}
```

## Authentication

Each request to API must be followed with special **authentication token**. Requests without it will be rejected with `401` status code.

There are some different ways to add your access token to request:

### X-Senderella-Api-Token header

You can pass your access key using **X-Senderella-Api-Token** header.

`
X-Senderella-Api-Token: YOUR_TOKEN
`

### accessToken with GET parameters

Just add `accessToken=YOUR_TOKEN` to GET params. For example:

`
/some-endpoint/?param1=X&param2=Y&accessToken=YOUR_TOKEN
`
