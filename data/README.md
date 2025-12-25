# Data Requirements and Structure

## Purpose of the data

The system operates on historical sales data of pharmaceutical products
and is designed to build demand forecasts for individual products.

Each forecast is generated independently for a single product
without aggregation across products, categories, or pharmacies.

---

## Data sources

The system expects structured tabular sales data obtained from
pharmacy accounting or sales reporting systems.

Typical sources include:
- local exports from pharmacy management systems,
- sales reports in CSV or Excel format,
- aggregated transactional sales data.

The system does not rely on direct database connections or APIs
and operates on locally stored files.

---

## Forecasting granularity

Forecasts are built at the **individual product level**.

Key characteristics:
- each product is modeled independently,
- no cross-product aggregation is performed,
- no hierarchical or grouped forecasting is assumed.

A single time series corresponds to one product.

---

## Expected data structure

The input dataset is expected to contain the following fields:

| Field name        | Type        | Required | Description |
|------------------|-------------|----------|-------------|
| `date`           | Date        | Yes      | Date of sales observation |
| `product_id`     | String / ID | Yes      | Unique identifier of the product |
| `sales_quantity` | Numeric     | Yes      | Number of units sold on the given date |

Optional fields may include:
- pharmacy identifier,
- product name,
- additional metadata.

However, these fields are not required for forecasting logic.

---

## Time series requirements

- The time index must be explicitly provided via the `date` field
- Observations are expected to be ordered in time
- The system supports fixed time steps (e.g., daily, weekly, monthly)
- Missing dates should be explicitly represented or handled during preprocessing

Each product time series must be sufficiently long to allow
model training and evaluation.

---

## Data assumptions and limitations

The system assumes that:
- sales data reflects actual realized demand,
- values in `sales_quantity` are non-negative,
- returns, write-offs, and corrections are either excluded or preprocessed.

The system does not model:
- pricing effects,
- promotions or marketing campaigns,
- substitutions between products.

These factors are considered outside the current scope.

---

## Data handling policy

Real sales data, proprietary datasets, and any confidential information
are intentionally not stored or published in this repository.

The repository may include:
- data format descriptions,
- schemas,
- synthetic or illustrative sample data.

All real-world data remains outside the version-controlled codebase.
