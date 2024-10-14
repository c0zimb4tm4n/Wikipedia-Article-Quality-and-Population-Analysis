# DATA512 - Homework 2: Wikipedia Article Quality and Population Analysis

## Overview

This project explores the concept of bias in data using Wikipedia articles about political figures from different countries. The assignment involves combining a dataset of Wikipedia articles with a dataset of country populations and using the ORES machine learning service to estimate the quality of each article. The analysis aims to evaluate how the coverage of politicians and the quality of articles varies across countries and geographic regions.

The project culminates in a series of tables showcasing:
- The countries with the highest and lowest coverage of politicians per capita.
- The countries with the highest and lowest proportion of high-quality articles per capita.
- Rankings of geographic regions by articles-per-person and proportion of high-quality articles.

## Folder Structure
```
data-512-homework_2/
│
├── code/                            # Folder containing all code files
   ├── data_analysis.ipynb              # Notebook for analyzing the merged data
   ├── data_aquisition.ipynb            # Notebook for acquiring and processing article and population data
   ├── data_merge.ipynb                 # Notebook for merging datasets and cleaning data
│
├── data/                            # Folder containing all data files
   ├── articles_scores.csv          # Article titles, URLs, countries, revision IDs, and ORES quality scores
   ├── country_population_with_region.csv  # Countries with their population and corresponding regions
   ├── duplicate_politicians.csv    # Duplicate entries for politicians serving multiple countries
   ├── ores_error_log.txt           # Log of articles with missing or invalid ORES scores
   ├── politicians_by_country_AUG.2024.csv # Raw data of politicians and their countries from Wikipedia
   ├── population_by_country_AUG.2024.csv  # Population data by country, including hierarchical regions
   ├── wp_countries-no_match.txt    # List of countries for which no match was found between the two datasets
   ├── wp_politicians_by_country.csv # Final merged dataset of politicians, regions, population, and article quality
│
├── LICENSE                          # License file
└── README.md                        # This file
```

## Data Sources

1. **Wikipedia Politician Articles**: Crawled from Wikipedia's Category: Politicians by nationality.
   - *File*: `politicians_by_country_AUG.2024.csv`
   - *Schema*:
     - `name`: Name of the politician
     - `url`: URL of the Wikipedia article
     - `country`: Country where the politician serves
     - `revision_id`: Wikipedia article's latest revision ID
     - `article_quality`: ORES quality score for the article (FA, GA, etc.)

2. **Population Data**: Downloaded from the World Population Data Sheet by the Population Reference Bureau.
   - *File*: `population_by_country_AUG.2024.csv`
   - *Schema*:
     - `Geography`: Name of the country or region
     - `Population`: Population of the country in millions

## Process

### Step 1: Data Acquisition
- **Politician Article Data**: Extracted using the Wikipedia API. The `articles_scores.csv` file stores politician names, URLs, revision IDs, and ORES quality scores.
- **Population Data**: Processed to assign countries to specific regions and remove zero population entries. Saved in `country_population_with_region.csv`.

### Step 2: Article Quality Prediction
- For each article in the politician dataset, we retrieve the latest `revision_id` using the Wikipedia API, followed by querying the ORES API to estimate article quality.
- Any failed API requests are logged in `ores_error_log.txt`.
- ORES quality labels include: FA (Featured Article), GA (Good Article), B, C, Start, and Stub. High-quality articles are considered to be FA or GA.

### Step 3: Data Merging
- The datasets are merged on the `country` field. Countries that do not match between the datasets are logged in `wp_countries-no_match.txt`.
- The final merged dataset `wp_politicians_by_country.csv` contains the following columns:
   - `country`: Country of the article
   - `region`: Geographic region of the country
   - `population`: Population of the country
   - `article_title`: Title of the Wikipedia article
   - `revision_id`: Wikipedia article's latest revision ID
   - `article_quality`: ORES quality score (FA, GA, etc.)

### Step 4: Analysis
The analysis focuses on calculating:
1. **Total Articles Per Capita**: The number of Wikipedia articles per 10 million people.
2. **High-Quality Articles Per Capita**: The number of high-quality (FA or GA) articles per 10 million people.

The results are produced in the form of tables, including:
1. Top 10 countries by total articles per capita.
2. Bottom 10 countries by total articles per capita.
3. Top 10 countries by high-quality articles per capita.
4. Bottom 10 countries by high-quality articles per capita.
5. Geographic regions ranked by total articles per capita.
6. Geographic regions ranked by high-quality articles per capita.

### Step 5: Research Implications

#### Biases Expected Before Data Processing
Before working with the data, I expected to find biases related to geographical coverage. Since Wikipedia is dominated by English-speaking contributors, I assumed that countries with larger English-speaking populations, such as the U.S., U.K., and Australia, would have greater article coverage and more high-quality articles compared to non-English-speaking countries.

#### Biases Discovered During Processing
Several potential sources of bias were identified:
- **Geographical Discrepancies**: Certain regions like *North America* were missing from the population dataset, which affected the completeness of the analysis.
- **Country Name Inconsistencies**: Variations in how countries are named in the datasets (e.g., "China" vs. "China (Hong Kong)") led to mismatches during data merging. Additionally, countries like "North Korea" and "South Korea" caused issues due to Wikipedia categorization.
- **Zero-Quality Articles**: Many smaller countries had no high-quality articles, which made it difficult to construct a "bottom 10" list for quality analysis.
- **Small Population Bias**: Countries with smaller populations often had fewer articles, leading to an inherent bias in the articles-per-capita metric. Additionally, some populations were rounded to zero, further distorting the data.

#### Insights on Wikipedia as a Data Source
The results suggest that Wikipedia's coverage is biased towards larger, wealthier, and predominantly English-speaking countries. While Wikipedia is an open-source platform accessible to everyone, the disparity in article quality and quantity indicates that countries with more internet users and higher educational levels contribute more significantly to its content. This reflects broader inequalities in internet access and digital literacy around the world, which results in skewed representation in global datasets like Wikipedia.

---

## Results

- **Top 10 Countries by Total Articles per Capita**: Displays the highest coverage of politicians per capita.
- **Bottom 10 Countries by Total Articles per Capita**: Shows countries with the least coverage per capita.
- **Top 10 Countries by High-Quality Articles per Capita**: Shows the countries with the highest proportion of FA and GA articles per capita.
- **Bottom 10 Countries by High-Quality Articles per Capita**: Countries with the lowest proportion of high-quality articles.
- **Geographic Regions by Total Coverage**: Rankings of regions based on total articles per capita.
- **Geographic Regions by High-Quality Coverage**: Rankings of regions by high-quality articles per capita.

## License

This project is licensed under the [MIT License](LICENSE).

## References

- Wikipedia API: https://www.mediawiki.org/wiki/API:Main_page
- ORES API Documentation: https://www.mediawiki.org/wiki/ORES
- Population Data Source: United Nations Population Database (August 2024)
