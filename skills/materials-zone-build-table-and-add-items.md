---
name: Build a data table and add items
description: Create a folder and table in MaterialsZone, then add material records (items) with values.
api: openapi/materials-zone-openapi.json
operations:
  - POST /folders
  - POST /tables
  - POST /tables/{tableId}/items
  - POST /tables/{tableId}/items/batch
  - GET /tables/{tableId}/items
---

# Build a data table and add items

Use this skill to stand up a MaterialsZone data structure (Folder > Table > Items) and populate it.

## Auth
Every request sends the private API key in the `authorization` header. Base URL: `https://api.materials.zone/v2beta1`.

## Steps
1. **Create a folder** — `POST /folders` with `{ "title": "..." }`. Folders can nest (via `parentId`). Returns the folder `id`.
2. **Create a table** — `POST /tables` with `{ "title": "...", "folderId": "<folder id>" }`. Returns the table `id`.
3. **Add a single item** — `POST /tables/{tableId}/items` with the item payload (`title`, `values`). Each value references a `parameterId`.
4. **Add many items at once** — `POST /tables/{tableId}/items/batch` with a `BatchCreateItemsRequest` to ingest records in bulk.
5. **Read items back** — `GET /tables/{tableId}/items`. This response is paginated (see the paginate skill): loop on `pagination.endCursor` while `pagination.hasNextPage` is true.

## Rules
- Requests and responses are JSON.
- On `400` inspect the `{ code, message }` error envelope for the malformed field; `401` means a missing/invalid API key.
- Item values are typed by their `Parameter`; create/verify parameters via the protocol/parameter operations before ingesting values that reference new columns.
