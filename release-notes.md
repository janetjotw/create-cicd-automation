# Products API release notes

All notable changes to the Products API are listed here, newest first.

## Unreleased

### Added
- `GET /popularity` endpoint. Returns a `breakdown` of `major` and `count` for the class you pass in the required `class` query parameter. (#3)
- `POST /majors` endpoint. Adds a major, using a `major` string in the request body. Requires bearer (JWT) authentication. (#3)

## 1.0.0

- Initial release of the Products API with endpoints to list, create, get, update, and delete products.
