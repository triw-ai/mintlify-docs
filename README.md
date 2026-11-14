# Triw Documentation

Source for the Triw developer documentation, built with [Mintlify](https://mintlify.com).

Live site: `docs.triw.io` (after the Mintlify project points at this repository)

## Layout

```
docs.json            Mintlify config: navigation, languages, branding, contextual menu
index.mdx            English (default locale)
quickstart.mdx
authentication.mdx
api-keys.mdx
billing.mdx
models.mdx
errors.mdx
images.mdx
sdks/                openai · anthropic · gemini
tools/               claude-code · codex · cursor · cline · opencode · gemini-cli
                     hermes-agent · openclaw · deepseek-harness · cc-switch
id/                  Indonesian translations, identical paths
logo/                logo-light.png · logo-dark.png  (Triw app icon)
favicon.png
```

English pages sit at the repository root. Every Indonesian page lives under `id/` with the same relative path. A page path appears in exactly one language inside `docs.json`; listing the same path twice breaks the build.

## Local preview

```bash
npm i -g mint
mint dev
```

Serves on `http://localhost:3000`. Run `mint broken-links` before pushing a large change.

## Deploy

Pushes to `main` deploy automatically once the repository is connected to a Mintlify project:

1. Mintlify dashboard → **Add project** → connect GitHub → select `triw-ai/mintlify-docs`.
2. **Docs directory**: repository root (where `docs.json` lives).
3. Add the custom domain `docs.triw.io` in project settings and create the CNAME record it prints.

## Writing rules

- Every page starts with frontmatter: `title` and `description`. The description is what search results and link previews show.
- Code samples must run as pasted: real base URL (`https://router.triw.io`), `sk-...` placeholder key, correct path for the protocol.
- One concept per page. Sidebar order is the reading order for a new user, not an alphabetical list.
- Add new pages to **both** language blocks in `docs.json`, then create the `id/` counterpart.
- Client pages follow one shape: what it is → configure (exact field names) → verify → gotchas.

## Accuracy

The product is the source of truth. Before changing a factual claim, verify it against the gateway:

| Claim | Verify against |
| --- | --- |
| Endpoint paths | `router/relay-router.go` in the Triw repository |
| Error codes | a live error response from `https://router.triw.io` |
| Quota unit | `500,000 quota = $1` (`QuotaPerUnit` in `common/constants.go`) |
| Accepted auth headers | `middleware/auth.go` |
| Model availability | `GET /v1/models` with a real key — never hardcode a catalog |
| Prices | `https://router.triw.io/pricing` |

Model catalogs and prices are deployment data. The docs point readers at the live endpoints instead of copying values that go stale.
