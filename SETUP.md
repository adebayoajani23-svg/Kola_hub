# KOLA Messaging Lab — implementation package

## Included
- `index.html`: the current KOLA Help Hub website with Messaging Lab added, plus a featured Parts Paradise store promotion in the site announcement bar, navigation, Messaging Lab and footer.
- `worker.js`: secure Cloudflare Worker API for AI advert generation, campaign draft storage and customer feedback storage; it also serves the static website assets.
- `wrangler.toml`: same-Worker static asset + API configuration template.

## One-time Cloudflare setup
1. Create a Cloudflare KV namespace (Workers & Pages → KV). Copy its namespace ID into `wrangler.toml`, replacing `REPLACE_WITH_YOUR_KV_NAMESPACE_ID`.
2. Put these files in one project folder on a computer with Wrangler installed and authenticated to your Cloudflare account. The project serves `index.html` and handles `/api/*` from the same Worker, including on your `workers.dev` address.
3. Add the OpenAI key as a Worker secret (never put it in `index.html`): `npx wrangler secret put OPENAI_API_KEY`.
4. Deploy from the project folder: `npx wrangler deploy`.
5. Verify the KV binding is named exactly `KOLA_DATA` and `ALLOWED_ORIGIN` matches the exact public site origin.
6. Visit `/api/health`. It should return JSON with `ok: true`, `aiConfigured: true`, and `storageConfigured: true`.

## Featured store promotion
The website consistently invites visitors to browse Parts Paradise at https://parts-paradise-builder.lovable.app. The link opens in a new tab. Update the URL in `index.html` if the store address changes.

## API routes
- `GET /api/health` — reports whether AI and KV are configured.
- `POST /api/generate` — generates advert copy and audience recommendations through OpenAI.
- `POST /api/feedback` — stores customer feedback in KV.
- `POST /api/campaigns` — stores generated campaign drafts in KV.

## Important launch notes
- The website generates and saves campaign drafts and opens WhatsApp with prefilled advert copy. It does **not** automatically publish paid Meta/Google adverts. Direct publishing requires approved platform developer apps, business accounts, permissions, access tokens, policy review and additional integration code.
- Feedback and campaign data are stored in KV. Add authentication and an admin-only reporting endpoint before exposing a feedback dashboard to staff.
- KV is suitable for a lightweight first release. For searchable reporting, multi-user permissions and CRM features, migrate records to D1 or Supabase.
- Configure rate limiting, abuse protection, privacy notice and retention/deletion policy before public launch. The included API is a functional starter, not a complete compliance or production-hardening layer.
