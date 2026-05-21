# FMF Pipeline Cohort — Deployment Brief

**Deployable file:** `briefs/fmf-pipeline-cohort.html` (in this repo)
**Purpose:** A Netlify-ready version of the landing page with working form submissions delivered by email.
**Estimated time to live:** 20–30 minutes.

---

## Status

The deployable file is **ready**. The form-wiring changes have been applied — you do
not need claude.ai or any code editor. Just download `fmf-pipeline-cohort.html` and
follow Part 2 (Netlify) and Part 3 (subdomain).

The design, copy, layout, calculator, and FAQ are byte-identical to the original.
Only the application form was modified.

---

## Part 1 — What was changed (for the record)

Five changes, all confined to the application form. Nothing else differs from the
original HTML.

### 1. Form element attributes
```html
<!-- Before -->
<form class="form-grid" onsubmit="event.preventDefault();">
<!-- After -->
<form class="form-grid" name="cohort-application" method="POST" data-netlify="true" netlify-honeypot="bot-field">
```
`data-netlify="true"` is what Netlify scans for at deploy time. `netlify-honeypot`
adds invisible spam protection.

### 2. Hidden fields (form routing + spam trap)
Added immediately inside the `<form>`:
```html
<input type="hidden" name="form-name" value="cohort-application">
<p style="display:none;"><label>Do not fill this in: <input name="bot-field"></label></p>
```
The `form-name` field routes the AJAX POST to the right form. The hidden honeypot
catches bots — real users never see it.

### 3. `name` attributes on every field
The original inputs had only `id` attributes. **Form submissions only capture fields
with a `name` attribute** — without this, Netlify would receive blank applications.
A `name` (matching each `id`) was added to all 10 fields, plus `required` on the five
text/email/phone fields.

### 4. Submit button moved inside the form
The button previously sat *outside* the `</form>` tag — it could never trigger a
submission. It was moved inside the form, changed to `type="submit"`, and given
`grid-column: 1 / -1` so it spans the full form width on desktop and stays clean on
mobile. (`1 / -1` spans all real columns without breaking the 1-column mobile layout.)

### 5. AJAX submission with inline thank-you
The old `onsubmit="event.preventDefault();"` was replaced with a handler that POSTs
to Netlify and then swaps the form for an inline "Application received" message — no
page redirect, the applicant stays on the page.

---

## Part 2 — Deploy to Netlify (step by step)

