# FOOD-MATIC — Project Initialization Guide

## What is FOOD-MATIC?

FOOD-MATIC is a browser-based food development tool designed for R&D teams,
baristas, and food entrepreneurs. It runs entirely in the browser — no server
needed — and can be hosted for free on GitHub Pages.

---

## Tech Stack

| Layer       | Choice                          | Reason                              |
|-------------|---------------------------------|-------------------------------------|
| Language    | HTML + CSS + Vanilla JS         | Zero build step, GitHub Pages ready |
| Storage     | `localStorage` (JSON)           | Works offline, no backend           |
| Import/Export | Native File API (JSON files)  | Portable data, no lock-in           |
| Hosting     | GitHub Pages (static)           | Free, runs from repo                |

---

## Project Structure

```
FOOD-MATIC/
├── index.html        ← Main app (single file, self-contained)
├── INIT.md           ← This file: setup & architecture guide
├── README.md         ← Project overview for GitHub
├── beverages/        ← Future: beverage-specific data/exports
└── sauces/           ← Future: sauce-specific data/exports
```

---

## How to Run Locally

**Option A — Open directly:**
```
Double-click index.html in your file explorer
```

**Option B — Local dev server (recommended to avoid CORS issues):**
```bash
# Python
python -m http.server 8080

# Node
npx serve .
```
Then open `http://localhost:8080`

---

## How to Deploy on GitHub Pages

1. Push this repo to GitHub
2. Go to **Settings → Pages**
3. Source: **Deploy from branch → main → / (root)**
4. Your app will be at: `https://<username>.github.io/<repo-name>/`

---

## Data Architecture

All data is stored as JSON in `localStorage` under the key `foodmatic_v1`.

### Schema

```json
{
  "version": 1,
  "exportedAt": "ISO-8601 timestamp",
  "ingredients": [
    {
      "id": "unique-id",
      "name": "Espresso",
      "cat": "Coffee",
      "qty": 1000,
      "unit": "ml",
      "price": 350,
      "notes": "Double shot base"
    }
  ],
  "recipes": [
    {
      "id": "unique-id",
      "name": "Caramel Latte",
      "cat": "Hot Coffee",
      "serving": 1,
      "yield": 250,
      "price": 85,
      "desc": "Step-by-step method...",
      "createdAt": "ISO-8601",
      "updatedAt": "ISO-8601",
      "ingredients": [
        { "ingId": "ref-to-ingredient-id", "name": "Espresso", "unit": "ml", "amount": 30 }
      ]
    }
  ]
}
```

### Key Formulas

```
Cost Per Unit  = purchase_price / purchase_qty
Ingredient Cost = cost_per_unit × amount_used
Total Recipe Cost = Σ ingredient_costs
Cost Per Serving = total_recipe_cost / servings
Food Cost % = (cost_per_serving / selling_price) × 100
Gross Profit = selling_price − cost_per_serving
```

---

## Features (v1)

- [x] **Ingredients Library** — name, category, purchase qty/price, cost per unit
- [x] **Recipe Builder** — menu name, category, servings, yield, method
- [x] **Cost Calculator** — auto-calculates total cost, cost/serving, food cost %, GP
- [x] **Dashboard** — overview stats, top cost recipes, ingredient price list
- [x] **Import/Export JSON** — portable data, sharable between devices
- [x] **Responsive UI** — works on mobile and desktop
- [x] **Dark Theme** — eye-friendly for kitchen/lab use

---

## Planned Modules

| Module         | Status  | Description                              |
|----------------|---------|------------------------------------------|
| Beverages      | v1      | Core recipe + cost system                |
| Sauces         | planned | Separate category with batch scaling     |
| Batch Scaler   | planned | Scale recipe × N servings                |
| Cost History   | planned | Track price changes over time            |
| Photo Upload   | planned | Attach photos to recipes                 |
| Print View     | planned | Printer-friendly recipe card             |
| Multi-language | planned | EN / TH toggle                           |

---

## Coding Conventions

- **No build tools** — keep it plain HTML/CSS/JS, importable as a single file
- **Data mutations** always call `save()` then re-render
- **IDs** generated with `uid()` = timestamp + random (no UUID dependency)
- **XSS prevention** — all user strings rendered through `esc()` helper
- **No frameworks** — keep zero dependencies so the file works offline forever

---

## Contributing / Extending

To add a new module:
1. Add a new `<div id="page-xxx" class="page">` in `index.html`
2. Add a nav tab button that calls `showPage('xxx')`
3. Add the render/save functions following existing patterns
4. Extend the DB schema and `save()`/`load()` as needed

---

*FOOD-MATIC — built for kitchens, not just computers.*
