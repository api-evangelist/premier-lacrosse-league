# Premier Lacrosse League

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

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
