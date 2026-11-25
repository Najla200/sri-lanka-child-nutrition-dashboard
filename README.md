# Sri Lanka Child Nutrition Dashboard | Power BI

An interactive dashboard exploring under-five malnutrition across Sri Lankan health reporting areas during **2020–2023**, with national benchmarks and historical district poverty context.

## Dashboard previews

### Overview
<img width="1385" height="781" alt="Overview Page" src="https://github.com/user-attachments/assets/2f077877-ebaa-4ccd-99dd-49195f14e5be" />

### Area Detail
<img width="1390" height="777" alt="AreaDetail Page" src="https://github.com/user-attachments/assets/5dd97c6f-6a72-4070-9fb6-c0b920eec18f" />

## Business question

Which health reporting areas have higher child malnutrition prevalence than the national rate, and how have these differences changed over time?

## Audience

Public health planners and nutrition programme teams exploring regional differences and identifying areas for further investigation.

## Data sources

- **Family Health Bureau — Annual Report 2023, Annexure 11:** under-five underweight, stunting and wasting prevalence by health reporting area, 2020–2023.  
  [Official report](https://fhb.health.gov.lk/storage/resources/annual-report-2023.pdf)
- **Department of Census and Statistics — Poverty Indicators 2019, Table 4:** district poverty headcount percentages under the updated poverty line.  
  [Official report](https://www.statistics.gov.lk/Resource/en/Poverty/PovertyIndicators-2019.pdf)

The nutrition dataset covers **28 health reporting areas**, three indicators and four years, with separate national benchmarks.

## Dashboard features

### Overview

- National prevalence card
- Indicator and year slicers
- Health reporting area ranking
- National comparison across all three nutrition indicators
- National trend from 2020–2023
- Table showing area prevalence, national prevalence and the gap in percentage points

### Area Detail

- Area prevalence, national prevalence and gap cards
- Historical district poverty card
- Area-versus-national comparison across nutrition indicators
- Area and national trends over time
- Area chart showing the gap from national prevalence
- Detail table and health reporting area selector

Users can right-click an area’s bar on Overview and select **Drill through → Area Detail**. Page navigation buttons support movement between pages.

## Data preparation and modelling

Power Query was used to extract the PDF tables, remove report headers and footers, reshape the nutrition data into a long format, standardise geographic names and preserve missing values.

The model contains:

- `Nutrition_Areas`
- `Nutrition_National`
- `HealthArea_Lookup`
- `Indicator_Lookup`
- `Date_Lookup`

Shared date and indicator tables filter the nutrition tables. District poverty values are included in the health area lookup as historical context.

## Key DAX measures

```DAX
Area Prevalence =
SELECTEDVALUE(Nutrition_Areas[PrevalencePct])
```

```DAX
National Prevalence =
SELECTEDVALUE(Nutrition_National[PrevalencePct])
```

```DAX
Gap vs National pp =
IF(
    OR(
        ISBLANK([Area Prevalence]),
        ISBLANK([National Prevalence])
    ),
    BLANK(),
    [Area Prevalence] - [National Prevalence]
)
```

Prevalence values are stored on a **0–100 scale**. The gap is measured in **percentage points (pp)**:

- Positive gap: area prevalence is above the national rate.
- Negative gap: area prevalence is below the national rate.

The prevalence measures are intended for contexts with one indicator and year, and one health area for area-level results.

## Example findings

For **underweight prevalence in 2023**:

- Nuwara Eliya RDHS recorded **26.4%**, compared with the national rate of **17.1%**—a gap of **9.3 percentage points**.
- Kandy RDHS recorded **18.5%**, or **1.4 percentage points** above the national rate.
- National underweight prevalence increased from **12.2% in 2021** to **17.1% in 2023**.

These comparisons can support further investigation into local nutrition needs; they do not establish the causes of the differences.

## Data issues and limitations

- Ratnapura’s 2021 nutrition observations were reported as missing. They remain blank rather than being replaced with zero.
- Health reporting areas do not correspond exactly to administrative districts. The district mapping supplies poverty context while retaining separate health reporting areas.
- Poverty figures describe **all ages in 2019**. They are not current or child-specific poverty estimates.
- Area prevalence percentages should not be summed. National prevalence comes from the published national figures, rather than an unweighted average of areas.
- Nutrition indicators can overlap, so their percentages do not form parts of a single total.
- The data is annual. January 1 dates are modelling keys, not actual observation dates.

## How to open

1. Download `22FIS0558_ChildNutrition_Final.pbix`.
2. Open it in Power BI Desktop.
3. Select an indicator and year on Overview.
4. Right-click an area’s bar to drill through to Area Detail.

Refreshing the data may require updating the local PDF source paths in Power Query.

## Tools

Power BI Desktop · Power Query · DAX
