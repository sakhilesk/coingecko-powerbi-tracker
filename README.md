readme_content = """<div align="center">
  <h1 align="center">Live Cryptocurrency Market Analytics Dashboard</h1>
  <p align="center">
    <strong>An End-to-End Power BI Data Engineering & Business Intelligence Project</strong>
  </p>
  <br />
  <p align="center">
    <a href="https://app.powerbi.com/groups/me/reports/031f5247-dd84-47b6-99f8-79f11974eec8/7afb55fca0f9f413e212?experience=power-bi" target="_blank">
      <img src="https://img.shields.io/badge/View_Live_Dashboard-Power_BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black" alt="View Live Dashboard" />
    </a>
  </p>
</div>

<hr />

## 📖 Overview

This project is a fully automated, end-to-end **Power BI** analytics dashboard that tracks real-time cryptocurrency market data and 30-day historical pricing trends. It connects directly to the **CoinGecko Public REST API**, bypassing the need for premium data feeds or intermediate databases. 

The dashboard provides actionable intelligence on market capitalization, trading volume, asset dominance, and market breadth (sentiment), allowing investors or analysts to quickly gauge the health of the digital asset market.

## 🎯 Business Problem Solved

**The Challenge:**
Cryptocurrency markets operate 24/7 and are highly volatile. Analysts and portfolio managers often struggle to consolidate real-time pricing, market breadth (gainers vs. losers), and historical trends into a single view without paying for expensive Bloomberg terminals or enterprise API subscriptions.

**The Solution:**
This Power BI dashboard solves this by:
1. **Automating Data Ingestion:** Connecting directly to an open-source JSON web API.
2. **Providing Real-Time Sentiment:** Using DAX-driven KPIs to calculate "Market Breadth" (how many coins are in the green today).
3. **Visualizing Asset Dominance:** Utilizing inline matrix data bars to show how much capital is concentrated in the top 10 assets.
4. **Historical Context:** Combining real-time snapshot data with a dynamically generated 30-day historical trend line.

---

## 🏗️ Architecture & Detailed Workflow

### 1. Loading the Data (API Ingestion)
The project relies on two primary endpoints from the [CoinGecko API v3](https://www.coingecko.com/en/api/documentation). Data is ingested directly into Power BI using the built-in Web connector.
* **Real-Time Markets Data:** Navigated to `Get Data > Web > Advanced` and connected to the `/coins/markets` endpoint (configured with URL parameters `vs_currency=usd`, `order=market_cap_desc`, and `per_page=100`) to pull a live snapshot of the top 100 coins by Market Cap.
* **Historical Data:** Connected to the `/coins/{id}/market_chart` endpoint (configured with `days=30` and `interval=daily`) to pull a 30-day trailing price array for specific assets.
* *Authentication:* Handled natively via Anonymous web requests, bypassing complex API key management for these public endpoints.

### 2. Transforming the Data (Power Query Editor)
API responses are natively returned as unstructured, nested JSON arrays. Heavy transformation was applied in the Power Query Editor to clean and build a relational schema:
* **Parsing JSON & Expanding Records:** Converted the initial `List` response into a Table format. The `Record` columns were then expanded to extract specific, granular attributes (e.g., `id`, `symbol`, `current_price`, `market_cap`, `total_volume`, `price_change_percentage_24h`).
* **Data Type Enforcement:** Explicitly cast fields to optimize the VertiPaq DAX engine. `current_price` and `market_cap` were converted to Fixed Decimal Numbers, while `symbol` and `id` were cast as Text.
* **Unix Timestamp Conversion:** The historical API returns timestamps in Unix milliseconds, which Power BI cannot natively read as dates. A custom `M` language column was engineered to convert this to standard DateTime:
