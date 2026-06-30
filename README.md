# Ambit Agents — demo site

A single static page demoing the Ambit Agents pitch: a free, no-backend interactive
walkthrough that takes a visitor through three steps —

1. **Tell us the cost** — a short diagnostic that captures their hours/week, who does
   the work, and how often, to drive the ROI numbers shown at the end.
2. **Feel the work** — a simulated spreadsheet search where they manually find
   mismatched orders/invoices (missing invoice, amount mismatch, date mismatch)
   against a running timer.
3. **Watch it disappear** — the same dataset run through a simulated agent in
   seconds, ending in a personalized ROI summary based on what they entered in
   step 1.

No API calls, no backend, no build step. Everything is hardcoded HTML/CSS/vanilla JS
in `index.html` so it's reliable in front of a prospect regardless of wifi.

## Deploying to Cloudflare Pages

### Option A — drag and drop (fastest)
1. Log into the Cloudflare dashboard → **Workers & Pages** → **Create application** → **Pages** → **Upload assets**.
2. Drag this folder's `index.html` in directly. Cloudflare will deploy it instantly
   and give you a `*.pages.dev` URL.
3. Optionally connect a custom domain (e.g. `ambitagents.com`) under
   **Custom domains** once it's registered.

### Option B — connect to GitHub (recommended for ongoing edits)
1. Push this folder to a new GitHub repo.
2. In Cloudflare dashboard: **Workers & Pages** → **Create application** → **Pages** →
   **Connect to Git** → select the repo.
3. Build settings:
   - Framework preset: **None**
   - Build command: *(leave blank)*
   - Build output directory: `/`
4. Deploy. Every push to the connected branch redeploys automatically.

## Wiring up the consultation form

The form at the bottom (`#consult-form`) currently just shows a success message —
it does not send anywhere yet. Two easy ways to make it real on Cloudflare:

1. **Cloudflare Pages Function** — add a `functions/submit.js` file that receives
   the POST and emails you (e.g. via a service like Resend or Mailchannels), or
   writes to a small KV/D1 store.
2. **HubSpot Forms API** — point the form's `fetch` call at your HubSpot portal's
   Forms submission endpoint so submissions land directly as contacts owned by
   Jordan (owner ID 164914707).

Either approach is a small addition — ask Claude to wire it once you've decided
which one fits your stack.

## Editing the dataset

The order/invoice rows used in step 2 and step 3 live near the top of the
`<script>` block in `index.html` (`ordersData` / `invoicesData`). The three
"exception" rows are flagged with `status:'exception'`; everything else is
`status:'clean'`. Swap names, amounts, and dates here if you want a version
themed to a specific vertical before a particular consultation.