### Step 1 — Create your Netlify account
Go to [netlify.com](https://netlify.com) and sign up free. No credit card needed for this.

### Step 2 — Deploy the file
1. From your Netlify dashboard, click **"Add new site"** → **"Deploy manually"**
2. Drag and drop your HTML file into the upload box
3. Netlify deploys in under 30 seconds and gives you a URL like `https://random-name-123.netlify.app`

### Step 3 — Verify form detection
1. Go to **Site configuration** → **Forms**
2. You should see **"cohort-application"** listed with 0 submissions
3. If it's not there, the `data-netlify="true"` attribute was not applied — re-check Change 1

### Step 4 — Set up email notifications
1. In **Forms** → click **"cohort-application"** → **"Form notifications"**
2. Click **"Add notification"** → **"Email notification"**
3. Enter `marketing@fillmyfunnel.co.uk`
4. Save — you will now receive an email for every application submitted

### Step 5 — Test the form
Go to your Netlify URL, fill in the application form, and submit. Check:
- The thank-you message appears inline (no page redirect)
- An email arrives at `marketing@fillmyfunnel.co.uk`
- The submission appears in Netlify → Forms → cohort-application

---

## Part 3 — Add your subdomain (e.g. cohort.fillmyfunnel.co.uk)

### In Netlify
1. **Site configuration** → **Domain management** → **Add custom domain**
2. Type `cohort.fillmyfunnel.co.uk` and click **Verify**
3. Netlify will prompt you to add a DNS record

### At your DNS provider
Add a **CNAME record**:

| Type  | Name   | Value                          | TTL  |
|-------|--------|--------------------------------|------|
| CNAME | cohort | `your-site-name.netlify.app`   | 3600 |

Replace `your-site-name` with the actual subdomain Netlify assigned (visible in Domain management).

### HTTPS
Netlify provisions a free Let's Encrypt certificate automatically once DNS propagates (typically 5–30 minutes). No action needed.

### Updating the page
Drag and drop an updated HTML file onto the same Netlify site to redeploy. All settings (domain, notifications, forms) are preserved.

---

## Appendix A — Design system tokens

For reference if rebuilding or extending the page.

### Colours (CSS custom properties)

```css
--cornflower: #6A8FFD;   /* Primary accent */
--pale-sky: #C0D5F0;     /* Light blue */
--dusk-blue: #3F4C7B;    /* Secondary text */
--electric: #2068FF;     /* Bright blue */
--blue-black: #0C101D;   /* Headings / near-black */
--prussian: #151C33;     /* Deep navy */
--pale-slate: #C1C9D4;   /* Light grey-blue */
--alice-blue: #E7ECF3;   /* Very light blue */
--platinum: #F2F5F8;     /* Page background alt */
--white: #FFFFFF;
--success: #05C168;      /* Green — semantic only */
--success-100: #DEF2E6;
--warn: #FDBD1A;         /* Amber — semantic only */
--warn-100: #FFF6E4;
--danger: #FF5A65;       /* Red — semantic only */
--danger-100: #FFEFF0;
--cold-blue: #1D8BFE;
--cold-bg: #EAF4FF;
```

### Typography

```html
<link href="https://fonts.googleapis.com/css2?family=Lora:wght@400;500;600;700&family=DM+Sans:wght@400;500;600;700&display=swap" rel="stylesheet">
```

- **Headings:** Lora (serif), 400/500/600/700
- **Body:** DM Sans (sans-serif), 400/500/600/700
- Base line-height: 1.55
- `-webkit-font-smoothing: antialiased`

### Spacing

- Section padding: `96px 24px`
- Max-width container `.inner`: `1180px`
- Breakpoints: `980px` / `880px` / `800px` / `720px`

### Border radius

- Pills/buttons: `100px`
- Cards (large): `18px`
- Cards (standard): `14px`
- Cards (small): `12px` / `10px`

### Card pattern

```css
background: var(--white);
border: 1.5px solid rgba(106,143,253,0.22);
border-radius: 14px;
box-shadow: 0 2px 12px rgba(21,28,51,0.03);
```

---

## Appendix B — Section inventory

| ID | Nav label | Background | Heading |
|---|---|---|---|
| `#hero` | — | White | *25%* more opportunities from LinkedIn in 6 months. Or we refund every penny of our fees. |
| *(no id)* | — | White | Most LinkedIn ad accounts struggle to drive *reliable, scalable growth.* |
| `#offer` | Offer | Accent (pale-sky) | The commitment, *in one sentence.* |
| `#why-fmf` | Why FMF | Platinum | EMEA's only Official LinkedIn *Marketing Partner.* |
| `#roi` | Pay back? | ROI bg (pale gradient) | Does it *pay back* for your business? |
| `#qualify` | Who qualifies | Platinum | The two businesses we are *looking for.* |
| `#how` | How | White | 180 days, five phases, *two real off-ramps.* |
| *(no id)* | — | Accent | Five clauses that put *our skin in the game.* |
| *(no id)* | — | White | What FMF does that *other agencies don't.* |
| `#faq` | FAQ | Accent | The things every CMO asks *before applying.* |
| `#apply` | Apply for the cohort | Platinum | From application to *programme launch* in five steps. |

---

## Appendix C — Form field specification

Form name: `cohort-application`
Intro copy: "Ten quick questions. Three minutes."

| Field ID | Label | Type | Placeholder / Options |
|---|---|---|---|
| `name` | Your name | text | "First and last name" |
| `role` | Role | text | "CMO, Head of Marketing, etc." |
| `company` | Company | text | "Your company name" |
| `email` | Work email | email | "name@company.com" |
| `phone` | Phone | tel | "+44 7700 900000" |
| `acv_form` | Average contract value | select | Below £30k · £30k to £75k · £75k to £200k · £200k+ |
| `spend` | Current monthly LinkedIn spend | select | Below £5k · £5k to £10k · £10k to £20k · £20k+ |
| `current_opps_form` | Current LinkedIn-influenced opportunities (trailing 6 months) | select | Below 8 (minimum required for cohort) · 8 to 15 · 16 to 30 · 31 to 60 · 60+ |
| `cycle` | Typical sales cycle length | select | Under 3 months · 3 to 6 months · 6 to 12 months · Over 12 months |
| `attribution` | Attribution platform | select | Dreamdata · HockeyStack · Demandbase · Other platform installed · None, willing to install · None, unwilling to install |

Submit button text: **"Apply for the cohort"**
Footnote: "Applications close 14 June 2026. Programme starts 1 July. Response within 3 business days, every time, by email."

---

## Appendix D — ROI calculator logic (for reference)

All calculations are client-side JavaScript. No external API.

**Inputs:** `current_opps` · `acv` · `close_rate` (%) · `ltv_years` · `budget` (monthly £)

**Fixed constant:** management fees = £27,000 (6-month programme)

**Core formula:**
```
adSpend = budget × 6
totalCost = adSpend + 27000
newOpps = floor(currentOpps × 0.25)          // 25% guaranteed lift
newDeals = newOpps × (closeRate / 100)
year1Revenue = newDeals × acv
ltvRevenue = newDeals × acv × ltvYears
year1ROI = year1Revenue / totalCost
ltvROI = ltvRevenue / totalCost
```

**Verdict thresholds:**
- Red: `currentOpps < 8` OR `newOpps < 2` OR `ltvROI < 1.5`
- Amber: `ltvROI >= 1.5` and `< 3`
- Green: `ltvROI >= 3`

Green and amber verdicts show an "Apply for the cohort" CTA that anchors to `#apply`.
