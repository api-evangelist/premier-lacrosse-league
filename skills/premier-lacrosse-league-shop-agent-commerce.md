---
name: Buy Premier Lacrosse League merchandise as an agent
description: >-
  Search, cart and check out on the Premier Lacrosse League's Shopify-hosted store using its
  Universal Commerce Protocol MCP endpoint, with the buyer-approval and rate-limit rules the store
  itself publishes.
api: mcp/premier-lacrosse-league-mcp.yml
surface: https://shop.premierlacrosseleague.com/api/ucp/mcp
operations:
  - search_catalog
  - create_cart
  - create_checkout
  - update_checkout
  - complete_checkout
generated: '2026-08-02'
method: generated
source: https://shop.premierlacrosseleague.com/llms.txt
---

# Buy Premier Lacrosse League merchandise as an agent

The Premier Lacrosse League publishes no developer API for scores or statistics. Its one
agent-callable transactional surface is the merchandise store, which implements the
[Universal Commerce Protocol](https://ucp.dev) over MCP. Every step below is grounded in the tool
names and rules the store publishes at `https://shop.premierlacrosseleague.com/llms.txt` and in the
capabilities advertised at `https://shop.premierlacrosseleague.com/.well-known/ucp`. No tool name or
argument is invented here — the input schemas live in the UCP shopping OpenRPC document referenced
by the merchant profile, and are not restated in this skill.

## Before you start

1. **Discover.** `GET https://shop.premierlacrosseleague.com/.well-known/ucp`. Confirm the store
   still advertises `dev.ucp.shopping` over transport `mcp` and note which UCP version you will
   speak (`2026-04-08` is current; `2026-01-23` is also supported).
2. **Present an agent profile.** The MCP endpoint gates on UCP agent identity, not a bearer token.
   An anonymous `tools/list` returns HTTP 422 with JSON-RPC error `-32001` /
   `invalid_profile_url` ("Unable to fetch agent profile: Missing profile uri"). You must supply a
   resolvable UCP agent profile URI before any tool call will be listed or executed.
3. **Read the tool schemas from the server**, not from this file: once your profile resolves, call
   `tools/list` and use the returned `inputSchema` for each tool.

## Flow

1. `search_catalog` — find products matching the buyer's stated intent. Pass
   `context.address_country` and `context.currency` so pricing and availability are correct for the
   buyer.
2. `create_cart` — create a cart containing the chosen items and variants.
3. `create_checkout` — open the purchase flow against that cart.
4. `update_checkout` — set the shipping address and shipping method. The store's fulfillment
   capability advertises `allows_multi_destination.shipping: false` and
   `allows_method_combinations: [["shipping"]]`, so plan for a single destination and a single
   shipping method per checkout.
5. `complete_checkout` — finalize. **Do not call this without explicit, contemporaneous buyer
   approval of the payment.** If you cannot obtain approval at the moment of payment, the store
   directs you to route the purchase through Shop Pay via `https://shop.app/SKILL.md` instead.

## Rules the store publishes

- **Human approval is mandatory at payment.** This is the store's stated invariant, not a
  recommendation.
- **Back off on 429.** The MCP endpoint is rate-limited per IP. No quota, window or rate-limit
  response headers are published, so use exponential backoff.
- **Discounts** are available as a capability (`dev.ucp.shopping.discount`) from UCP `2026-04-08`
  onward.

## Read-only alternative (no MCP, no identity)

If you only need catalog data and are not transacting, the store serves public JSON with no
authentication at all:

- `GET /collections/all/products.json?limit=250&page=N` — paginated product list. `limit` was
  observed to accept up to 250; `page` is 1-based. No cursor, no `Link` header, no total count.
- `GET /products/{handle}.json` — a single product.
- `GET /collections/{handle}/products.json` — one collection's products.
- `GET /search?q={query}&type=product` — storefront search.

Observed `Product` fields: `id`, `title`, `handle`, `body_html`, `published_at`, `created_at`,
`updated_at`, `vendor`, `product_type`, `tags`, `variants[]`, `images[]`, `options[]`.

## Error handling

Errors arrive as JSON-RPC 2.0 objects: `{"jsonrpc":"2.0","id":N,"error":{"code":<int>,
"message":"...","data":{"code":"...","content":"...","continue_url":"..."}}}`. Treat
`data.code` as the actionable code — `invalid_profile_url` means fix your agent profile, not retry.
See `errors/premier-lacrosse-league-problem-types.yml`.

## Do not

- Do not attempt to use the league's web GraphQL API (`https://premierlacrosseleague.com/api/graphql`)
  for stats. It is origin-restricted to the league's own properties, returns
  `{"error":"Origin not allowed"}` to third parties, has introspection disabled, and is not offered
  for third-party use.
- Do not train on league content. `robots.txt` sets `Content-Signal: search=yes,ai-train=no,use=reference`.
