# Products API release notes

All notable changes to the Products API are listed here, newest first.

## Unreleased

### Breaking

- `sku` field is now required when you create a product with `POST /products`. Requests that omit `sku` will fail, so include a `sku` string in every create request. (#6)
- Authentication now uses an API key. Send your key in the `x-api-key` header on every request instead of HTTP basic authentication. Requests that still use basic auth will fail, so switch to the `x-api-key` header. (#7)
- `PUT /products/{productId}` is replaced by `PATCH /products/{productId}`. The endpoint updates only the fields you include in the request body. Change your update calls to use `PATCH`. (#7)
- `GET /products` now returns an object with `data`, `page`, `limit`, and `total` instead of an object with `message`. Read the products from the `data` array and use `page`, `limit`, and `total` to page through results. (#7)
- `PATCH /products/{productId}` requires at least one field in the request body. Requests with an empty body will fail, so include at least one field to update. (#7)
- `price` must now be 0 or greater when you create or update a product. Requests with a negative price will fail, so send a price of 0 or more. (#7)

### Added

- `sku` field on products. Returned by `GET /products/{productId}`, and accepted when you create a product with `POST /products` or update one with `PUT /products/{productId}`. Optional. (#5)
- `id` field on products. Returned by `GET /products`, `GET /products/{productId}`, `POST /products`, and `PATCH /products/{productId}`. It is assigned when you create the product and is read-only. (#7)
- `page` query parameter on `GET /products`. Use it to choose which page of results to return. Optional, defaults to 1. (#7)
- `400` response on `GET /products`, returned when the request is invalid. (#7)

### Changed

- `limit` query parameter on `GET /products` now defaults to 20 and accepts values from 1 to 100. (#7)
- Error responses (`400`, `401`, `404`, and `500`) now return a JSON body with `code` and `message` fields. (#7)

## 1.0.0

- Initial release of the Products API with endpoints to list, create, get, update, and delete products.
