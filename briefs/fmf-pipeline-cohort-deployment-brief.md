# FMF Pipeline Cohort — Deployment Brief

**File this brief is based on:** `2b728130-pipelinecohortfmf.html`
**Purpose:** Turn the finished HTML landing page into a Netlify-deployable page with working form submissions delivered by email.
**Estimated time to live:** 30–45 minutes.

---

## How to use this brief

The HTML file is already production-quality. The only changes needed are:

1. Wire the form to Netlify Forms (4 code changes)
2. Deploy the file to Netlify (drag and drop)
3. Add email notification for submissions
4. Point your subdomain

**The fastest path:** paste the prompt in Part 1 into claude.ai, paste the full HTML contents after it, download the artifact, then follow Part 3.

---

## Part 1 — The claude.ai prompt (copy and paste this)

> You are going to modify a single self-contained HTML landing page. Your only job is to wire the application form to Netlify Forms so it can be deployed and receive submissions. Do not change any copy, layout, design, colours, or interactive behaviour. Apply these four changes exactly and return the complete modified HTML file:
>
> **Change 1 — Form element attributes**
> Find: `<form class="form-grid" onsubmit="event.preventDefault();">`
> Replace with: `<form class="form-grid" name="cohort-application" method="POST" data-netlify="true">`
>
> **Change 2 — Hidden form-name field**
> Immediately after the opening `<form>` tag, add:
> `<input type="hidden" name="form-name" value="cohort-application">`
>
> **Change 3 — Move submit button inside the form**
> The `<button class="btn-submit" type="button">Apply for the cohort</button>` currently sits outside the `</form>` closing tag. Move it to just before `</form>` and change `type="button"` to `type="submit"`. The `<p class="form-footnote">` stays outside the form.
>
> **Change 4 — AJAX submission with inline thank-you**
> In the `<script>` block, after the `calculateROI()` call at the bottom, add this:
>
> ```javascript
> const cohortForm = document.querySelector('form[name="cohort-application"]');
> if (cohortForm) {
>   cohortForm.addEventListener('submit', function(e) {
>     e.preventDefault();
>     const data = new FormData(cohortForm);
>     fetch('/', {
>       method: 'POST',
>       headers: { 'Content-Type': 'application/x-www-form-urlencoded' },
>       body: new URLSearchParams(data).toString()
>     }).then(() => {
>       const shell = cohortForm.closest('.apply-shell');
>       shell.innerHTML = `
>         <div style="text-align:center;padding:60px 24px;">
>           <div style="font-family:'Lora',serif;font-size:1.6rem;font-weight:700;color:#0C101D;margin-bottom:16px;">Application received.</div>
>           <p style="font-size:1rem;color:#3F4C7B;line-height:1.6;max-width:480px;margin:0 auto;">We'll respond personally within 3 business days by email. No automated sequences — a real reply from our senior strategy team.</p>
>         </div>`;
>     }).catch(() => {
>       alert('Something went wrong. Please email us directly at marketing@fillmyfunnel.co.uk');
>     });
>   });
> }
> ```
>
> Return the complete HTML file with only these four changes applied. Nothing else should differ from the input.

**After the prompt, paste the entire contents of your HTML file.**

---

## Part 2 — The four changes explained

For reference if you want to apply them manually rather than via claude.ai.

### Change 1 — Form element attributes

```html
<!-- Before -->
<form class="form-grid" onsubmit="event.preventDefault();">

<!-- After -->
<form class="form-grid" name="cohort-application" method="POST" data-netlify="true">
```

`data-netlify="true"` is what Netlify scans for at deploy time. Without it, Netlify never registers the form.

### Change 2 — Hidden form-name field

```html
<form class="form-grid" name="cohort-application" method="POST" data-netlify="true">
  <input type="hidden" name="form-name" value="cohort-application">
  <!-- rest of fields -->
```

Required for AJAX submissions. Netlify uses this to route the POST to the correct form.

### Change 3 — Submit button inside the form

```html
<!-- Move this line from outside </form> to inside, just before </form> -->
<button class="btn-submit" type="submit">Apply for the cohort</button>
```

A submit button outside the form tag has no association with the form by default — it will not trigger form submission.

### Change 4 — AJAX submission handler

Replaces the existing `onsubmit="event.preventDefault();"` behaviour with a proper AJAX POST to Netlify, followed by an inline thank-you state. No page redirect. The user stays on the page and sees confirmation immediately.

---

## Part 3 — Deploy to Netlify (step by step)

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

## Part 4 — Add your subdomain (e.g. cohort.fillmyfunnel.co.uk)

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
