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
            "address_region": "NY",
            "postal_code": "10001",
            "language": "en-US",
            "currency": "USD",
            "intent": "Customer prefers fair trade products"
          },
          "filters": {
            "available": true,
            "ships_to": {
              "country": "US"
            },
            "ships_from": [
              {"country": "US"}
            ],
            "price": {
              "min": 5000,
              "max": 15000
            },
            "condition": ["new"],
            "price_tier": ["low", "medium"],
            "attributes": [
              {"name": "Color", "values": ["Black", "Blue"]},
              {"name": "Size", "values": ["10"]}
            ],
            "rating": {"variant": {"min": 4.5, "min_count": 10}}
          },
          "view": "offer",
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

## search_catalog with saved catalog

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
          "catalog_id": "01arz3ndektsv4rrffq69g5fav",
          "query": "organic coffee beans",
          "view": "offer",
          "pagination": {
            "limit": 20
          }
        }
      }
    }
  }'
```

## search_catalog with similarity

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
          "query": "trail running shoes",
          "like": [
            {"id": "gid://shopify/p/7f3a2b8c1d9e"}
          ],
          "filters": {
            "ships_to": {"country": "US"}
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
          "filters": {
            "available": true
          },
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
          "preferences": ["Color", "Size"],
          "context": {
            "address_country": "US"
          },
          "view": "summary"
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

## Similarity search

Use `catalog.like` in a `search_catalog` request to find products similar to a reference product, variant, or image. Pass one item as one of:

- **Item reference:** A product or variant GID. For example, `{"id": "gid://shopify/p/..."}`, `{"id": "gid://shopify/Product/..."}`, or `{"id": "gid://shopify/ProductVariant/..."}`.
- **Image content:** A base64-encoded image with its MIME type. For example, `{"image": {"content_type": "image/jpeg", "data": "<base64>"}}`.

You can combine `like` with `query` in a single request to narrow similarity results by keyword. When `like` contains an image and `query` is present, the catalog uses multimodal search. Multimodal search uses the text query to describe what the agent is looking for and the image to provide visual context, such as style, shape, or pattern. When `like` contains only an image, the catalog uses visual similarity search, which returns items that visually resemble the image without additional text intent.

```json
{
  "catalog": {
    "query": "trail running shoes",
    "like": [
      {"id": "gid://shopify/p/7f3a2b8c1d9e"}
    ],
    "filters": {
      "ships_to": {"country": "US"}
    }
  }
}
```

## Promoted placements

Promoted placements extend `search_catalog` with an optional paid-placement flow. When you pass a `catalog_id` for a saved catalog that has promoted placements enabled, Shopify blends promoted variants into the ranked results. You earn commission on attributed purchases when buyers click through using the variant `url` exactly as provided.

### How it works

- Pass `catalog.catalog_id` from an affiliate-enabled saved catalog on your `search_catalog` calls.
- Shopify determines whether to blend promoted placements into the results based on the catalog's server-managed configuration.
- Requests without an affiliate-enabled catalog return only organic variants.
- Callers who aren't approved receive organic variants and a `messages` note explaining they aren't authorized for promoted placements.

### Identifying promoted variants

Inspect each variant in the `search_catalog` response. A promoted variant includes a `placement` object. Organic variants omit it.

Promoted variant:

```json
{
  "id": "gid://shopify/ProductVariant/45012",
  "placement": {
    "type": "affiliate",
    "commission": {
      "percentage": {
        "value": 1.5
      }
    }
  }
}
```

Organic variant:

```json
{
  "id": "gid://shopify/ProductVariant/91823"
}
```

| Field | Type | Description |
| - | - | - |
| `variants[].placement` | object | Marks the variant as a promoted placement. Present only on promoted placements. |
| `variants[].placement.type` | string | The placement type. The well-known value is `"affiliate"`. |
| `variants[].placement.commission` | object | Describes a merchant-provided additional commission. Present only when a merchant offers additional commission. |
| `variants[].placement.commission.percentage.value` | number | The merchant-provided additional commission percentage, added to the base rate. |

### Preserving attribution

In an authorized response, all variant URLs include `shclid` and `shcgid` attribution parameters — whether the variant is promoted or organic. Unauthorized responses omit these parameters.

```text
https://{merchant_site}/products/{product_handle}?variant={v}&utm_source=shopify&utm_medium=catalog&shclid={click_id}&shcgid={catalog_id}
```

| Parameter | Description |
| - | - |
| `utm_source=shopify` | Identifies Shopify-sourced traffic for merchant attribution. |
| `utm_medium=catalog` | Identifies Global Catalog traffic. |
| `shclid` | Identifies the click for attribution. |
| `shcgid` | Identifies the developer's catalog for attribution and payout. |

Commissions are credited only when you send buyers through the variant `url` exactly as provided, with attribution parameters intact. Rerouting, masking the link behind your own domain, or altering parameters breaks attribution and disqualifies the conversion.

### Commission

- Base rate: 0.3% (30 bps) on attributed purchases.
- Commission applies to every item in the attributed order that's available through the Global Catalog, not only the clicked product.
- Conversions are attributed using a last-click methodology with a 7-day attribution window.
- When `placement.commission` is present, the merchant-provided percentage is added to the base rate.

### Disclosure

When you show promoted placements:

- Tell users that you may earn a commission, such as "We may earn a commission on purchases."
- Label promoted placements so users can tell them apart from organic variants.
- Don't bury the disclosure in a footer or privacy policy.
- Don't make false or misleading claims about products, prices, merchants, or Shopify.

These obligations include US FTC material-connection requirements and comparable advertising-transparency and sponsored-content rules in the EU, the UK, and other jurisdictions where you operate.

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
- `search_catalog` accepts `query`, `context`, `filters`, `view`, `like`, `catalog_id`, and `pagination`.
- `catalog.saved_catalog_slug` is a deprecated compatibility alias for `catalog.catalog_id`. Use `catalog.catalog_id` for new integrations. If both are passed, `catalog.catalog_id` takes precedence.
- `catalog.like` accepts an item reference or image content. Pass both `catalog.query` and an image for multimodal search; pass only an image for visual similarity search.
- `catalog.filters.available` defaults to true (only sale-ready items). Set to false to include unavailable items.
- `catalog.filters.ships_to` accepts country (ISO 3166-1 alpha-2), region, and postal_code.
- `catalog.filters.ships_from` accepts an array of country codes; multiple entries use OR logic. Digital products that don't require shipping can still match.
- `catalog.filters.price` accepts min and max integers in minor currency units. Example: `{"min": 5000, "max": 20000}` = $50.00–$200.00 USD.
- `catalog.filters.condition` known values: "new", "secondhand". Multiple values use OR logic.
- `catalog.filters.shops` accepts shop GIDs, e.g. `gid://shopify/Shop/987654321`. Up to 1000 shop IDs per request.
- `catalog.filters.attributes` supports Color, Size, and Target gender. Entries combine with AND logic; values within one entry combine with OR logic. Unsupported attribute names are ignored and returned in messages.
- `catalog.filters.rating.variant.min` is a 0–5 scale; `variant.min_count` is the minimum number of reviews.
- `catalog.filters.price_tier` supported values: low, medium, high. Multiple values use OR logic. Unsupported values are ignored and returned in messages.
- `catalog.filters.categories` accepts id (required) and taxonomy (optional, defaults to Shopify's standard taxonomy). Multiple values use OR logic.
- `catalog.view` — use "offer" for comparison shopping, "summary" for a condensed product detail view. When absent, the server returns its default shape.
- `catalog.pagination` is cursor-based. Pass the returned `pagination.cursor` as `catalog.pagination.cursor` to request the next page. `limit` is an integer, min 1, default 10, max 250.
- `total_count` in the response is an estimate of how many results match, not an exact count. Don't rely on it for precise totals or to calculate an exact number of pages.
- `lookup_catalog` requires `catalog.ids` — an array of up to 10 identifiers. Accepts `gid://shopify/Product/{id}` and `gid://shopify/ProductVariant/{id}`. Multiple IDs that resolve to the same product are grouped into a single product in the response.
- `lookup_catalog` response conforms to the UCP catalog lookup response, including products with inputs correlation on each variant and `not_found` messages for unresolved identifiers.
- `get_product` requires `catalog.id` — accepts `gid://shopify/Product/{id}` or `gid://shopify/ProductVariant/{id}`.
- `get_product` accepts `catalog.selected` (option selections for variant narrowing, e.g. `[{"name": "Color", "label": "Blue"}]`), `catalog.preferences` (option names in relaxation priority order — when an exact match isn't available, options are dropped from the end of this list first), `catalog.context`, and `catalog.view`.
- `get_product` response includes `product.selected` reflecting effective option selections, option values with `available` and `exists` signals, and variants matching the selection.
- Storefront extension response fields (`gift_card`, `collections`, `requires.shipping`, `requires.selling_plan`, `selling_plans`, `checkout_url`) are returned by Shopify and passed through in `structuredContent`.
- Promoted placements: when a `catalog_id` for an affiliate-enabled saved catalog is passed, promoted variants include a `placement` object and all variant URLs carry `shclid`/`shcgid` attribution params. Send buyers through the variant `url` exactly as provided to preserve attribution.
- Calls to this endpoint must include `Accept: application/json, text/event-stream`.
