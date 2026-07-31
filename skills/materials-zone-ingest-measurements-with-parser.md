---
name: Ingest instrument measurements with a parser
description: Register a custom parser and attach instrument measurement files to items in MaterialsZone.
api: openapi/materials-zone-openapi.json
operations:
  - POST /parsers
  - GET /parsers
  - GET /parsers/{parserId}
  - POST /items/{itemId}/measurements
  - GET /files/{code}
---

# Ingest instrument measurements with a parser

Use this skill to turn raw instrument output into structured measurements on an item.

## Auth
Send the API key in the `authorization` header. Base URL: `https://api.materials.zone/v2beta1`.

## Steps
1. **Create a parser** — `POST /parsers`. Required fields: `name`, `physicalMeasurement`, `instrumentModel`, `instrumentManufacturer`, `viewType`, `supportedFileExtensions` (e.g. `["csv","xlsx","txt"]`). If unsure of `viewType`, start with `VIEW_2D_CUSTOM_AXES`. The parser is auto-assigned a `code` like `AB-CD-EF-25`.
2. **List / inspect parsers** — `GET /parsers` and `GET /parsers/{parserId}` to confirm configuration and enabled state.
3. **Attach a measurement** — `POST /items/{itemId}/measurements` referencing the parser (`parserCode`) and the raw file. The parser transforms the raw file into structured columns per its configuration.
4. **Retrieve files** — `GET /files/{code}` returns presigned `downloadURL`/`uploadURL` for measurement raw files.

## Rules
- `403` on parser operations means the API key lacks permission to manage parsers.
- Each parser code combination can be used up to 100 times per organization.
- JSON in/out; a `400` returns `{ code, message }` describing the invalid field.
