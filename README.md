readme_content = """<div align="center">
  <h1 align="center">Live Cryptocurrency Market Analytics Dashboard</h1>
  <p align="center">
    <strong>An End-to-End Power BI Data Engineering & Business Intelligence Project</strong>
  </p>
</div>

<hr />

## 📖 Overview

This project is a fully automated, end-to-end **Power BI** analytics dashboard that tracks real-time cryptocurrency market data and 30-day historical pricing trends. It connects directly to the **CoinGecko Public REST API**, bypassing the need for premium data feeds. 

The dashboard provides actionable intelligence on market capitalization, trading volume, asset dominance, and market breadth (sentiment), allowing investors or analysts to quickly gauge the health of the digital asset market.

## 🎯 Business Problem Solved

**The Challenge:**
Cryptocurrency markets operate 24/7 and are highly volatile. Analysts and portfolio managers often struggle to consolidate real-time pricing, market breadth (gainers vs. losers), and historical trends into a single view without paying for expensive Bloomberg terminals or enterprise API subscriptions.

**The Solution:**
This Power BI dashboard solves this by:
1. **Automating Data Ingestion:** Connecting directly to an open-source JSON web API and utilizing Power Query to transform nested arrays into a relational model.
2. **Providing Real-Time Sentiment:** Using DAX-driven KPIs to calculate "Market Breadth" (how many coins are in the green today) rather than just looking at Bitcoin's price.
3. **Visualizing Asset Dominance:** Utilizing inline matrix data bars to show how much capital is concentrated in the top 10 assets versus the rest of the top 100.
4. **Historical Context:** Combining real-time snapshot data with a dynamically generated 30-day historical trend line for specific assets.

## 🏗️ Architecture & Workflow

1. **Data Source:** [CoinGecko API v3](https://www.coingecko.com/en/api/documentation)
   - *Endpoint 1 (Real-Time):* `/coins/markets` (Top 100 coins by Market Cap)
   - *Endpoint 2 (Historical):* `/coins/{id}/market_chart` (30-day trailing price)
2. **Data Extraction (Power Query):** - HTTP Web requests with dynamic parameters.
   - Parsing nested JSON `List` and `Record` objects.
3. **Data Transformation:** - Expanding JSON columns.
   - Converting Unix millisecond timestamps to standard DateTime using custom Power Query `M` formulas (`#datetime(1970, 1, 1, 0, 0, 0) + #duration(0, 0, 0, [Timestamp] / 1000)`).
   - Strict data typing for accurate DAX aggregation.
4. **Data Modeling & DAX:** Creation of explicit measures to track total capitalization, dominance percentages, and moving averages.
5. **Visualization:** Dark-mode UI layout prioritizing Tufte's data-ink ratio principles.

---

## 💻 DAX Measures Reference

Below is the complete DAX code library engineered for this data model.

### 1. Market Health & KPIs

**Total Market Capitalization**
Calculates the combined valuation of the top 100 tracked digital assets.
