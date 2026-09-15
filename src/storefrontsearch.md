# Storefront Catalog Curl Calls

## Settings

- MCP endpoint: `https://storefrontcatalog.anigok.com/mcp`
- Method: `POST`
- Header: `Content-Type: application/json`
- Header: `Accept: application/json, text/event-stream`
- Transport requirement: the current MCP handler requires both `application/json` and `text/event-stream` in the `Accept` header

## search_catalog

```bash
curl -X POST https://storefrontcatalog.anigok.com/mcp \
  -H "Content-Type: application/json" \
  -H "Accept: application/json, text/event-stream" \
  -d '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "tools/call",
    "params": {
      "name": "search_catalog",
      "arguments": {
        "shop_domain": "your-shop-domain.myshopify.com",
        "meta": {
          "ucp-agent": {
            "profile": "https://example.com/.well-known/ucp"
          }
        },
        "catalog": {
          "query": "organic coffee beans",
          "context": {
            "address_country": "US",
            "language": "en-US",
            "currency": "USD",
            "intent": "Customer prefers fair trade products"
          },
          "filters": {
            "available": true
          },
          "pagination": {
            "limit": 10
          }
        }
      }
    }
  }'
```

## search_catalog with unavailable items

```bash
curl -X POST https://storefrontcatalog.anigok.com/mcp \
  -H "Content-Type: application/json" \
  -H "Accept: application/json, text/event-stream" \
  -d '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "tools/call",
    "params": {
      "name": "search_catalog",
      "arguments": {
        "shop_domain": "your-shop-domain.myshopify.com",
        "meta": {
          "ucp-agent": {
            "profile": "https://example.com/.well-known/ucp"
          }
        },
        "catalog": {
          "query": "shirts",
          "filters": {
            "available": false
          }
        }
      }
    }
  }'
```

## lookup_catalog

```bash
curl -X POST https://storefrontcatalog.anigok.com/mcp \
  -H "Content-Type: application/json" \
  -H "Accept: application/json, text/event-stream" \
  -d '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "tools/call",
    "params": {
      "name": "lookup_catalog",
      "arguments": {
        "shop_domain": "your-shop-domain.myshopify.com",
        "meta": {
          "ucp-agent": {
            "profile": "https://example.com/.well-known/ucp"
          }
        },
        "catalog": {
          "ids": [
            "gid://shopify/Product/123",
            "gid://shopify/ProductVariant/456"
          ],
          "context": {
            "address_country": "US"
          }
        }
      }
    }
  }'
```

Response:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "structuredContent": {
      "ucp": {
        "version": "2026-08-25",
        "capabilities": {
          "dev.ucp.shopping.catalog.lookup": [{"version": "2026-08-25"}],
          "dev.shopify.catalog": [{"version": "2026-08-25"}]
        }
      },
      "products": [
        {
          "id": "gid://shopify/Product/101",
          "title": "Classic Blue Oxford Shirt",
          "description": {"html": "<p>Tailored cotton oxford shirt.</p>"},
          "price_range": {
            "min": {"amount": 7500, "currency": "USD"},
            "max": {"amount": 7500, "currency": "USD"}
          },
          "collections": [
            {
              "id": "gid://shopify/Collection/shirts",
              "handle": "shirts",
              "title": "Shirts",
              "description": {"html": "<p>Dress and casual shirts.</p>"}
            }
          ],
          "variants": [
            {
              "id": "gid://shopify/ProductVariant/201",
              "title": "Size M",
              "price": {"amount": 7500, "currency": "USD"},
              "availability": {"available": true},
              "requires": {"shipping": true},
              "checkout_url": "https://example.myshopify.com/cart/201:1",
              "inputs": [{"id": "gid://shopify/Product/101", "match": "featured"}]
            }
          ]
        },
        {
          "id": "gid://shopify/Product/102",
          "title": "$50 Gift Card",
          "gift_card": true,
          "price_range": {
            "min": {"amount": 5000, "currency": "USD"},
            "max": {"amount": 5000, "currency": "USD"}
          },
          "variants": [
            {
              "id": "gid://shopify/ProductVariant/202",
              "title": "$50 Gift Card",
              "price": {"amount": 5000, "currency": "USD"},
              "availability": {"available": true},
              "requires": {"shipping": false},
              "checkout_url": "https://example.myshopify.com/cart/202:1",
              "inputs": [{"id": "gid://shopify/Product/102", "match": "featured"}]
            }
          ]
        }
      ]
    }
  }
}
```

## get_product

```bash
curl -X POST https://storefrontcatalog.anigok.com/mcp \
  -H "Content-Type: application/json" \
  -H "Accept: application/json, text/event-stream" \
  -d '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "tools/call",
    "params": {
      "name": "get_product",
      "arguments": {
        "shop_domain": "your-shop-domain.myshopify.com",
        "meta": {
          "ucp-agent": {
            "profile": "https://shopify.dev/ucp/agent-profiles/2026-08-25/valid-with-capabilities.json"
          }
        },
        "catalog": {
          "id": "gid://shopify/Product/123",
          "selected": [
            {"name": "Color", "label": "Blue"}
          ],
          "context": {
            "address_country": "US"
          }
        }
      }
    }
  }'
