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
| `targeting.html` | CAOC staff | Target nomination (from the JIPTL): REDFOR order of battle → JIPTL, ported from the 639CAOC JIPTL pipeline |
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

## Target nomination (JIPTL pipeline)

Browser port of `tools/jiptl_pipeline/jiptl_generate.py` from the 639CAOC repo:

1. **Key tasks & priorities** — the cycle config: Key Task weights and per-task caps, tier weights, default grades.
   Task 5 (CAS/TST) is continuous and never ranked.
2. **Area of interest** — a box (N/S/E/W bounds) or a point + radius, typed in or set by clicking the page's map.
   Defined on this page only; it is independent of the ACO and the theater map.
3. **REDFOR order of battle** — Coalition Parser CSV exports (`Group, Unit, Type, Latitude, Longitude, Altitude (ft)`;
   DMS or decimal). `samples/iron_trident_redfor.csv` is an example.
4. **Run** — classify each DCS type, group by DCS group, name the group after its doctrinally-critical unit
   (e.g. an SA-11 by its TELARs, an SA-6 by its 1S91 radar), keep groups inside the BMA,
   keep groups inside the area of interest, score = task weight + tier weight, rank, cap per task, number `T10001…`.
5. **Review** — tabs for JIPTL / Watchlist (cut by caps) / Outside BMA / Unclassified types / Not targets;
   re-grade, re-order, cut or promote lines, then **Send to JIPTL** (the JIPTL returns to Draft for CFACC approval).

The built-in unit table covers ~130 common DCS types. Load the full `config/unit_classification.json` from the 639CAOC
repo on the page to use the complete catalogue. `.miz` intake still needs the Python `miz_intake.py` (calibrated projection).

## Files

- `js/data.js` — seed data (fictional groups, campaigns, sample products, proposals, map)
- `js/app.js` — storage, auth gates, roles/permissions, shared chrome
- `js/products.js` — product schemas and document renderers (incl. comm matrix)
- `js/proposal.js` — campaign proposal questions and renderer
- `js/targeting.js` — JIPTL nomination pipeline, built-in DCS unit table, SAM kill doctrine
- `samples/iron_trident_redfor.csv` — sample Coalition Parser export for the demo campaign
- `css/styles.css` — three visual styles (Ops Floor, Night Vision, Staff Paper)

DKS (Digital Kneeboard Simulator) export buttons are placeholders until that integration is built.
