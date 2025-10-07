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


## Technologies Used

- Python (pandas, requests)
- Jupyter / IPython notebooks
- CSV for dataset
- VAT Comply public API for historical exchange rates

## Project Structure

```
├── data/
│   ├── generate_data.ipynb
│   └── orders-2024-01-21.csv
├── images/
│   ├── image.jpg
│   ├── img_currencylayer_dashboard.png
│   └── img_env_vars.gif
├── notebooks/
│   └── notebook.ipynb
├── README.md                 # Project documentation
```
## Results & Insights

- A Python script that incorporates an API call to convert various currencies to USD rates.
- The notebook computes `amount_usd` per order and then the sum.
- Total USD sales computed in the notebook: **326,864.39599246805 USD**

## Future Work

- Cache or persist exchange rates to avoid repeated API calls.
- Support more currencies and batch date ranges.
- Add unit tests to validate conversions and rounding.
- Produce daily/weekly aggregated reports and visualisations.

## Acknowledgement

- Exchange rates via VAT Comply API: https://www.vatcomply.com/documentation#rates
- Problem statements and/or datasets in this project were sourced from DataCamp real-world projects.

## Contact

**Reynaldo III Castillo**

- **Email:** reynaldoiii.castillo@gmail.com
- **LinkedIn:** [linked.com/reynaldo-iii-castillo](https://www.linkedin.com/in/reynaldo-iii-castillo-975120303)