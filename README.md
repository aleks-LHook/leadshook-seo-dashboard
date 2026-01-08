# Leadshook SEO Dashboard

A retro NES-themed SEO analytics dashboard for tracking Share of Search and Brand Growth Rate metrics.

## Quick Start

1. Start a local web server in this directory:
   ```bash
   python -m http.server 8000
   ```
   Or use VS Code Live Server extension.

2. Open `http://localhost:8000/share-of-search.html` in your browser.

> **Note:** The dashboards must be served from a web server due to CSV loading (CORS restrictions prevent loading from `file://`).

## Dashboard Pages

### Share of Search (`share-of-search.html`)

Tracks your brand's search volume as a percentage of total competitor search volume.

**Features:**
- Monthly view with bar chart showing all competitors
- Trend analysis with forecasting
- Two competitor sets: Direct Competitors and Enterprise Players
- Interactive table with sorting and filtering

**Key Metrics:**
- **Share of Search** - Leadshook's percentage of total branded search volume
- **Leadshook Volume** - Monthly branded searches
- **Total Market** - Combined search volume of all tracked brands
- **Market Rank** - Position among competitors by search volume

### Brand Growth Rate (`brand-growth-rate.html`)

Compares your brand's search growth against category keyword growth.

**Features:**
- Year-over-Year, Month-over-Month, and Quarter-over-Quarter comparisons
- Growth trend visualization
- Category keyword breakdown by group

**Key Metrics:**
- **Growth Delta** - Difference between brand and category growth rates
- **Brand Volume** - Total branded search volume
- **Category Volume** - Total category keyword volume
- **Brand Share** - Brand volume as percentage of category

## Data Files

All data is stored in CSV files in the `data/` folder.

### `data/direct-competitors.csv`

Share of Search data for quiz/lead generation competitors.

```csv
month,brand,keyword,volume
November 2025,Leadshook,leadshook,500
November 2025,Leadshook,leads hook,150
November 2025,Outgrow,outgrow,1900
```

| Column | Description |
|--------|-------------|
| month | Full month name and year (e.g., "November 2025") |
| brand | Brand name (must be consistent across rows) |
| keyword | The search keyword being tracked |
| volume | Monthly search volume from Ahrefs |

### `data/enterprise-players.csv`

Share of Search data for enterprise form/survey platforms. Same format as above.

### `data/brand-keywords.csv`

Leadshook branded keyword volumes for growth rate tracking.

```csv
month,keyword,volume
November 2024,leadshook,420
November 2024,leads hook,130
November 2025,leadshook,500
```

| Column | Description |
|--------|-------------|
| month | Full month name and year |
| keyword | Branded search term |
| volume | Monthly search volume |

### `data/category-keywords.csv`

Category (non-branded) keyword volumes for growth comparison.

```csv
month,group,keyword,volume
November 2024,Funnel Builder,funnel builder,552
November 2024,Landing Page Builder,landing page builder,3220
November 2025,Quiz & Lead Gen,quiz funnel software,150
```

| Column | Description |
|--------|-------------|
| month | Full month name and year |
| group | Category group name (for breakdown display) |
| keyword | Category search term |
| volume | Monthly search volume |

## Monthly Update Process

### Automated Update (Recommended)

Use the Ahrefs MCP integration with Claude Code to automatically fetch real-time search volumes:

1. **Connect to Ahrefs MCP**
   ```bash
   claude /mcp
   # Select "ahrefs" when prompted
   ```

2. **Run the Update Command**
   ```
   Update the [MONTH YEAR] data by using the ahrefs mcp to get real time volumes
   ```
   Example: "Update the January 2026 data by using the ahrefs mcp to get real time volumes"

3. **Verify the Updates**
   - Check `data/direct-competitors.csv` - all competitor branded keywords
   - Check `data/enterprise-players.csv` - enterprise platform keywords
   - Check `data/brand-keywords.csv` - Leadshook branded keywords
   - Check `data/category-keywords.csv` - category keywords

4. **Refresh Your Dashboard**
   - Open the dashboard in your browser
   - Hard refresh (Ctrl+F5 or Cmd+Shift+R)
   - Verify all metrics are updated

### Manual Update (Backup Method)

If you need to update data manually without the API:

#### 1. Get Data from Ahrefs

For each keyword you're tracking:
1. Go to Ahrefs Keywords Explorer
2. Enter the keyword
3. Note the search volume for your target country (US)

#### 2. Update Share of Search Data

Add new rows to `data/direct-competitors.csv` and/or `data/enterprise-players.csv`:

```csv
January 2026,Leadshook,leadshook,520
January 2026,Leadshook,leads hook,160
January 2026,Outgrow,outgrow,1950
...
```

#### 3. Update Brand Growth Data

Add new rows to `data/brand-keywords.csv`:

```csv
January 2026,leadshook,520
January 2026,leads hook,160
January 2026,leadhook,55
January 2026,lead hooks,35
```

#### 4. Update Category Growth Data

Add new rows to `data/category-keywords.csv`:

```csv
January 2026,Funnel Builder,funnel builder,620
January 2026,Funnel Builder,sales funnel software,610
January 2026,Landing Page Builder,landing page builder,3600
...
```

## Adding a New Competitor

1. Open the relevant CSV file (e.g., `data/direct-competitors.csv`)
2. Add rows for each keyword the competitor ranks for:
   ```csv
   November 2025,NewCompetitor,newcompetitor,800
   November 2025,NewCompetitor,new competitor,200
   ```
3. Add historical data if available for trend analysis
4. The dashboard will automatically assign a color and include them in charts

## Adding a New Category Group

1. Open `data/category-keywords.csv`
2. Add rows with a new group name:
   ```csv
   November 2025,New Category,keyword one,500
   November 2025,New Category,keyword two,300
   ```
3. The dashboard will automatically create a new breakdown section

## File Structure

```
Ahrefs Data/
├── README.md                    # This file
├── share-of-search.html         # Share of Search dashboard
├── brand-growth-rate.html       # Brand Growth Rate dashboard
├── nes-theme.css                # Shared NES retro styling
└── data/
    ├── direct-competitors.csv   # Quiz/Lead Gen competitor data
    ├── enterprise-players.csv   # Enterprise platform data
    ├── brand-keywords.csv       # Leadshook branded keywords
    └── category-keywords.csv    # Category keywords for growth
```

## Styling

The dashboards use a retro NES (Nintendo Entertainment System) theme:
- **Font:** Press Start 2P (Google Fonts)
- **Primary Color:** #e76e55 (Leadshook red)
- **Accent Colors:** #92cc41 (green), #209cee (blue), #f7d51d (yellow)

Styles are shared via `nes-theme.css`. The navbar provides navigation between dashboards.

## Troubleshooting

### "Failed to load CSV data" Error

- Make sure you're running from a web server, not opening the HTML file directly
- Check that CSV files exist in the `data/` folder
- Verify CSV format (headers must match exactly)

### Data Not Showing

- Check browser console for errors (F12 → Console)
- Verify month format is "Month YYYY" (e.g., "November 2025")
- Ensure no trailing commas or empty rows in CSV

### Charts Not Rendering

- Clear browser cache and refresh
- Check that Chart.js CDN is accessible
- Verify data has at least one month of data
