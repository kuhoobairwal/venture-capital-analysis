

## Dataset Overview

- **Total number of observations (rows):** 4820 (after data cleaning)
- **Source:** Compiled from FactSet, a financial data and software company  
- **Time span:** Fund launches from the 2008-2025 
- **Focus:** Fund-level data, not startup-level data


## Relevant Columns for Prediction

| Column Name              | Description |
|--------------------------|-------------|
| `Fund Amount Raised`     | **Target variable** — total capital raised by the fund (in millions) |
| `AUM (Current)`          | Assets under management for the firm managing the fund |
| `Fund Age`               | Age of the fund (in years) |
| `Firm # of Funds`        | Number of total funds the firm has launched |
| `Average Fund Size (MM)` | Current average fund size at the firm |
| `Fund Type`              | Investment stage focus (e.g., Early Stage, Buyout, Mixed) — multi-label |
| `Fund Country Focus`     | Geographic target of investments (e.g., US, Europe) |
| `Fund Status`            | Whether the fund is currently raising, inevesting or closed |
| `Fund Industry Focus`    | Industry sectors the fund invests in (e.g., healthcare, tech) — multi-label |

These columns were selected based on their potential predictive value and their availability **prior to fundraising**, to avoid data leakage.


# Data Cleaning and Exploratory Data Analysis

## Data Cleaning Steps

The raw dataset initially contained data on over **6,000 venture capital funds**, which required substantial cleaning and transformation to ensure consistency and usability. Key steps included handling missing values, standardizing existing features, and creating derived variables from timestamps.

- The original dataset included a `Fund Open Date` column, which was used to create a new variable: `Fund Age`. This feature calculates the number of years a fund has been active, allowing for a more interpretable and relevant metric when evaluating a fund's track record.

- The dataset also contained a column called `Fund Invests in Multiple Rounds`, referring to cases where a VC fund reinvests in startups it has already funded. This can be a strategic move to protect equity or support growth. However, this column had over **5,200 missing values out of 6,200**, making it too incomplete to be useful. As a result, it was dropped from the final dataset.

- I conducted initial visual analysis to understand the distribution of numerical variables. This revealed that several columns, including `AUM (Current)`, were **highly skewed** and contained significant outliers. Below are plots showing the boxplot and distribution of `AUM (Current)` before cleaning:

<iframe
 src="plots/aum-raw-distribution.html"
 width="800"
 height="600"
 frameborder="0"
></iframe>

<iframe
 src="plots/aum-raw-boxplot.html"
 width="800"
 height="600"
 frameborder="0"
></iframe>

- To address these extreme outliers, I applied **IQR-based filtering** instead of the more common 3-standard-deviation rule. Because the AUM distribution was **right-skewed**, the mean and standard deviation were distorted by a few extremely large values. The IQR method uses the interquartile range to identify and filter out outliers more robustly. This resulted in a distribution that better reflects the **central tendency** of most funds.


<iframe
 src="plots/aum-clean-distribution.html"
 width="800"
 height="600"
 frameborder="0"
></iframe>

<iframe
 src="plots/aum-clean-boxplot.html"
 width="800"
 height="600"
 frameborder="0"
></iframe>

- After these cleaning steps, the total number of observations was reduced to **4,820**, resulting in a cleaner and more reliable dataset for modeling and analysis.


### Preview of Cleaned Data

| Fund Name            | Fund Status | Fund Open Date | Fund Type   | Fund Country Focus | Fund Industry Focus                         | AUM (Current) | Firm # of Funds | Average Fund Size (MM) | Fund Amount Sought | Fund Amount Raised | Fund Age |
|----------------------|-------------|----------------|-------------|--------------------|----------------------------------------------|----------------|------------------|--------------------------|---------------------|---------------------|----------|
| 01 Advisors 01 Fund  | Divesting   | 2019-05-03     | Early Stage | United States      | Other                                        | 855.00         | 3.0              | 285.00                   | 200.00              | 135.00              | 6        |
| 01 Advisors 02 LP    | Investing   | 2021-01-20     | Later Stage | United States      | Internet Software/Services                   | 855.00         | 3.0              | 285.00                   | 325.00              | 325.00              | 4        |
| 01 Advisors 03 LP    | Investing   | 2022-03-24     | Early Stage | United States      | Packaged Software; Internet Software/Services| 855.00         | 3.0              | 285.00                   | 325.00              | 395.00              | 3        |
| 01fintech LP         | Investing   | 2022-01-01     | Buyout      | United States      | Other                                        | 61.90          | 1.0              | 61.90                    | 300.00              | 61.90               | 3        |
| 01vc Fund II LP      | Investing   | 2019-01-01     | Early Stage | China              | Other                                        | 14.57          | 3.0              | 14.57                    | 14.57               | 14.57               | 6        |



