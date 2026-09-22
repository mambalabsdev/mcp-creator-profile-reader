# Influencer Profile Scraper MCP Server

[![Smithery](https://smithery.ai/badge/mambabuilt/mcp-creator-profile-reader)](https://smithery.ai/servers/mambabuilt/mcp-creator-profile-reader) [![Glama score](https://glama.ai/mcp/servers/mambalabsdev/mcp-creator-profile-reader/badges/score.svg)](https://glama.ai/mcp/servers/mambalabsdev/mcp-creator-profile-reader) [![MCP Registry](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fregistry.modelcontextprotocol.io%2Fv0%2Fservers%3Fsearch%3Dcom.mambabuilt%252Fmcp-creator-profile-reader%26limit%3D1&query=%24.servers%5B0%5D._meta%5B%22io.modelcontextprotocol.registry%2Fofficial%22%5D.status&label=mcp%20registry&color=blue)](https://registry.modelcontextprotocol.io/v0/servers?search=com.mambabuilt/mcp-creator-profile-reader&limit=1) [![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-creator-profile-reader)](https://www.npmjs.com/package/@mambalabsdev/mcp-creator-profile-reader) [![npm downloads](https://img.shields.io/npm/dm/@mambalabsdev/mcp-creator-profile-reader)](https://www.npmjs.com/package/@mambalabsdev/mcp-creator-profile-reader) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-creator-profile-reader)](https://github.com/mambalabsdev/mcp-creator-profile-reader/blob/main/LICENSE) [![mcpservers.org](https://img.shields.io/badge/mcpservers.org-listed-blue)](https://mcpservers.org/servers/mambalabsdev/mcp-creator-profile-reader)

MCP server for the Mamba Labs [Influencer Profile Scraper](https://apify.com/mambalabs/creator-profile-reader) actor on Apify.

Creator handles or profile URLs in, one profile row out per creator.

## Install

```bash
npx -y @mambalabsdev/mcp-creator-profile-reader
```

### Claude Desktop

```json
{
  "mcpServers": {
    "mamba-creator-profile-reader": {
      "command": "npx",
      "args": ["-y", "@mambalabsdev/mcp-creator-profile-reader"],
      "env": { "APIFY_TOKEN": "your-apify-token" }
    }
  }
}
```

Get an Apify token at [console.apify.com/account/integrations](https://console.apify.com/account/integrations).

## Tool

### `read_influencer_profiles`

Read influencer profiles by handle across seven platforms: followers, engagement, bio, and bio link.

| Input | Type | Required | Notes |
| --- | --- | --- | --- |
| `handles` | array | no | One per line. A profile URL on any supported platform (https://www.tiktok.com/@name, https://www.instagram.com/name/, https://www.youtube.com/@name, a Pinterest, Twitch, or Threads profile, an Apple Podcasts show. |
| `platforms` | array | no | Which platforms a bare @handle is looked up on. A full profile URL carries its own platform and ignores this. This actor does not search; pass the creators you want read. Supported: TikTok, Instagram, YouTube,. |
| `us_only` | boolean | no | Launch scope is US creators. A row whose country_guess is a known non US country is returned as an error row saying so. Rows with no country signal are kept. Uncheck to keep every country. Default `true`. |
| `escalate_on_block` | boolean | no | On by default. A profile fetch that comes back as a bot detection page is retried once over the residential proxy. On Instagram the bio, bio link, and following are read from the profile page over residential when. Default `true`. |
| `batch_size` | integer | no | Rows fetched at once. Leave empty for the measured per platform default; the measurement is in the README. Higher is faster and, above the measured point, loses rows. |
| `twitch_client_id` | string | no | Optional. Your own registered Twitch application client id. With `twitch_app_token` the Twitch reads use the official Helix API instead of the public web endpoint. Never a Mamba Labs credential. |
| `twitch_app_token` | string | no | Optional. An app access token for your Twitch client id (client credentials flow). Used only for Twitch reads, never stored or logged. |

Nothing is required. Influencer Profile Scraper answers a run with no usable input with a row carrying `row_status` and `error_reason` rather than failing, and the tool mirrors that.

## Pricing

Influencer Profile Scraper is pay per event on Apify. Every price below is flat across the FREE, BRONZE, SILVER, and GOLD tiers.

| Event | Charged for | Price | Fires when |
| --- | --- | ---: | --- |
| `actor-start` | Actor start | $0.001 | Once per run, on start. Covers the run overhead. |
| `profile-read` | Profile read | $0.006 | Once per profile row where the public profile page was read and at least the follower count or the bio came back. A private, missing, or blocked profile returns a labeled error row and does not charge. |
| `instagram-bio-fetch` | Instagram bio fetch | $0.01 | Once per Instagram profile row when the bio, bio link, and following were not on the embed widget or the datacenter API and the profile page was read over the residential proxy and came back readable. Only when escalate_on_block is on. Never on the embed or datacenter reads, never on another platform, never on a blocked page, and never on an error row. |

## Reading the output

Every row carries `row_status` and `error_reason`. A creator the actor could not read comes back as a row saying why, not as a gap in the list, so an absence is readable rather than inferred. Filter on `row_status` before loading a table.

## Actor

Actor ID `EqEnklDVMIcB1V499`. The wrapper calls the actor by that immutable ID rather than by its Store slug, so a Store rename never breaks it.

## Suite

| Actor | Actor ID | MCP server |
| --- | --- | --- |
| [Influencer Finder](https://apify.com/mambalabs/creator-finder) | `tpkedmloIIWtXx6sg` | [`@mambalabsdev/mcp-creator-finder`](https://www.npmjs.com/package/@mambalabsdev/mcp-creator-finder) |
| [Influencer Profile Scraper](https://apify.com/mambalabs/creator-profile-reader) | `EqEnklDVMIcB1V499` | [`@mambalabsdev/mcp-creator-profile-reader`](https://www.npmjs.com/package/@mambalabsdev/mcp-creator-profile-reader) |
| [Link in Bio Scraper and Newsletter Detector](https://apify.com/mambalabs/link-in-bio-newsletter-checker) | `OorucdheTIgu7RFzK` | [`@mambalabsdev/mcp-link-in-bio-newsletter-checker`](https://www.npmjs.com/package/@mambalabsdev/mcp-link-in-bio-newsletter-checker) |
| [Influencer Change Monitor](https://apify.com/mambalabs/creator-change-monitor) | `d2VVgahNL6UmcLkhg` | [`@mambalabsdev/mcp-creator-change-monitor`](https://www.npmjs.com/package/@mambalabsdev/mcp-creator-change-monitor) |
| [Influencer Lead List Builder](https://apify.com/mambalabs/creator-lead-list-all-in-one) | `KnmByszcv135yM30G` | [`@mambalabsdev/mcp-creator-lead-list-all-in-one`](https://www.npmjs.com/package/@mambalabsdev/mcp-creator-lead-list-all-in-one) |

Built by [Mamba Labs](https://mambabuilt.com).
