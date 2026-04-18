# Food Delivery · Real-Time Analytics Dashboard
## Stream Analytics — Group 5 · BBADBA A · Milestone 2

Live dashboard visualising real-time insights from a food delivery platform streaming pipeline.

**Live app:** [https://streamanalyticsgroup5.streamlit.app](https://streamanalyticsgroup5.streamlit.app)

---

## What this dashboard shows

The dashboard reads Parquet files written by Apache Spark Structured Streaming directly from Azure Blob Storage and refreshes every 15 seconds. It covers five analytical use cases:

| Section | Use case | Description |
|---|---|---|
| KPI row | All feeds | Total events, delivered orders, total revenue, avg order value, cancellation rate |
| UC1 & UC2 | Orders | Order event type distribution + cancellation reasons and zones |
| UC3 | Orders | Total revenue and average order value by delivery zone |
| UC4 | Couriers | Courier status by zone (stacked) + overall status distribution + live map of Madrid |
| UC5 | Couriers | Anomaly detection — couriers that went offline while carrying an active order |
| Volume | Both feeds | Order and courier event volume per hour showing lunch and dinner peaks |

---

## Pipeline architecture

```
AVRO Producers (Google Colab)
        ↓  SASL_SSL / AVRO
Azure Event Hubs — iesstsabbadbaa-grp-01-05
        group_5_orders · group_5_couriers
        ↓  Spark readStream + from_avro()
Spark Structured Streaming (Google Colab)
        ↓  writeStream → Parquet every 20s
Azure Blob Storage — iesstsabbadbaa / group5
        ↓  read_parquet() every 15s
This Streamlit Dashboard
```

---

## Data feeds

**Feed A: Orders** (`group_5_orders`)
Events: `CREATED → ACCEPTED → PREP_STARTED → READY → PICKED_UP → DELIVERED / CANCELLED`

**Feed B: Couriers** (`group_5_couriers`)
Events: `ONLINE · OFFLINE · LOCATION · ASSIGNED · UNASSIGNED`
Statuses: `IDLE · EN_ROUTE_TO_RESTAURANT · WAITING · EN_ROUTE_TO_CUSTOMER · OFFLINE`

Both feeds cover 5 delivery zones in Madrid — Z1_Center, Z2_North, Z3_South, Z4_East, Z5_West — with Z1_Center weighted at 40% of demand.

---

## Repository structure

```
streamanalyticsgroup5/
├── dashboard.py        ← Streamlit app
├── requirements.txt    ← Python dependencies
└── README.md           ← This file
```

---

## Run locally

```bash
pip install -r requirements.txt
streamlit run dashboard.py
```

Opens at `http://localhost:8501`.

The dashboard connects directly to Azure Blob Storage — no local Spark setup needed. As long as the Spark notebook on Google Colab is running (Section 7 — Parquet writers), the dashboard updates live.

---

## Team

Group 5 — BBADBA A · IE University

| Name | GitHub |
|---|---|
| Bernarda Andrade | @22andradeb |
| Javier Comin | — |
| Nour Farhat | @nour-farhat |
| Sofía Serantes | @sofiaserantes |
| Tessa Correig | @tessacorreigmartra |
| Rakan Hourani | @rakanhourani |

---

## Related repositories

- [Milestone 1 — Data Feed Design & Generation](https://github.com/sofiaserantes/Milestone1_StreamAnalytics_G5)
