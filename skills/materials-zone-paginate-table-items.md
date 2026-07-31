---
name: Page through all items in a table
description: Retrieve every item in a MaterialsZone table using cursor-based pagination.
api: openapi/materials-zone-openapi.json
operations:
  - GET /tables/{tableId}/items
---

# Page through all items in a table

Use this skill to export a complete table when it holds more items than one page.

## Auth
Send the API key in the `authorization` header. Base URL: `https://api.materials.zone/v2beta1`.

## Steps
1. **First page** — `GET /tables/{tableId}/items` (optionally `?pageSize=100`, the default). The response is `{ data: [...], pagination: { startCursor, endCursor, hasNextPage } }`.
2. **Collect** the `data` array into your result set.
3. **Next page** — if `pagination.hasNextPage` is `true`, repeat the request with `?cursor=<previous endCursor>`.
4. **Stop** when `pagination.hasNextPage` is `false`.

## Rules
- Omitting `cursor` always returns the first page.
- Keep `pageSize` reasonable (default 100) to bound response size.
- `401` means a missing/invalid API key; `400` returns `{ code, message }`.
