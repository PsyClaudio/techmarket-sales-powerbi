# TechMarket — Retail Sales Analysis with Power BI

Power BI portfolio project developed by **Claudio Lombardi** for the Business Intelligence with Power BI course in the ProfessionAI Master in Data Analytics.

The report explores 2014 sales for TechMarket, an educational electronics retail case study, with a dedicated analysis of the returns available for January and February.

## Business questions

- How do sales revenue and units sold change throughout the year?
- Which cities, stores and products contribute to sales?
- How much do discounts reduce gross sales revenue?
- How do the recorded returns affect sales in January and February?

## Report pages

| Page | Purpose |
| --- | --- |
| Home | Entry point and report navigation |
| Vendite per Mese | Monthly units sold, gross revenue, revenue after discounts and weighted discount rate |
| Unità Vendute per Città | Geographic comparison of sales volumes and revenue |
| Dettaglio Prodotto | Product-level sales, pricing and discounts |
| Informazioni sui Negozi | Store information and sales by store and month |
| Vendite Nette con Resi | January–February sales and returns, with city and store dropdowns |

The report interface and model field names are in Italian.

## Data and preparation

The supplied course dataset includes twelve monthly sales CSV files for 2014, two returns CSV files for January and February, and Excel reference tables for stores, products and Italian provinces.

Power Query combines monthly files, assigns data types, cleans reference data and merges product information into sales. The model connects sales to store and product information, and links returns to their original sales transactions. The product table remains available as a dimension because report visuals and filters use it.

The inspected model contains **1,453 sales rows**, **32 stores**, **12 products** and **27 return records**.

## Verified reference figures

These figures assume no city or product selections. The returns page additionally restricts the sales transaction date to 1 January–28 February 2014.

| Metric | Period | Value |
| --- | --- | ---: |
| Units sold | Full year 2014 | 349,384 |
| Gross sales revenue | Full year 2014 | 78,824,066.00 |
| Revenue after discounts, before returns | Full year 2014 | 70,434,103.30 |
| Units sold | January–February 2014 | 77,778 |
| Recorded returned units | January–February analysis | 1,677 |
| Returned units / units sold | January–February analysis | 2.16% |
| Weighted discount rate | January–February 2014 | 14.48% |

Revenue figures are sales amounts, not profit. Returns are linked to sales transactions, so monthly comparisons on the returns page use the original sales transaction date rather than the return processing date. Return records are only available for January and February; the project does not establish a full-year return rate.

## Main DAX measures

Gross sales are calculated row by row, avoiding multiplication of separately aggregated prices and quantities:

```dax
Vendita Lorda (Totale) =
SUMX(
    'Vendite',
    'Vendite'[Prezzo Unitario] * 'Vendite'[Unità Vendute]
)
```

```dax
Sconto Totale =
SUMX(
    'Vendite',
    'Vendite'[Prezzo Unitario]
        * 'Vendite'[Unità Vendute]
        * 'Vendite'[Sconto]
)
```

```dax
Vendita Netta =
[Vendita Lorda (Totale)] - [Sconto Totale]
```

```dax
Sconto Medio Ponderato =
DIVIDE([Sconto Totale], [Vendita Lorda (Totale)])
```

```dax
% Unità Rese =
DIVIDE(SUM('Resi'[Unità Rese]), SUM('Vendite'[Unità Vendute]))
```

The return-rate measure must be interpreted within a consistent period and filter context. The returns page uses a January–February sales-date filter. Returned revenue is valued using the original transaction's unit price and discount; `Vendita Netta Reale` subtracts this amount from revenue after discounts.

## Opening the report

1. Download `Report TechMarket Portfolio.pbix` and open it in Microsoft Power BI Desktop.
2. Explore the saved report using the navigation buttons and slicers.
3. To refresh the data, obtain the original course source files and extract them locally. The report's source paths refer to the author's computer and must be changed before refreshing elsewhere.
4. In Power Query, update the sales folder, returns folder and the three Excel file sources to their local locations. Keep the sales and returns folders separate.
5. Apply the changes and refresh. Check that the January–February filter remains in place on the returns page.

GitHub hosts the downloadable report; this project does not include a public interactive Power BI Service link. Source data availability and redistribution depend on the course materials; downloading the report alone does not supply separate refresh source files.

## Portfolio revision

Following tutor feedback, the report was revised to use row-level revenue calculations, consistent weighted discount indicators, a defined returns-analysis period and clearer city/store controls. Visuals and navigation bookmarks were updated to use the revised measures.

## Skills demonstrated

Power BI Desktop · Power Query · DAX · Data modeling · CSV and Excel data integration · Interactive reporting · KPI validation

## Author

**Claudio Lombardi** — [GitHub portfolio](https://github.com/PsyClaudio)
