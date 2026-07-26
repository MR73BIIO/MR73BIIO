readme_content = '''<p align="center">
  <img src="https://raw.githubusercontent.com/MR73BIIO/MR73BIIO/main/assets/Baner.png" 
       alt="MR73BIIO — Where roots meet code" 
       width="100%">
</p>

<h1 align="center">Marcin Ruszczak · MR73BIIO</h1>
<p align="center"><strong>Python · ETL · Automation · GIS · DACH Market</strong></p>

<p align="center">
  I extract, validate and automate data workflows — and I do it in German.<br>
  22 years in operations before I wrote my first line of code.<br>
  I build for the person who opens the file on Monday morning, not for the demo.
</p>

<p align="center">
  <a href="https://www.upwork.com/freelancers/~0181c636507bece80f">🚀 Hire me on Upwork</a> · 
  <a href="https://mr73biio.github.io">🌐 Services & Pricing</a> · 
  <a href="https://x.com/MR73BIIO_Q">𝕏 @MR73BIIO_Q</a> · 
  <a href="https://www.linkedin.com/in/marcin-ruszczak-27b37b19b">💼 LinkedIn</a> · 
  <a href="https://www.tradingview.com/u/MR73BIIO_Q/">📈 TradingView</a>
</p>

---

> *„Każda idea, która rodzi się uczciwie w biciu serca, może przekroczyć każdą granicę, a jej czas ma większą wagę niż każda waluta."*  
> **MR73BIIO · Pulsus Cordis Tui · 1998**

---

## What I do

### 🌲 GIS & Tree Cadastre
Drone imagery → GIS-ready inventory. 10k+ trees mapped from a single flight.  
*Python · QGIS · GeoJSON* · [Case study →](#)

### 📊 Data Extraction & ETL
German invoice PDFs → validated CSV/Excel. Raw exchange feeds → gap-free archives.  
*Python · Pandas · Linux · VPS* · [Case study →](#)

### 🧪 Strategy Audit
OOS split + Bonferroni correction. I ran 256 variants of my own strategy through it: **0 passed. 0 deployed.**  
*Python · SciPy · Statistics* · [See the audit →](https://github.com/MR73BIIO/strategy-validation)

---

## Proof of work

- **149 live trades** audited through `audit.py` → **REJECTED** (saved me from deploying a false edge)
- **256 strategy variants** tested out-of-sample against four symbols, four market regimes, on Hyperliquid perpetuals → **0 survived** the Bonferroni-corrected threshold
- **10,000+ trees** mapped from single drone flight to GIS-ready cadastre
- **100% coverage** on raw exchange feed → validated, gap-free time-series archive

> *„I set up scaffolding, not dependency: you get the code, the access and the documentation, and I step off the structure."*

---

## How I validate a trading strategy

I do not build strategies to confirm them. I build validation pipelines to **destroy them** before they destroy capital.

Every candidate must pass four gates:

| Gate | Threshold | Why |
|---|---|---|
| Out-of-sample split | ≥ 100 trades | Below this, t-stat is noise |
| Edge after fees | > 0 | Round-trip cost: 0.0258% (maker+maker) |
| Statistical significance | t ≥ 2.0 **on the test set** | Not on the set you optimized against |
| Neighbourhood stability | ≥ 60% | Real edge is a plateau, not a spike |

On top: **Bonferroni correction** for 64 parameter combinations — because argmax on a grid produces false winners even on pure noise.

**Result on my own strategy:** 256 variants. Zero passed. Zero deployed.  
[Read the full audit →](https://github.com/MR73BIIO/strategy-validation)

---

## How we work together

1. **You send the mess** — PDFs, raw feeds, CSV chaos, a strategy idea
2. **I audit & scope** — 24h turnaround on feasibility + fixed quote
3. **I build & document** — you get code, access, README, no lock-in
4. **I step off** — the pipeline runs without me

*Typical engagement: 3–5 days for ETL, 1–2 days for strategy audit.*

---

## Stack & credentials

**Languages:** Polish (native) · German (fluent, Swiss workplace) · English · Russian  
**Stack:** Python · Pandas · SciPy · QGIS · Linux · VPS / systemd · Git · SQL  
**Credentials:** Harvard CS50P (2026) · CS50 Cybersecurity (in progress)

---

<p align="center">
  <strong>Ready to automate the repetitive part?</strong><br>
  <a href="https://www.upwork.com/freelancers/~0181c636507bece80f">Start on Upwork →</a> · 
  <a href="https://mr73biio.github.io">Browse services →</a> · 
  <a href="https://x.com/MR73BIIO_Q">DM on X →</a> · 
  <a href="https://www.linkedin.com/in/marcin-ruszczak-27b37b19b">LinkedIn →</a> · 
  <a href="https://www.tradingview.com/u/MR73BIIO_Q/">TradingView →</a>
</p>

<p align="center"><em>Pulsus Cordis Tui — time is the only currency that does not come back.</em></p>