```

Response:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "structuredContent": {
      "ucp": {
        "version": "2026-08-25",
        "capabilities": {
          "dev.ucp.shopping.catalog.lookup": [{"version": "2026-08-25"}],
          "dev.shopify.catalog": [{"version": "2026-08-25"}]
        }
      },
      "product": {
        "id": "gid://shopify/Product/123",
        "title": "Classic Blue Oxford Shirt",
        "description": {
          "html": "<p>Tailored cotton oxford shirt.</p>"
        },
        "price_range": {
          "min": {"amount": 7500, "currency": "USD"},
          "max": {"amount": 7500, "currency": "USD"}
        },
        "collections": [
          {
            "id": "gid://shopify/Collection/shirts",
            "handle": "shirts",
            "title": "Shirts",
            "description": {"html": "<p>Dress and casual shirts.</p>"}
          }
        ],
        "options": [
          {
            "name": "Color",
            "values": [
              {"label": "Blue", "available": true, "exists": true},
              {"label": "White", "available": true, "exists": true}
            ]
          },
          {
            "name": "Size",
            "values": [
              {"label": "S", "available": true, "exists": true},
              {"label": "M", "available": true, "exists": true},
              {"label": "L", "available": false, "exists": true}
            ]
          }
        ],
        "selected": [
          {"name": "Color", "label": "Blue"}
        ],
        "variants": [
          {
            "id": "gid://shopify/ProductVariant/201",
            "title": "Blue / Size M",
            "price": {"amount": 7500, "currency": "USD"},
            "availability": {"available": true},
            "requires": {"shipping": true, "selling_plan": false},
            "checkout_url": "https://example.myshopify.com/cart/201:1",
            "options": [
              {"name": "Color", "label": "Blue"},
              {"name": "Size", "label": "M"}
            ],
            "selling_plans": [
              {
                "id": "gid://shopify/SellingPlan/123",
                "name": "Subscribe & save 15%",
                "group": "Subscription",
                "recurring": true,
                "description": "Get this product delivered every month and save 15%.",
                "options": [
                  {"name": "Delivery", "value": "Every month"}
                ],
                "price": {
                  "type": "percentage",
                  "value": 15,
                  "currency": "USD"
                }
              }
            ]
          }
        ]
      }
    }
  }
}
```

## Storefront Catalog extension

The Storefront Catalog extension adds Shopify-specific fields to the base UCP catalog tools. This server implements the `dev.shopify.catalog` extension (version `2026-08-25`), which extends `dev.ucp.shopping.catalog.search` and `dev.ucp.shopping.catalog.lookup`.

This extension is scoped to a single merchant's storefront. For cross-merchant product discovery, use the Global Catalog MCP server.

### Product fields

When this extension is active, products in `search_catalog`, `lookup_catalog`, and `get_product` responses include:

| Field | Type | Description |
| - | - | - |
| `gift_card` | boolean | Whether this product is a gift card — a digital product, not a physical item. |
| `collections` | Array[Collection] | Merchant-curated collections this product belongs to. |

#### Collections

Each collection object contains:

| Field | Type | Description |
| - | - | - |
| `id` | string | Collection GID (e.g., `gid://shopify/Collection/123`). |
| `handle` | string | URL-safe slug for the collection. |
| `title` | string | Collection display name. |
| `description` | Description | Collection description in HTML format. |
| `url` | string | Canonical collection page URL (if available). |
| `media` | Array[Media] | Collection image (if available). |

### Variant fields

Variants include checkout prerequisites and purchase options:

| Field | Type | Description |
| - | - | - |
| `requires.shipping` | boolean | Whether a shipping address is needed. When `false`, checkout can skip address collection. |
| `requires.selling_plan` | boolean | Whether a selling plan must be selected. When `true`, include a `selling_plan_id` on the checkout line item. |
| `checkout_url` | string | Direct checkout URL for this variant. |
| `selling_plans` | Array[SellingPlan] | Available purchase options (subscriptions). Only on `get_product` responses — use `requires.selling_plan` in search to detect availability. |

#### Selling plans

Each selling plan includes:

| Field | Type | Description |
| - | - | - |
| `id` | string | Selling plan GID. |
| `name` | string | Plan display name (e.g., "Subscribe & save 15%"). |
| `group` | string | Selling plan group name. |
| `recurring` | boolean | Whether this plan involves recurring deliveries. |
| `description` | string | Optional plan description. |
| `options` | Array | Plan options with `name` and `value`. |
| `price` | object | Price adjustment with `type`, `value`, and `currency`. |

### Filters

When this extension is active, `search_catalog` and `lookup_catalog` accept an additional filter:

| Field | Type | Default | Description |
| - | - | - |
| `available` | boolean | `true` | When `true` (default), only sale-ready items are returned. Set to `false` to include unavailable items. |

```json
{
  "catalog": {
    "query": "shirts",
    "filters": {
      "available": false
    }
  }
}
```

## Notes

- The caller provides `shop_domain`.
- The Worker maps `shop_domain` to `https://{shop-domain}/api/ucp/mcp`.
- The caller provides `meta["ucp-agent"].profile` on every call.
- `search_catalog` accepts `query`, `context`, `filters.available`, and `pagination`. No cross-merchant filters (ships_to, ships_from, shops, price, condition, attributes, rating, price_tier, categories).
- `lookup_catalog` accepts `ids` (up to 10), `filters.available`, and `context`.
- `get_product` accepts `id`, `selected`, and `context`. No filters, no preferences, no view.
- `pagination.limit` is an integer, min 1, default 10, max 250.
- Storefront extension response fields (`gift_card`, `collections`, `requires.shipping`, `requires.selling_plan`, `selling_plans`, `checkout_url`) are returned by Shopify and passed through in `structuredContent`.
- Calls to this endpoint must include `Accept: application/json, text/event-stream`.
