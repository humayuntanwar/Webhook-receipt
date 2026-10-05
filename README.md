# Webhook Receipt

Paste a webhook’s headers + raw body. Get parsed fields, a signature-header checklist, and a copyable `curl` replay stub — all in the browser.

**Author:** Humayun Tanwar

Built for FDE / Solutions Engineering work when a Stripe, Firebase, Svix-style, GitHub, or generic JSON webhook fails in a customer environment and you need to see what actually arrived before blaming the handler.

## Why

Customer webhooks break for boring reasons: missing signing headers, wrong event shape, retries you already processed. This tool helps you inspect a receipt in under a minute without pasting secrets into someone else’s SaaS.

## Features

- Paste **headers** and **raw body** (JSON)
- **Auto-detect** provider: Stripe, Firebase / Google, Svix / Clerk / Resend-style, GitHub, or generic
- **Parsed summary** + field chips for common `id` / `type` / `event` keys
- **Signature checklist** — which headers are present and what to verify (presence + guidance only; no HMAC cracking, no secret upload)
- **Copyable `curl`** stub aimed at your localhost URL
- Sample loaders for Stripe and generic JSON
- **100% client-side** — nothing is uploaded

## Run locally

```bash
cd webhook-receipt
open index.html          # macOS
xdg-open index.html      # Linux

# or serve statically
python3 -m http.server 8765
# → http://localhost:8765
```

No build step. No dependencies. Deploy as static files (Vercel / Netlify / GitHub Pages).

## How it works

1. Headers are parsed into a case-insensitive map.
2. Body is parsed as JSON when possible.
3. Provider is chosen from your preset or inferred from headers / body shape.
4. Checklist items are rendered for that provider (e.g. `Stripe-Signature`, `X-Hub-Signature-256`).
5. A `curl` command is built from the same headers + body for local replay.

This does **not** validate signatures cryptographically. Keep your webhook secrets out of the paste box.

## Supported providers

| Provider | Detection / checklist focus |
| --- | --- |
| Stripe | `Stripe-Signature`, event `id` / `type` |
| GitHub | `X-Hub-Signature-256`, `X-GitHub-Event`, `X-GitHub-Delivery` |
| Svix-style | `svix-id`, `svix-timestamp`, `svix-signature` |
| Firebase / Google | Authorization / channel-style headers (varies) |
| Generic | Common `X-Signature` / `X-Webhook-Signature` patterns |

## License

MIT © Humayun Tanwar — see [LICENSE](./LICENSE).

## Contributing

Issues and PRs welcome. Keep it a single static page with no backend and no telemetry.
