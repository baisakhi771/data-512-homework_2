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

This project uses two APIs to gather data for Wikipedia article quality predictions:

1. Wikipedia API - To retrieve the latest revision ID of the articles.

2. ORES API - To predict the quality of Wikipedia articles.

### Wikipedia API ([Article Page Info MediaWiki API](https://www.mediawiki.org/wiki/API:Info))

The Wikipedia API is used to fetch metadata for each Wikipedia article, including the most recent revision ID. The revision ID is required as an input to make requests to the ORES API for article quality predictions.
For each article in politicians_by_country.AUG.2024.csv, we send a request to the Wikipedia API to get the latest revision details, specifically the revid, which is used in subsequent API calls to ORES.

Here’s how the API flow works:

Get Article Metadata: For each politician's article, we use the Wikipedia API to request the most current revid (revision ID).
Fetch Quality Predictions: Using the title and revid from the previous step, we request a predicted article quality score from the ORES API.
Sample code for making Wikipedia API requests is provided in a [notebook](https://drive.google.com/file/d/1iGH_pOMlspeCwDzKCPRlQdq73iS16R6k/view?usp=drive_link) developed by developed by Dr. David W. McDonald for use in DATA 512, a course in the UW MS Data Science degree program. This code is provided under the [Creative Commons](https://creativecommons.org) [CC-BY license](https://creativecommons.org/licenses/by/4.0/). Revision 1.2 - September 16, 2024

### ORES API ([ORES API documentation](https://ores.wikimedia.org))

The ORES API (Objective Revision Evaluation Service) is a machine learning system designed to predict the quality of Wikipedia articles. ORES provides article quality estimates based on peer-reviewed quality classes, ranked as follows:

FA - Featured article

GA - Good article (A-Class)

B - B-Class article

C - C-Class article

Start - Start-class article

Stub - Stub-class article

To get article quality predictions from ORES, we need the article’s title and the latest revid, retrieved via the Wikipedia API.

The [sample code](https://drive.google.com/file/d/1GN1ULxKombHRzVsNKzj7tBhnBrSWUWXc/view?usp=drive_link) for interacting with the ORES API developed by Dr. David W. McDonald for use in DATA 512, a course in the UW MS Data Science degree program. This code is provided under the [Creative Commons](https://creativecommons.org) [CC-BY license](https://creativecommons.org/licenses/by/4.0/). Revision 1.0 - August 15, 2023

Accessing the APIs
To access both the Wikipedia API and the ORES API, we need to request an API key. For security purposes, ensure the key is stored in an environment variable or a local key manager, as demonstrated in the example code. Do not expose API keys directly in your codebase.

Error Handling and Logging
It’s possible that some API requests may fail. In those cases, log the articles for which no quality score could be retrieved. The error log can be saved in a file or displayed in the notebook. Additionally, compute and display the error rate, which is the number of failed requests divided by the total number of articles. If our error rate exceeds 1%, we should review and improve your code.


## Data Schema and Output Files

In this project, theree intermediate data files and one final output file were saved to the repository. Each file represents a different stage of the data collection and processing workflow.

### Intermediate Files

First Intermediate File (**politician_article_revid.json**):

This JSON file was generated after the initial API request to the Wikipedia API for retrieving page information mainly the Wikipedia article's last revision id. Specifically, the output was saved as a JSON file containing a dictionary where each article_title (string) was paired with its corresponding revision_id (integer). This file acts as a record of the current revisions of the Wikipedia articles for each politician.

Second Intermediate File (**article_scores.csv**):

Following the second API request to the ORES API, a CSV file was generated. This file contains two columns:

article_title (string)

article_quality (string)

The article quality represents the predicted assessment class for each article, as returned by the ORES machine learning model.

Third Intermediate File (**wp_countries-no_match.txt**):

During the merging of the Wikipedia and population datasets, some countries may not have matching entries in both datasets. This file, wp_countries-no_match.txt, is generated to list all countries that were present in the Wikipedia dataset but did not have a corresponding entry in the population dataset (or vice-versa). The countries are listed line by line, each on a separate line, as a text file.


### Final Output File

The final output file (**wp_politicians_by_country.csv**) was created by merging the two intermediate datasets with the population_by_country_AUG.2024.csv dataset. This merged dataframe was saved as a CSV file with the following six columns:

region (String) - Region of the world where the country is located

country (string) – The country associated with the politician

population (integer) – The population of the politician's country

article_title (string) – The name of the politician

revision_id (integer) – The most recent revision ID of the Wikipedia article

article_quality (string) – The predicted quality score of the article

This structured format allows for seamless analysis by combining relevant metadata (e.g., article quality, country population) with the politicians' Wikipedia articles.

## Analysis and Result

### Analysis Overview
The analysis for this project focuses on calculating two key metrics:

**Total Articles Per Capita**: The number of Wikipedia articles per person for each country and region.

**High-Quality Articles Per Capita**: The number of "high-quality" articles per person for each country and region, where high-quality is defined as articles that ORES predicted to be either "FA" (Featured Article) or "GA" (Good Article).

The analysis assigns each country to a single region, using the closest or lowest-level region from the population_by_country_AUG.2024.csv file. Regional population data is in millions, so the resulting proportions are relatively small and are represented accordingly.

### Results
The final results from the analysis are presented in the following six tables, which are embedded within the analysis notebook:

**Top 10 Countries by Total Articles Per Capita**
A table listing the 10 countries with the highest total articles per capita, in descending order.

**Bottom 10 Countries by Total Articles Per Capita**
A table listing the 10 countries with the lowest total articles per capita, in ascending order.

**Top 10 Countries by High-Quality Articles Per Capita**
A table listing the 10 countries with the highest high-quality articles per capita, in descending order.

**Bottom 10 Countries by High-Quality Articles Per Capita**
A table listing the 10 countries with the lowest high-quality articles per capita, in ascending order.

**Geographic Regions by Total Articles Per Capita**
A rank-ordered list of geographic regions by total articles per capita.

**Geographic Regions by High-Quality Articles Per Capita**
A rank-ordered list of geographic regions by high-quality articles per capita.


## Research Implications

This study provided valuable insights into the importance of data preprocessing and the intricacies of working with APIs. One key lesson was the necessity of cleaning and validating data before analysis, as merging Wikipedia articles with population data revealed inconsistencies, such as mismatched country names and entries without matches. This experience highlighted the often imperfect nature of real-world data, requiring careful attention to detail.

I was surprised by the uneven distribution of Wikipedia articles across countries. Smaller, less-populated nations often had disproportionately high coverage, while larger countries showed relatively low representation. This disparity may result from Wikipedia’s community-driven model, where contributors might focus more on certain regions based on factors like internet access or cultural priorities. For instance, I expected populous countries like India to have better article coverage, but their large populations diluted the articles-per-capita ratio.

Additionally, the variation in article quality was striking. Contrary to my expectations, countries with smaller populations frequently had a higher proportion of "Featured" or "Good" articles. This suggests potential socio-economic biases, where wealthier regions may have more active contributors producing higher-quality content, leaving others underrepresented in both quantity and quality.

These findings indicate that even platforms like Wikipedia can exhibit inherent biases that affect data reliability. Such discrepancies are crucial for researchers to consider when utilizing Wikipedia data for analysis, as they may not provide an accurate representation of global knowledge.

In this reflection, I’ll focus on three key questions from the assignment to further analyze these observations:

### 1.	What biases did you expect to find in the data (before you started working with it), and why?

Before starting the analysis, I anticipated that there would be biases related to the geographical distribution of contributors to Wikipedia. I expected that wealthier and more developed regions would have more comprehensive and higher-quality articles due to better access to the internet and more resources for contributing to the platform. I also thought there might be cultural biases, where certain topics are more thoroughly covered in some regions compared to others, reflecting the interests and expertise of the contributors.

### 2.	What (potential) sources of bias did you discover in the course of your data processing and analysis?

During my analysis, I uncovered several sources of bias. For one, the population data did not always match Wikipedia entries due to inconsistent naming conventions for countries, which hindered accurate merging of datasets. Additionally, I noticed that smaller nations had a higher articles-per-capita ratio, indicating that less populous countries might be better represented than their larger counterparts. The community-driven nature of Wikipedia can lead to underrepresentation of significant topics in larger, more diverse countries, where contributors may focus on local interests rather than global issues.

### 3.	What might your results suggest about (English) Wikipedia as a data source?

The results suggest that while Wikipedia can be a valuable resource, it is inherently biased due to its reliance on volunteer contributors. This model can result in uneven coverage of topics and regions, leading to skewed representations of knowledge. The analysis indicated that certain countries and regions may be disproportionately represented, impacting the reliability of Wikipedia as a comprehensive data source for global information. Researchers should approach Wikipedia with a critical mindset, recognizing these limitations when drawing conclusions from its data.

### 4.	What might your results suggest about the internet and global society in general?

My findings highlight broader implications regarding the digital divide and knowledge accessibility in global society. They suggest that factors such as internet access, education, and socio-economic status significantly influence the availability and quality of information online. This disparity reinforces existing knowledge gaps, where some regions have abundant resources and contributions while others remain underrepresented. Ultimately, these results call for more inclusive efforts to improve information access and representation across diverse populations in the digital landscape.


## License
This project is licensed under the MIT License. See the LICENSE file for details.

