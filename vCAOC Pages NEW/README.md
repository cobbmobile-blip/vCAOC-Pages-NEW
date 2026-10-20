# vCAOC Pages (prototype)

Clickable prototype of a virtual Combined Air Operations Center (CAOC) website for DCS World campaigns.
Static HTML/CSS/JS — no build step. Data is stored in the browser (localStorage) and logins are a
client-side demo gate only; a real deployment needs server-side auth and a shared database.

## Run it

Serve the folder with any static web server and open `login.html`, for example:

```
python -m http.server 8765
```

then browse to http://localhost:8765/login.html. (The theater map loads Leaflet and map tiles from the internet.)

## Pages

| Page | Who | What |
|---|---|---|
| `login.html` | CAOC staff | Staff sign-in |
| `index.html` | CAOC staff | Campaign dashboard: products, deliverables tracker (CFACC), requests, groups |
| `product.html?p=<id>` | CAOC staff | Product builder + live preview (CI, AOD, JIPTL, TDP, ATO, ACO, SPINS, Comm Plan, BDA) |
| `map.html` | CAOC staff | Theater map from the ACO: nav/spider/dest points, threats with DCS range rings, FLOT, FSCL, MRR, AR tracks, HVAA orbits, artillery ROZ |
| `campaign-review.html` | CAOC staff (CFC decides) | Campaign proposal review queue |
| `unit-login.html` | Participating groups | Group sign-in |
| `unit.html` | Groups (staff can observe) | Op Status: product board, distribution matrix, ATO lines, group pack download |
| `campaign-submit.html` | Groups | Campaign proposal wizard (JFACC, intent, win/loss, REDFOR, schedule…) |

## Demo accounts (prototype only)

- Staff: ATLAS (CFC), OVERLORD (CFACC), SPARTAN, CHALICE, WIZARD, DARKSTAR — password `aoc-demo`
- Groups: viper, hornet, eagle, hawg, knights, tomcat, harrier, mistral, thunder, keepers, ridge, support — code `unit-demo`

## Roles

- **CFC** — manages the CAOC, decides on campaign proposals, can assist the CFACC.
- **CFACC** — makes sure every deliverable is built and approved; approves and publishes.
- **Group JFACC** — elected by the sponsoring group; manages the campaign and is the CAOC's single POC.

## Files

- `js/data.js` — seed data (fictional groups, campaigns, sample products, proposals, map)
- `js/app.js` — storage, auth gates, roles/permissions, shared chrome
- `js/products.js` — product schemas and document renderers (incl. comm matrix)
- `js/proposal.js` — campaign proposal questions and renderer
- `css/styles.css` — three visual styles (Ops Floor, Night Vision, Staff Paper)

DKS (Digital Kneeboard Simulator) export buttons are placeholders until that integration is built.
