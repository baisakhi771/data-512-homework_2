# Project Title: Considering Bias in Data
## Analysis of Coverage of Politicians and the Quality of Articles about Politicians in Wikipedia

## Project Overview

The goal of this project is to explore the concept of bias in data using Wikipedia articles. This project will consider articles on political figures from different countries. For this assignment, we will combine a dataset of Wikipedia articles with a dataset of country populations, and use a machine learning service called ORES to estimate the quality of each article.

We performed an analysis of how the coverage of politicians on Wikipedia and the quality of articles about politicians varies among countries. Our analysis consists of a series of tables that show:

1. The countries with the greatest and least coverage of politicians on Wikipedia compared to their population.

2. The countries with the highest and lowest proportion of high quality articles about politicians.

3. A ranking of geographic regions by articles-per-person and proportion of high quality articles.


## Data Sources

The data for this project comes from two primary sources:

### Politicians Data:

The [Wikipedia Category: Politicians by nationality](https://en.wikipedia.org/wiki/Category:Politicians_by_nationality) was crawled to generate a comprehensive list of Wikipedia article pages about politicians from various countries. The resulting dataset is provided in the file politicians_by_country.AUG.2024.csv, located in the main folder. This file contains the names, Wikipedia URLs, and countries of politicians.
Note: Since Wikipedia categories are folksonomically created, there may be some inconsistencies or mislabeling in the dataset. The crawling process attempted to clean and resolve such issues, but it is recommended to inspect the data for any remaining duplicates or inaccuracies and document how these are handled.

### Population Data:

The population dataset, population_by_country_AUG.2024.csv, was sourced from the Population Reference Bureau's [world population data sheet](https://www.prb.org/international/indicator/population/table) and is available in the main folder. This file contains population counts for countries and cumulative totals for regions (e.g., AFRICA, OCEANIA), with regional entries distinguished by ALL CAPS in the geography field.
Note: These regional population rows are meant for reporting coverage and quality by region and should not be merged with the country-specific politician data. However, retaining these rows will help in the analysis of regional population coverage.

In summary, both datasets—politicians and population—require careful handling due to potential inconsistencies or misclassifications. Detailed documentation on how such issues are resolved is included in the analysis section of this project.

## API Usage and Documentation


## Files and Outputs

### Intermediate Files



### Final Output Files


## Result


## Research Implications




## License
This project is licensed under the MIT License. See the LICENSE file for details.

