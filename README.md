# Consumer Finance Segmentation Using Machine Learning

## Project Overview

This project explores consumer financial characteristics using data from the **2019 Survey of Consumer Finances (SCF)**. It applies data wrangling, exploratory analysis, feature scaling, dimensionality reduction, and K-Means clustering to investigate patterns in household financial circumstances.

The project also incorporates interactive visualisation using **Plotly and Dash**, allowing users to explore the resulting analysis through a web-based dashboard.

The goal is to demonstrate how unsupervised machine learning can be used to identify groups of consumers with similar financial characteristics and support further investigation of household financial behaviour.

## Objectives

The main objectives of this project are to:

* Prepare and transform consumer financial data for analysis.
* Explore household financial indicators, including debt, housing assets, and net worth.
* Apply feature selection and standardisation to prepare data for machine learning.
* Use Principal Component Analysis (PCA) to reduce the dimensionality of selected features.
* Apply K-Means clustering to identify groups of consumers with similar financial characteristics.
* Visualise analytical results using Plotly.
* Develop an interactive dashboard using Dash to explore clustering results.

## Dataset

The analysis uses the **2019 Survey of Consumer Finances (SCF)** dataset, commonly distributed as `SCFP2019.csv.gz`.

The Survey of Consumer Finances provides information on U.S. households' assets, liabilities, income, and other financial characteristics.

## Technologies Used

The project uses the following Python tools and libraries:

* **Python** — data analysis and machine learning.
* **Pandas** — data manipulation and tabular analysis.
* **NumPy** — numerical operations.
* **Matplotlib** — data visualisation.
* **Seaborn** — statistical visualisation, where used.
* **Scikit-learn** — feature scaling, dimensionality reduction, and clustering.
* **Plotly** — interactive charts.
* **Dash** — interactive web dashboard.
* **Jupyter Notebook** — development and documentation of the analytical workflow.

## Methodology

### 1. Data loading and preparation

The dataset is loaded into Python for processing. Data preparation involves selecting relevant variables and transforming the data into a format suitable for analysis.

The notebook includes a data-wrangling function to support this process.

### 2. Exploratory data analysis

Selected financial variables are examined to understand the structure and distribution of household financial characteristics. The analysis includes variables relating to debt, housing assets, and net worth.

### 3. Feature selection and scaling

Relevant numerical features are selected for clustering. Standardisation using `StandardScaler` is used to place features on comparable scales before applying distance-based machine learning methods.

### 4. Dimensionality reduction

Principal Component Analysis (PCA) is used to represent selected financial features in a lower-dimensional space. This supports the exploration and visualisation of patterns in the data.

### 5. Consumer segmentation with K-Means

K-Means clustering is used to group observations according to similarities in the selected feature space.

The notebook includes analysis involving different cluster configurations. The resulting groups can be investigated to understand how household financial characteristics vary across clusters.

Cluster labels are statistical groupings, not predefined consumer categories. Their interpretation requires examining the financial characteristics of each group.

### 6. Interactive visualisation

Plotly and Dash are used to create interactive visualisations of the analysis. The dashboard supports exploration of clustering results and relevant plot settings.

## Expected Outputs

Depending on the selected options and the successful execution of the notebook, the analysis can produce:

* Prepared consumer financial data.
* Exploratory visualisations of selected financial variables.
* Scaled features for machine learning.
* PCA representations of the selected features.
* K-Means cluster assignments.
* Visualisations for examining patterns among the resulting clusters.
* An interactive Dash dashboard for exploring selected results.

Actual results depend on the dataset, preprocessing decisions, feature selection, and clustering configuration.

## Reproducibility

For reproducible results:

1. Use the documented version of the dataset.
2. Install the required Python dependencies.
3. Follow the notebook cells in order.
4. Keep data preprocessing and feature selection consistent.
5. Document any changes to the clustering configuration.
6. Record any modifications to the original notebook or dataset.
