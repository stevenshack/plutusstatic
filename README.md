# plutusstatic — static mirror of smbroyalty.com

Mirrored 2026-10-07 from the live site with:
```bash
wget --mirror --convert-links --adjust-extension \
  --page-requisites --no-parent \
  --reject-regex="/office/account" https://smbroyalty.com
```
then promoted to this folder root. Links converted to relative; all
images/CSS/JS are local (`static/`).

## Pages
index, about-us, why-sell, royalty-calculator, faq, essays,
tools-and-resources, privacy, terms, offer-terms,
get-offer/ (lead capture — see below)

Removed: `/offer/*` (old multi-step backend flow), `/office/*`
(signin/signup — dynamic accounts), and the newsletter page plus its
footer links (deleted altogether).

## Lead capture (`get-offer.html`)

All "Get Offer" links point to `get-offer.html`, a lead form in the
site's own styling: name, email, phone, county/state, mineral details.
It uses Netlify Forms as-is (`data-netlify="true"`); swap the `<form>`
tag for a Formspree endpoint if you prefer (comment in the file).
`offer/index.html` is a redirect stub for old bookmarks; the dead
multi-step `/offer/restart/` flow was removed.

## Preview / deploy
```bash
python3 -m http.server 8000   # open http://localhost:8000/
```
Deploy this folder as-is to Netlify, Cloudflare Pages, GitHub Pages,
Vercel, or S3. No build step.

Note: `/offer/` flow and newsletter signup POST to the live backend —
they render statically but won't process submissions once Heroku is off
unless re-pointed to a form handler (Netlify Forms / Formspree).

## Heroku takedown (after DNS cutover + backups)
```bash
heroku login
heroku apps
heroku pg:backups:capture --app YOUR-APP && heroku pg:backups:download --app YOUR-APP
heroku ps:scale web=0 --app YOUR-APP
heroku apps:destroy --app YOUR-APP --confirm YOUR-APP
```
