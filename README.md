# datahub-skills

> Drop-in AI skill packages for [B2Bware DataHub](https://github.com/SyncSpider-GmbH/b2bware-documentationai-wiki) — usable from Claude Code, Cursor, GitHub Copilot CLI, and any other AI assistant that loads Markdown skills.

This repository ships three modular skills, built automatically from the public DataHub documentation:

| Skill | What it covers |
|---|---|
| **datahub-core** | Authentication (storefront vs server-to-server), filtering, includes, pagination (`per_page` / `total_pages`), pricing, media URLs, webhooks, idempotency, rate limiting, versioning, environments, error handling. Plus the AuthHub OpenAPI 3.0 spec. |
| **datahub-hubs** | Per-hub endpoint references for ProductHub, OrderHub, CustomerHub, AttributesHub, MediaHub, NotificationHub, TaxHub, SettingsHub, LicenseHub, AuthHub, EventHub, RuleHub. Plus all 12 OpenAPI 3.0 specs. |
| **datahub-storefront** | Headless storefront playbook — browsing the catalog, product detail, cart and checkout, customer account, post-purchase, configurable products, cart-time tax resolution. |

## Install

Skill packages are published as ZIP attachments on the [Releases](https://github.com/SyncSpider-GmbH/datahub-skills/releases) page. The `/releases/latest/download/` path always points at the most recent build.

```bash
# Claude Code
mkdir -p ~/.claude/skills && cd ~/.claude/skills \
  && curl -L https://github.com/SyncSpider-GmbH/datahub-skills/releases/latest/download/datahub-core.zip -o datahub-core.zip \
  && unzip -o datahub-core.zip && rm datahub-core.zip

# Cursor — adjust path to your assistant's skills directory
mkdir -p ~/.cursor/skills && cd ~/.cursor/skills \
  && curl -L https://github.com/SyncSpider-GmbH/datahub-skills/releases/latest/download/datahub-hubs.zip -o datahub-hubs.zip \
  && unzip -o datahub-hubs.zip && rm datahub-hubs.zip
```

Mix and match — install only the skill(s) you need.

## How It's Built

The skills are derived from `b2bware-documentationai-wiki` MDX sources. On every push to that repo's `main` branch, a `repository_dispatch` event triggers the workflow here, which:

1. Checks out the docs repo into `docs-source/`
2. Runs `scripts/build-public-skills.py --version <YYYY.MM.DD>`
3. Strips Mintlify-specific JSX (`<Callout>`, `<Tabs>`, `<Tab>`) to produce plain Markdown
4. Bundles per-skill references + OpenAPI specs
5. Publishes a GitHub Release tagged `skills-v<YYYY.MM.DD>` with the three ZIPs attached

The same script can be run locally — see `scripts/build-public-skills.py --help`.

## Versioning

Skills are date-versioned (`YYYY.MM.DD`) and tied 1:1 to the underlying docs commit they were built from. The release notes always link back to the originating commit in `b2bware-documentationai-wiki`.

## License

The skills inherit the license of the source documentation. See the [DataHub docs repository](https://github.com/SyncSpider-GmbH/b2bware-documentationai-wiki) for details.
