// ...existing code...

# DataGadgets — USD Sales Aggregation (2024-01-21)

## Table of Contents

- [Scenario & Problem Statement](#scenario--problem-statement)
- [Dataset Description](#dataset-description)
- [Actions & Approach](#actions--approach)
- [Screenshots & Examples](#screenshots--examples)
- [Technologies Used](#technologies-used)
- [Project Structure](#project-structure)
- [Results & Insights](#results--insights)
- [Future Work](#future-work)
- [Acknowledgement](#acknowledgement)
- [Contact](#contact)

## Scenario & Problem Statement

You operate the "DataGadgets" webshop. Orders on 2024-01-21 come in multiple currencies (USD, EUR, GBP). The in-house report must compute the total USD sales for that day by converting non-USD amounts to USD using the exchange rates for 2024-01-21 (sourced from the VAT Comply rates API).

## Dataset Description

The raw orders CSV contains two columns:

- `amount` — numeric price value
- `currency` — ISO currency code (EUR, GBP, USD)

Source file: [data/orders-2024-01-21.csv](data/orders-2024-01-21.csv)

A small helper used to generate the sample dataset is available as [`create_dummy_data`](data/generate_data.ipynb) in [data/generate_data.ipynb](data/generate_data.ipynb).

## Actions & Approach

1. Load the CSV into a pandas DataFrame (`orders`) — see [`orders`](notebooks/notebook.ipynb).
2. Query VAT Comply rates API for base `USD` and date `2024-01-21`:
   - Endpoint: `https://api.vatcomply.com/rates`
   - Query params: `{"base": "USD", "date": "2024-01-21"}`
3. Map each order's `currency` to the API `rates` to create a new `exchange_rate` column.
4. Compute `amount_usd = amount * exchange_rate`.
5. Sum `amount_usd` to get the total USD sales for the day.

## Screenshots & Examples

| index | amount | currency | exchange_rate | amount_usd |
| ----- | ------ | -------- | ------------- | ---------- |
| 0     | 43.75  | EUR      | 0.918527      | 40.185542  |
| 1     | 385.50 | GBP      | 0.788326      | 303.899490 |
| 2     | 495.50 | GBP      | 0.788326      | 390.615298 |
| 3     | 117.99 | GBP      | 0.788326      | 93.014529  |
| 4     | 624.00 | USD      | 1.000000      | 624.000000 |

Example (first 5 rows after conversion) shown in notebook: [`orders` preview and conversions](notebooks/notebook.ipynb).

## Technologies Used

- Python (pandas, requests)
- Jupyter / IPython notebooks
- CSV for dataset
- VAT Comply public API for historical exchange rates

## Project Structure

- [README.md](README.md) — this file
- [data/generate_data.ipynb](data/generate_data.ipynb) — helper to generate `orders-2024-01-21.csv`; contains [`create_dummy_data`](data/generate_data.ipynb)
- [data/orders-2024-01-21.csv](data/orders-2024-01-21.csv) — dataset of orders for 2024-01-21
- [images/image.jpg](images/image.jpg), [images/img_currencylayer_dashboard.png](images/img_currencylayer_dashboard.png), [images/img_env_vars.gif](images/img_env_vars.gif) — media used in the notebook/README
- [notebooks/notebook.ipynb](notebooks/notebook.ipynb) — analysis notebook that performs the conversion and aggregation (defines `orders` and computes `total_usd_sales`)

## Results & Insights

- The notebook computes `amount_usd` per order and then the sum.
- Total USD sales computed in the notebook: **326,864.39599246805 USD** (see [notebooks/notebook.ipynb](notebooks/notebook.ipynb) for calculation).

Quick formula used:

$$
\text{amount\_usd} = \text{amount} \times \text{exchange\_rate}
$$

and

$$
\text{total\_usd\_sales} = \sum \text{amount\_usd}
$$

## Future Work

- Add robust error handling for API failures and missing rates.
- Cache or persist exchange rates to avoid repeated API calls.
- Support more currencies and batch date ranges.
- Add unit tests to validate conversions and rounding.
- Produce daily/weekly aggregated reports and visualisations.

## Acknowledgement

- Exchange rates via VAT Comply API: https://www.vatcomply.com/documentation#rates
- Dataset generator and example notebook included in this repo.

## Contact

Maintainer: DataGadgets analytics team  
Project files and examples:

- Notebook: [notebooks/notebook.ipynb](notebooks/notebook.ipynb)
- Data: [data/orders-2024-01-21.csv](data/orders-2024-01-21.csv)
- Script to regenerate sample data: [data/generate_data.ipynb](data/generate_data.ipynb)

// ...existing code...
