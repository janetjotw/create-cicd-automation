# Products API release notes

All notable changes to the Products API are listed here, newest first.

## Unreleased

### Added

- `sku` field on products. Returned by `GET /products/{productId}`, and accepted when you create a product with `POST /products` or update one with `PUT /products/{productId}`. Optional. (#5)

## 1.0.0

- Initial release of the Products API with endpoints to list, create, get, update, and delete products.
