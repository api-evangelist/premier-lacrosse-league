# Premier Lacrosse League

The Premier Lacrosse League (PLL) is the professional field lacrosse league of North America,
founded in 2018 by Paul and Mike Rabil and headquartered in El Segundo, California. It played its
inaugural season in 2019, absorbed Major League Lacrosse in December 2020, and now fields eight
city-affiliated clubs across an Eastern and a Western conference, broadcast on ESPN, ESPN2, ABC and
ESPN+.

- Website: https://premierlacrosseleague.com/
- Stats: https://stats.premierlacrosseleague.com/
- Shop: https://shop.premierlacrosseleague.com/
- GitHub: https://github.com/Premier-Lacrosse-League

## API surface

The league publishes **no developer portal, no OpenAPI and no API documentation**. Everything in
this repo was discovered by probing on 2026-08-02 and is recorded as observed:

| Surface | Endpoint | State |
|---|---|---|
| Web GraphQL API | `https://premierlacrosseleague.com/api/graphql` | Live Apollo Server. Origin-restricted, introspection disabled. Root fields `team` and `allTeams` confirmed. |
| Shop storefront JSON | `https://shop.premierlacrosseleague.com/collections/{handle}/products.json` | Public, unauthenticated, paginated. |
| Shop UCP MCP | `https://shop.premierlacrosseleague.com/api/ucp/mcp` | Live JSON-RPC 2.0 MCP server. `tools/list` gated on a UCP agent profile URI. |
| Shop OAuth / OIDC | `https://account.premierlacrosseleague.com/authentication/oauth/*` | RFC 8414 + OIDC discovery published, PKCE S256, 4 scopes. |

Not found anywhere on the estate: OpenAPI, AsyncAPI, webhooks, A2A agent card, `security.txt`,
status page, changelog, deprecation policy, SDKs, CLI, or a published compliance programme.

`robots.txt` carries `Content-Signal: search=yes,ai-train=no,use=reference` and disallows
Amazonbot, Applebot-Extended and Bytespider.
