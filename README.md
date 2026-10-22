# payplan.com — holding page

What visitors see while **payplan.com** is unavailable. PayPlan's Business
Continuity Plan is active following the PHP cyber security advisory.

Static HTML and one stylesheet. No build, no framework, no dependency on any
PayPlan service — it has to render when other things do not.

## Outstanding

**The WhatsApp link points nowhere yet.** The button href is
`https://wa.me/0000000000`. Replace those digits with the WhatsApp business
number in international format — digits only, no `+` and no spaces, so a UK
number written `07700 900123` becomes `447700900123`.

It could not be filled in automatically: `whatsAppNumber` is empty in Sanity,
and payplan.com was already returning 503 so it could not be read from the
live page either.

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
