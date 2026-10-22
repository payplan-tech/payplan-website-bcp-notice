# payplan.com — holding page

What visitors see while **payplan.com** is unavailable. PayPlan's Business
Continuity Plan is active following the PHP cyber security advisory.

One static HTML file. No build, no framework, no dependency on any
PayPlan service — it has to render when other things do not.

## The WhatsApp link

`https://wa.me/447441927800?text=Ready%20to%20chat%20now`

That number and prefill come from the published post "What happens when you
contact PayPlan — a step-by-step guide", the only content in the dataset
carrying a WhatsApp link. The submission form's own `whatsAppNumber` field is
empty, and payplan.com was returning 503, so neither could supply it.

If the business WhatsApp number has changed, this is the one line to update.

## One file, under 4KB

`index.html` is the whole site: the CSS is inline and there is no stylesheet to
fetch. It is **4091 bytes**, inside the 4096-byte limit for a WAF custom
response body, so the same file can be served by Pages and pasted into the WAF.

**That budget is the constraint on every edit.** Check it before pushing:

```sh
wc -c index.html     # must stay under 4096
```

Things that were cut to fit, and should not come back without finding the bytes
elsewhere: the favicon, the code comments (which is why this file carries the
explanation instead), and the separate stylesheet. What was kept: the Figtree
webfont, because without it the page stops looking like PayPlan.

Brand values are inline literals rather than custom properties — each is used
once or twice, and the indirection cost more than it saved. They come from
`packages/design-tokens/v6.css` in the website repo: cobalt-600 `#2200cc`,
cobalt-900 `#0f0054`, sand-50 `#faf7f0`, sand-200 `#ece3d8`, sand-600
`#6b6257`, sand-900 `#231f1a`, gold-500 `#c9922f`.

## Changing the message

Everything editable is in `index.html`, between the `STATUS MESSAGE` comments.
Edit, commit to `main`, and the deploy runs itself. Nothing else needs
touching.

Keep it short. Someone reading this came looking for help with debt and wants
to know how to get it; anything past the first screen is a paragraph they will
not reach.

The current wording follows the approved Business Continuity Notification used
for Creditor Suite, with two deliberate differences: the heading names the
website rather than a system, and the actions are the consumer channels —
WhatsApp and `0800 316 1833` from `siteSettings`.

## The two actions

WhatsApp is a cobalt pill with the WhatsApp glyph; the phone is the same pill
outlined. Both are PayPlan's primary button from `HeroV6` — pill radius, 16px
semibold, 28px by 14px — rather than WhatsApp's own green: it is PayPlan's
page, and the glyph is what makes the channel recognisable.

The number appears once, as the text of the button that dials it. It was in
two places at first, which made a screen reader read it twice over.

## Deployment

GitHub Actions publishes to GitHub Pages on every push to `main`, and
**Actions → Deploy → Run workflow** republishes without a commit.

One-time repository setup: **Settings → Pages → Build and deployment →
Source: GitHub Actions**. The workflow asks for Pages to be enabled itself, but
the token is not permitted to in this organisation, so the step fails with
"Create Pages site failed" until the setting is on.

### Pointing a domain at it

Add a `CNAME` file at the repository root holding the hostname on its own line,
set that hostname's DNS to the Pages target, and enter it under **Settings →
Pages → Custom domain**. There is deliberately no `CNAME` committed: one
naming a domain not yet pointed here takes the default `github.io` address
away and leaves no working URL at all.

## What is deliberately absent

- **No analytics, tags or cookie banner.** Nothing to consent to, so nothing
  between a worried visitor and the message.
- **`noindex, nofollow`.** A holding page must not be left ranking in place of
  the real site once it is back.
- **No auto-refresh.** A page reloading under someone reading it is worse than
  one they refresh themselves.
- **The logo is not a link.** Everywhere else it goes to payplan.com, which is
  the thing that is down.
- **No quick exit button.** The live site carries one for visitors who need to
  leave quickly. This page holds nothing sensitive beyond a service notice, and
  a half-built version would be worse than none — worth adding if the outage
  runs long.
- **Brand tokens copied, not imported.** The palette and type scale come from
  `packages/design-tokens/v6.css` in the website repo. Nothing keeps the two in
  step; if the brand changes, change it here too.

## When the site is back

Point the domain back at the real site. Leave this repository as it is — the
next continuity event wants it ready, not rebuilt.