## Insights from Exploratory Data Analysis

Visual analysis of the dataset provided several key insights into the structure and characteristics of venture capital funds.

### 🌍 Fund Country Focus

Most funds in the dataset are based in the **United States** (2,377), followed by **India** with 213 funds. This trend aligns with expectations, as the U.S. has long been the epicenter of venture capital activity—particularly in regions like Silicon Valley and San Francisco.

<iframe
 src="plots/fund-country-vc.html"
 width="800"
 height="600"
 frameborder="0"
></iframe>

### 🚀 Fund Type (Stage Focus)

The majority of funds in the dataset are focused on **Early Stage** investments. Some funds invest across multiple stages (e.g., Seed + Early + Late), but **Late Stage-only funds are relatively rare**. This reflects real-world dynamics: as investment rounds progress, the required check sizes grow substantially—often into the hundreds of millions or billions—making later-stage investing accessible to fewer firms.

<iframe
 src="plots/fund-type-vc.html"
 width="800"
 height="600"
 frameborder="0"
></iframe>

### 📈 AUM vs. Number of Funds (Colored by Fund Age)

This scatter plot explores the relationship between the number of funds managed by a firm and its current assets under management (AUM). Each point represents a fund and is color-coded by its age.

<iframe
 src="plots/funds_v_aum.html"
 width="800"
 height="600"
 frameborder="0"
></iframe>

There is a **slightly positive trend**: as the number of funds managed increases, AUM tends to rise as well. However, the relationship is **weak and noisy**, with wide variation in AUM even among firms managing the same number of funds.

Color gradients suggest that **fund age is not a strong determinant of AUM**. Older funds appear throughout the AUM spectrum, indicating that factors like investment strategy, fund type, or firm reputation may be more influential than time alone.


### Grouped Table: Fund Type vs. Average AUM

This grouped table highlights the **average assets under management (AUM)** for funds operating at different investment stages. Funds that span **multiple stages**—especially those combining early stage, later stage, and buyout strategies—tend to manage significantly higher capital.

This suggests that **broad-stage or hybrid investment approaches** are associated with larger fund sizes. Rather than specializing solely in early or late stage, many of the highest-AUM fund types include a blend of strategies, which may signal greater flexibility, experience, or appeal to institutional investors.

| Fund Type                                           | Average AUM (in Millions) |
|----------------------------------------------------|---------------------------:|
| Early Stage; Fund of Funds; Secondary; Buyout      | 3,167.71                   |
| Seed Stage; Early Stage; Later Stage; Secondary... | 2,102.77                   |
| Seed Stage; Early Stage; Later Stage; Fund of Funds| 1,787.27                   |
| Early Stage; Later Stage; LBO; MBO; Buyout         | 1,777.40                   |
| Early Stage; Real Estate                           | 1,764.08                   |
| ...                                                | ...                        |
| Early Stage; Mezzanine; Debt                       | 28.42                      |
| Early Stage; Secondary                             | 18.03                      |
| Seed Stage; Early Stage; Fund of Funds             | 11.46                      |
| Mezzanine; LBO; Real Estate                        | 11.43                      |
| Seed Stage; Early Stage; Infrastructure/Proj Fin   | 4.81                       |

This breakdown supports the broader finding that **larger funds often diversify across multiple stages**, likely due to the increased capital demands and longer investment horizons involved in managing a more flexible portfolio.


### Imputation

For this project, I made selective decisions about how to handle missing data, balancing data quality with time constraints and model interpretability.

A notable variable, `Fund Industry Focus`, was **excluded from modeling** due to several challenges:
- It had **multi-label values** (e.g., "Healthcare; Fintech; Consumer"), requiring multi-hot encoding
- It showed **extremely high cardinality** with hundreds of unique industry combinations
- Fewer than **1,000 out of 4,800 rows** had non-missing values, making imputation unreliable
- Creating a catch-all `'Other'` category resulted in too many rows falling into this group
- Feature engineering for this column would have required extensive parsing, cleaning, and transformation that wasn't feasible within the project's time constraints

As a result, I opted to **drop `Fund Industry Focus` entirely** from the modeling dataset, instead of doing imputation. This decision was supported by the fact that most of the dataset (~80%) was missing industry data anyway, meaning the exclusion would not significantly affect model performance.
