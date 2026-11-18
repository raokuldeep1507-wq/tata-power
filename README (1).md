# Tata Power EV Charging, Bengaluru: Data Analysis Project

Analysis of EV charging demand, customers and the proposed expansion plan for Bengaluru, based on the workbook `Tata_power_project_kuldeep.xlsx`.

- **Data window:** 11 Aug 2024 – 8 Nov 2024 (89 days)
- **Scope:** 114,000 charging sessions, 65 stations (390 plugs), 8,000 customers, 18 districts, 98 proposed expansion projects

---

## 1. What is in this project

| File | What it is |
|---|---|
| `Tata_power_project_kuldeep.xlsx` | Source workbook: raw data sheets, a pivot sheet and four dashboards |
| `Tata_Power_EV_Charging_Report.pdf` | 8-page report with charts, insights and recommendations |
| `README.md` | This file |

### Sheets in the workbook

| Sheet | Rows | Contents |
|---|---|---|
| `charging_sessions` | 114,000 | One row per session: customer, station, start time, kWh, tariff, total cost |
| `charging_stations` | 65 | District, income tier, operator, plug count, latitude/longitude |
| `customers` | 8,000 | Income tier, car model, battery capacity (kWh), battery band |
| `districts` | 18 | Income tier, projected EVs, projected plugs, projected regular customers |
| `roi_bengaluru_district` | 98 | Proposed projects: charger type, capex components, revenue, EBITDA, payback, subsidy, 5-year ROI, priority tier |
| `pivot table` | – | Helper tables feeding the dashboards |
| `Dashboard 1` – `Dashboard 4` | – | Overview, station and plug performance, customer and EV insights, ROI and expansion planning |

---

## 2. Key numbers

| Metric | Value |
|---|---|
| Sessions | 114,000 |
| Energy delivered | 28.6 lakh kWh |
| Revenue | ₹4.97 crore (about ₹20.4 crore a year at the current run-rate) |
| Average per session | ₹436 and 25.1 kWh |
| Daily run-rate | about 1,280 sessions and ₹5.6 lakh |
| Proposed investment | ₹139.2 crore across 98 projects (889 guns) |
| Portfolio payback / 5-year ROI (capex-weighted) | 4.3 years / 16.3% |

---

## 3. Main findings

1. **Demand is flat.** Sessions per day are the same in September, October and early November, and the same on every day of the week.
2. **Price drives revenue, not volume.** Each plug delivers about 82 kWh a day at ₹13.5, ₹18 and ₹22.5 per kWh. High-income districts earn 67% more per plug only because of the tariff.
3. **Daytime is the peak.** 50% of sessions start between 08:00 and 15:59; 30% start between 20:00 and 07:59.
4. **Battery size defines customer value.** Premium-EV owners (60+ kWh) are 12% of customers and 26% of revenue.
5. **Tata Power has one of its 13 stations in a High-tier district;** every other operator has three. Demand per station is the same for all operators.
6. **Charger type decides ROI.** 7.4 kW AC: 126%, 22 kW AC: 60%, 50 kW DC: −17%, 150 kW DC: −50%.
7. **Start with the 54 High-priority projects** (₹49.8 crore, 2.6-year payback) and hold the 37 Low-priority, DC-heavy projects until throughput assumptions are tested.
8. **Dashboard 4's "average 5-year ROI of 41.4%" is a simple average.** Weighted by capex it is 16.3%.

The full discussion and recommendations are in the PDF report.

---

## 4. How the numbers were calculated

- **Revenue** uses `total_cost`. The `cost_per_kwh` column is rounded (₹14 and ₹22), while `total_cost` is billed at ₹13.5, ₹18 and ₹22.5, the same tariffs as the ROI sheet.
- **Per-day figures** are used for monthly comparison because August (from the 11th) and November (to the 8th) are partial months.
- **Annualised revenue** = revenue ÷ 89 days × 365.
- **Utilisation proxy** = sessions per plug per day, and kWh per plug per day. True time-based utilisation cannot be computed because the data has no session duration.
- **5-year ROI** (as in the sheet) = (5 × annual EBITDA − total capex) ÷ total capex, before subsidy.
- **Weighted payback and ROI** are computed on summed capex and summed EBITDA, not by averaging project percentages.
- **Plug gap** = projected plugs (from `districts`) − existing plugs (6 per station). One proposed gun is treated as one plug.
- **Implied utilisation** in the ROI sheet = (annual revenue ÷ tariff) ÷ (guns × charger kW × 8,760 hours).

---

## 5. Checking the key numbers in Excel

Header is in row 1 of every data sheet. Paste these formulas into any empty cell of the workbook.

| What | Formula |
|---|---|
| Total sessions | `=COUNTA(charging_sessions!A2:A114001)` |
| Total revenue (₹) | `=SUM(charging_sessions!G2:G114001)` |
| Total energy (kWh) | `=SUM(charging_sessions!E2:E114001)` |
| Average revenue per session (₹) | `=AVERAGE(charging_sessions!G2:G114001)` |
| Unique customers | `=COUNTA(customers!A2:A8001)` |
| Total capex (₹ lakh) | `=SUM(roi_bengaluru_district!M2:M99)` |
| Total annual EBITDA (₹ lakh) | `=SUM(roi_bengaluru_district!Q2:Q99)` |
| Weighted payback (years) | `=SUM(roi_bengaluru_district!M2:M99)/SUM(roi_bengaluru_district!Q2:Q99)` |
| Weighted 5-year ROI (%) | `=(5*SUM(roi_bengaluru_district!Q2:Q99)-SUM(roi_bengaluru_district!M2:M99))/SUM(roi_bengaluru_district!M2:M99)*100` |
| Simple average 5-year ROI (%), as on Dashboard 4 | `=AVERAGE(roi_bengaluru_district!V2:V99)` |

To explore further, use **Insert > PivotTable** on `charging_sessions` (add station details by looking up `station_id` in `charging_stations`) or on `roi_bengaluru_district` (for example Charger_Type or District in Rows, and the sum of Total_CAPEX_Lakh and Annual_EBITDA_Lakh in Values).

The workbook is about 27 MB, so it can take a few seconds to open and recalculate.

---

## 6. Limitations

- **The data is unusually uniform:** station volumes vary by a few percent, every weekday is identical, hourly demand moves in flat blocks, and customers do not favour any district. Confirm how the dataset was produced before treating behaviour findings (price sensitivity, no loyalty) as market facts.
- **Missing fields:** session duration, charger power, plug type and availability. Real utilisation, queuing and lost sales cannot be measured.
- **Price is tied to location,** so the data cannot prove price elasticity. A pilot is needed.
- **The ROI model** gives every charger type about the same throughput (about 82 kWh per gun per day), which may be too optimistic for 7.4 kW AC and too pessimistic for DC fast. It also ignores ramp-up, tax, discounting and equipment replacement, and is before subsidy.

---

## 7. Suggested next steps

1. Validate the throughput assumption with a pilot wave of the High-priority AC projects.
2. Re-scope Koramangala, Indiranagar and Whitefield with AC chargers.
3. Pilot a ₹1–2 per kWh tariff change at a few Mid-Range stations.
4. Add session duration and charger power to the sessions table.
5. Show capex-weighted ROI and payback next to the simple averages on Dashboard 4.
