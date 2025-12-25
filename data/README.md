# Data Requirements and Interaction

This document describes how AI-Pharm Analytics works with data,
what input formats are supported, and how input data is transformed
for forecasting and analytics.

The system is designed to accept real-world pharmacy data in familiar
formats and automatically convert them into a unified internal
representation suitable for forecasting models and future integrations.

---

## General approach

AI-Pharm Analytics follows a two-stage data workflow:

1. **User-facing input data**  
   Data provided in formats commonly used in pharmacies (e.g., Excel reports).

2. **Internal canonical data format**  
   A standardized representation used internally for modeling,
   evaluation, and integration.

This approach allows pharmacy staff to work with familiar data formats
while ensuring consistency and scalability inside the system.

---

## Input data format (Excel / wide format)

### Description

The primary supported input format is a spreadsheet-style table,
widely used in pharmacy practice.

Typical structure:
- rows represent individual products,
- columns represent calendar dates,
- cell values represent quantities sold.

### Example (input Excel)

| Product name | 2025-01-01 | 2025-01-02 | 2025-01-03 |
|--------------|------------|------------|------------|
| Nurofen 200  | 3          | 1          | 0          |
| Salbutamol  | 2          | 2          | 1          |

### Interpretation

- Product name — external product identifier (as provided by the user)
- Date columns — sales dates
- Cell values — number of units sold on the given date

This format does not require preprocessing by the user and reflects
how sales data is typically stored in pharmacies.

---

## Product identification

Product names or labels provided in input files are not assumed
to be stable identifiers.

Internally, the system assigns each product a stable **`product_key`**.

Identification logic:
- If a stable identifier (barcode, internal system ID) is provided,
  it is used directly.
- Otherwise, the system creates and maintains a mapping between
  user-provided product names and internal `product_key` values.

This ensures continuity of time series across multiple uploads.

---

## Internal data format (canonical representation)

Before forecasting, all input data is transformed into a unified
internal format.

### Canonical sales table (example)

| date       | product_key | product_name | sales_quantity |
|------------|-------------|--------------|----------------|
| 2025-01-01 | PRD_001     | Nurofen 200  | 3              |
| 2025-01-02 | PRD_001     | Nurofen 200  | 1              |
| 2025-01-01 | PRD_002     | Salbutamol  | 2              |

Each row represents sales of a single product on a single date.

All forecasting models operate exclusively on this internal format.

---

## Additional factors (optional)

The system supports optional additional factors that may influence demand,
such as:
- promotional activity indicators,
- stock availability (out-of-stock),
- pricing information,
- weather-related variables.

These factors are provided as separate datasets and aligned by date
and product (or location) before modeling.

---

## Assumptions and limitations

The system assumes that:
- sales data reflects realized demand,
- quantities are non-negative,
- corrections (returns, write-offs) are handled before data upload.

The system does not explicitly model:
- product substitution effects,
- pricing strategy optimization,
- marketing planning logic.

---

## Data handling policy

Real sales data and commercially sensitive information are not stored
or published in this repository.

The repository may contain:
- data format descriptions,
- schemas and examples,
- synthetic or illustrative data only.
