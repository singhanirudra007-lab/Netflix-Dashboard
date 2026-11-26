# 🎬 Netflix Content Dashboard

An interactive, single-page analytics dashboard that explores a Netflix-style content catalog of **100 titles** (movies and TV shows). It is built with plain **HTML, CSS and JavaScript** — no frameworks, no build step, no chart libraries — and styled in Netflix's brand colors on a dark UI inspired by modern SaaS dashboard designs.

> **Live demo:** `https://<your-username>.github.io/<repo-name>/` *(replace after enabling GitHub Pages, see [Deployment](#-deployment-github-pages))*

---

## 📌 Table of Contents
1. [Overview](#-overview)
2. [Features](#-features)
3. [Dashboard Sections](#-dashboard-sections)
4. [Tech Stack](#-tech-stack)
5. [Project Structure](#-project-structure)
6. [Dataset](#-dataset)
7. [How It Works](#-how-it-works)
8. [Getting Started](#-getting-started)
9. [Deployment (GitHub Pages)](#-deployment-github-pages)
10. [Updating the Data](#-updating-the-data)
11. [Customization](#-customization)
12. [Design System](#-design-system)
13. [Responsive Behavior](#-responsive-behavior)
14. [Limitations](#-limitations)
15. [Future Improvements](#-future-improvements)
16. [Author & License](#-author--license)

---

## 🔍 Overview

The project turns a spreadsheet (`Netflix_Content_Dashboard.xlsx`) into a live, filterable dashboard. Instead of static Excel charts, every number, chart and table re-computes instantly when you change a filter, so you can answer questions like:

- Which genres and maturity ratings pull the most views?
- How did the catalog and viewership grow by the year titles were added?
- Which countries drive the most viewership?
- Which individual titles are top performers, and how do they rate on IMDb?

The whole dashboard lives in **one `index.html` file**, so it works offline and deploys anywhere static files can be hosted.

## ✨ Features

- **Live filtering** – All / Movies / TV Shows toggle, country dropdown and free-text search (matches title, genre or country).
- **Four KPI cards** – Total Titles, Total Views, Average IMDb Score, Average Views per Title, each with a context badge.
- **Genre × Maturity heatmap** – hatched-to-solid red intensity scale, with a legend and hover tooltips.
- **Smooth line chart** – views and titles added per year, drawn as inline SVG with a hover crosshair and tooltip (touch supported).
- **Clickable country tiles** – click a country to filter the whole dashboard, click again to clear.
- **Top titles table** – top 7 titles for the current filter, with a view-share bar, IMDb score, type and country.
- **Light and dark theme** – follows the system `prefers-color-scheme` setting.
- **Fully responsive** – desktop, tablet and phone layouts.
- **Zero dependencies** – only the Inter font is loaded from Google Fonts (with a system fallback).

## 🧩 Dashboard Sections

| Section | What it shows | Interaction |
|---|---|---|
| **Top bar** | Search, type toggle, country dropdown, live date | Filters everything |
| **KPI cards** | Titles, total views (M), avg IMDb, avg views/title | Update with every filter |
| **Views by genre & maturity** | Heatmap of summed views per genre and rating (TV-PG, PG-13, TV-14, R, TV-MA) | Hover for exact values |
| **Views by year added** | Two smooth lines: views (red) and titles added (purple) | Hover/touch for crosshair tooltip |
| **Views by country** | Tiles for each of the 6 countries with views and title count | Click to filter |
| **Top titles** | Ranked table of the highest-viewed titles | Reacts to all filters |

### About the KPI badges
The source data has no time-series "growth" metric, so badges show context that *can* be computed honestly:
- **Titles / Views** → share of the full catalog (e.g. `42% of catalog`)
- **Avg IMDb / Avg views** → difference versus the overall average (green ↗ above, red ↘ below)

## 🛠 Tech Stack

| Layer | Choice |
|---|---|
| Markup | HTML5 |
| Styling | CSS3 (Grid, Flexbox, CSS variables, media queries) |
| Logic | Vanilla JavaScript (ES6+) |
| Charts | Hand-written inline **SVG** (Catmull-Rom → Bézier smoothing for the line chart) |
| Font | [Inter](https://fonts.google.com/specimen/Inter) via Google Fonts |
| Hosting | GitHub Pages (static) |

## 📁 Project Structure

```
netflix-content-dashboard/
├── index.html                       # The complete dashboard (HTML + CSS + JS + embedded data)
├── data/
│   ├── Netflix_Content_Dashboard.xlsx   # Original source workbook
│   └── netflix_content.csv              # Clean export of the "Data" sheet (100 rows)
├── README.md
├── LICENSE
└── .gitignore
```

## 📊 Dataset

The `Data` sheet of the workbook contains **100 titles** with these columns:

| Column | Description |
|---|---|
| Show ID | Unique ID (`s1`–`s100`) |
| Title | Name of the title |
| Type | `Movie` or `TV Show` |
| Genre | Drama, Comedy, Action, Thriller, Documentary, Romance, Sci-Fi, Horror |
| Country | USA, UK, India, Spain, Japan, South Korea |
| Release Year | Year the title was released |
| Date Added | Date it was added to the catalog |
| Maturity Rating | TV-PG, PG-13, TV-14, R, TV-MA |
| Duration (min) | Movies only (blank for TV shows) |
| Seasons | TV shows only (blank for movies) |
| IMDb Score | 0–10 rating |
| Views (millions) | Total views in millions |
| Year Added | Year extracted from *Date Added* |

> The dataset is a sample/synthetic catalog used for learning and portfolio purposes — it is **not** official Netflix data.

## ⚙️ How It Works

1. **Embedded data** – the 100 rows are stored as a compact JSON array inside `index.html` (`const D=[...]`). Each row is `[title, type, genre, country, releaseYear, dateAdded, maturity, duration, seasons, imdb, views]`.
2. **Single state object** – `st = { t: type, c: country, q: search }` holds the active filters.
3. **`render()` function** – filters the array, recomputes all aggregates (sums, averages, group-bys) and redraws every widget. It runs on every input change, which is fast enough for this dataset size.
4. **Heatmap** – views are summed per `genre|maturity` pair; cells are bucketed into 4 intensity levels relative to the current maximum.
5. **Line chart** – values are scaled to the SVG viewBox and connected with smooth cubic Bézier curves. Mouse/touch position is snapped to the nearest year to place the crosshair and tooltip.
6. **Country tiles** – calculated on the data filtered by everything *except* country, so you can still see and switch between countries while one is selected.

## 🚀 Getting Started

### Option 1 – Just open it
Download the repo and double-click `index.html`. That's it.

### Option 2 – Local server (optional)
```bash
# Python
python -m http.server 8000

# or Node
npx serve .
```
Then visit `http://localhost:8000`.

## 🌐 Deployment (GitHub Pages)

1. Create a new repository on GitHub (e.g. `netflix-content-dashboard`).
2. Upload all files from this folder (drag-and-drop on GitHub works) or push with Git:
   ```bash
   git init
   git add .
   git commit -m "Add Netflix content dashboard"
   git branch -M main
   git remote add origin https://github.com/<your-username>/netflix-content-dashboard.git
   git push -u origin main
   ```
3. Go to **Settings → Pages**.
4. Under **Build and deployment**, set **Source** to *Deploy from a branch*, pick **main** and **/ (root)**, then **Save**.
5. After a minute your site is live at `https://<your-username>.github.io/netflix-content-dashboard/`.
6. Paste that link at the top of this README.

## 🔄 Updating the Data

The dashboard reads the array in `index.html`, not the Excel file directly. To refresh it:

1. Edit `data/Netflix_Content_Dashboard.xlsx` (keep the same column order).
2. Re-export the `Data` sheet to CSV.
3. Convert the rows to the compact array format and replace `const D=[...]` in `index.html`. A quick Python helper:

```python
import pandas as pd, json
df = pd.read_excel("data/Netflix_Content_Dashboard.xlsx", sheet_name="Data").dropna(subset=["Title"])
rows = [[r["Title"], r["Type"], r["Genre"], r["Country"], int(r["Release Year"]),
         str(r["Date Added"])[:10], r["Maturity Rating"],
         None if pd.isna(r["Duration (min)"]) else int(r["Duration (min)"]),
         None if pd.isna(r["Seasons"]) else int(r["Seasons"]),
         float(r["IMDb Score"]), int(r["Views (millions)"])] for _, r in df.iterrows()]
print(json.dumps(rows, separators=(",", ":")))
```
4. Paste the printed JSON over `const D=[...]`.

If you add new genres, ratings or countries, update the `RT` array (maturity order) and the `FL` object (country flags) near the top of the script.

## 🎨 Customization

All colors are CSS variables at the top of the `<style>` block:

```css
:root{
  --bg:#0b0b0b;      /* page background */
  --panel:#141414;   /* cards and sidebar */
  --card:#1c1c1c;    /* inner tiles */
  --red:#E50914;     /* Netflix red (accent) */
  --tx:#f5f5f1;      /* text */
}
```

Change `--red` to re-theme the whole dashboard. The heatmap shades are set in the `bg` array inside `render()`.

## 🖌 Design System

| Token | Value | Use |
|---|---|---|
| Netflix Red | `#E50914` | Accents, active states, primary line, heatmap peak |
| Dark Red | `#B20710` | Heatmap mid-level |
| Black | `#141414` | Panels |
| Off-white | `#F5F5F1` | Text |
| Positive | `#46D369` | Up badges, high ratings |
| Typeface | Inter | All UI text |

Layout inspiration: a dark "Metric Flow"-style SaaS dashboard (sidebar navigation, KPI row, heatmap + line chart, tiles + table).

## 📱 Responsive Behavior

- **> 1000 px** – full layout: sidebar + 4-column KPIs + two-column chart rows.
- **≤ 1000 px** – sidebar becomes a horizontal strip; KPIs 2×2; charts stack.
- **≤ 520 px** – KPIs and country tiles collapse to a mobile-friendly grid; tables scroll horizontally.

## ⚠️ Limitations

- Data is embedded, so it is a snapshot and does not auto-sync with the Excel file.
- Sidebar items (Titles, Genres, etc.) are visual navigation only.
- Sample dataset of 100 titles; very large datasets would need a charting library and pagination.

## 🔮 Future Improvements

- Load the CSV dynamically (`fetch`) so data updates need no code edits
- Genre and maturity-rating filters
- Release-year vs IMDb scatter plot
- Sortable table columns and pagination
- Manual light/dark toggle
- Export filtered data to CSV

## 👤 Author & License

Built by **Anirudra** as a design + front-end learning project.

Released under the [MIT License](LICENSE).

> *Disclaimer: This is an independent educational project. Netflix and its logo are trademarks of Netflix, Inc.; this project is not affiliated with or endorsed by Netflix.*
