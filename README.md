# Consumer Finance Segmentation

A Python notebook project exploring household financial characteristics in the **Survey of Consumer Finances (SCF) 2019** dataset. The notebook combines exploratory data analysis (EDA), visual comparisons, K-Means clustering, feature scaling, cluster evaluation, and PCA-based visualisation. It also builds an interactive dashboard with Dash and Plotly.

> **Data note:** The SCF dataset is not included in this repository. Obtain the dataset from its official source and follow its usage terms. The notebook currently expects a local compressed CSV named `SCFP2019.csv.gz`; update the path as described below before running it.

## Contents

- [Project objectives](#project-objectives)
- [Analytical workflow](#analytical-workflow)
- [Repository structure](#repository-structure)
- [Dataset](#dataset)
- [Requirements](#requirements)
- [Setup](#setup)
- [Running the notebook](#running-the-notebook)
- [Running the Dash dashboard](#running-the-dash-dashboard)
- [Methods](#methods)
- [Interpretation and limitations](#interpretation-and-limitations)
- [Reproducibility](#reproducibility)
- [Troubleshooting](#troubleshooting)
- [Possible extensions](#possible-extensions)
- [License](#license)

## Project objectives

This project is designed to:

1. Explore demographic and financial characteristics in SCF data.
2. Compare households flagged by `TURNFEAR` with the full sample.
3. Examine relationships among variables such as debt, home value, assets, income, education, and net worth.
4. Apply K-Means clustering to selected household financial features.
5. Compare cluster solutions using inertia and silhouette scores.
6. Use principal component analysis (PCA) to display high-dimensional observations in two dimensions.
7. Provide interactive controls for feature-variance selection and cluster count in a Dash dashboard.

The notebook uses `TURNFEAR == 1` as its operational definition of the “credit-fearful” group. Confirm the variable's meaning and coding against the codebook that accompanies the exact SCF file you use.

## Analytical workflow

### 1. Exploratory data analysis

The notebook loads the SCF data, checks its dimensions, and filters a subset using `TURNFEAR`. It then visualises age groups, race categories, income categories, education levels, and selected financial variables. Some plots compare the filtered subset with the full dataset.

### 2. Relationships among financial variables

Correlation matrices and scatter plots are used to explore relationships between variables including `ASSET`, `HOUSES`, `INCOME`, `DEBT`, and `EDUC`. Correlation is descriptive and does not establish causation.

### 3. K-Means clustering with two features

The notebook initially clusters households using `DEBT` and `HOUSES`. It examines cluster assignments and centroids and evaluates candidate values of *k* using inertia and silhouette score.

### 4. Multi-feature clustering

The notebook then filters to records meeting both `TURNFEAR == 1` and `NETWORTH < 2,000,000`. It selects features with high variance, compares ordinary and trimmed variance, scales features with `StandardScaler`, and fits K-Means models. It uses cluster profiles and PCA plots to inspect the resulting groups.

### 5. Interactive dashboard

The Dash app offers controls for selecting trimmed versus untrimmed variance and the number of clusters. It displays a feature-variance bar chart, clustering metrics, and a PCA scatter plot.

## Repository structure

```text
consumer-finance-segmentation/
├── README.md
├── requirements.txt
├── .gitignore
├── LICENSE
├── data/
│   └── README.md
└── notebooks/
    └── consumer_finance_segmentation.ipynb
```

## Dataset

The notebook is based on the **2019 Survey of Consumer Finances (SCF)**. The source data file is expected to be named:

```text
SCFP2019.csv.gz
```

The notebook does not bundle the dataset. Download it from the Federal Reserve's official SCF resources and consult the accompanying codebook and documentation. Dataset distributions and variable coding may differ if you use a different release or a transformed version.

### Configure the data path

The uploaded notebook contains a Windows-specific path similar to:

```python
pd.read_csv("C:/Users/HomePC/Downloads/SCFP2019.csv.gz")
```

This path will not work on another computer. Put the file in the project's `data/` directory and update each data-loading call to:

```python
from pathlib import Path
import pandas as pd

DATA_PATH = Path("..") / "data" / "SCFP2019.csv.gz"
df = pd.read_csv(DATA_PATH)
```

This relative path is appropriate when the notebook is launched from the `notebooks/` directory. If your notebook's working directory differs, adjust the path accordingly. You can check it with:

```python
from pathlib import Path
print("Working directory:", Path.cwd())
print("Dataset exists:", DATA_PATH.exists())
```

The notebook defines `wrangle()` more than once, so update all data-loading locations, not only the first one.

## Requirements

The project uses:

- Python
- Jupyter Notebook
- pandas
- NumPy
- Matplotlib
- Seaborn
- Plotly
- Dash
- SciPy
- scikit-learn

Install the listed packages with:

```bash
python -m pip install -r requirements.txt
```

## Setup

### 1. Clone the repository

```bash
git clone https://github.com/YOUR-USERNAME/consumer-finance-segmentation.git
cd consumer-finance-segmentation
```

Replace `YOUR-USERNAME` with your GitHub username after creating the repository.

### 2. Create and activate a virtual environment

**Windows (PowerShell):**

```powershell
py -m venv .venv
.\.venv\Scripts\Activate.ps1
```

**macOS/Linux:**

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Install dependencies

```bash
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

### 4. Add the dataset

Create or use the `data/` folder, place `SCFP2019.csv.gz` there, and follow the path instructions above. Do not commit the dataset unless its licence and distribution terms explicitly permit it.

### 5. Launch Jupyter

```bash
jupyter notebook
```

Open `notebooks/consumer_finance_segmentation.ipynb` and run the cells in order.

## Running the Dash dashboard

The final notebook cell starts the app on port `8050`. For local development, use:

```python
app.run(host="127.0.0.1", port=8050, debug=False)
```

Then visit <http://127.0.0.1:8050> in a browser on the same computer.

If port 8050 is already in use, stop the other process or change the port number consistently. If you use a remote or hosted notebook, local browser access may require that platform's port-forwarding or proxy instructions.

**Notebook code review note:** check the callback argument order before relying on the dashboard. Dash supplies callback arguments in the same order as the declared `Input` objects. In the notebook, the metrics callback declares `trim-button` before `k-slider`, while its function parameters are named `k` then `trimmed`; the scatter callback has the opposite ordering issue. Align each function's parameter order with its `Input` order. Also, the radio control uses boolean values but its initial value is `5`; set its initial value to either `True` or `False` so it matches its options. Run and test the dashboard after making these changes.

## Methods

### K-Means

K-Means partitions observations into *k* groups by minimising within-cluster squared distances. It is sensitive to feature scale and outliers. The multi-feature workflow uses `StandardScaler` before clustering; the earlier two-feature example should be interpreted with awareness that `DEBT` and `HOUSES` are measured in potentially large dollar values.

### Selecting the number of clusters

- **Inertia** measures the within-cluster sum of squared distances and generally decreases as the number of clusters increases.
- **Silhouette score** measures how similar observations are to their own cluster compared with other clusters. Larger values generally indicate better separation, but the score should be considered alongside interpretability and stability.

Neither metric alone proves that a particular cluster count is substantively correct.

### Trimmed variance

The notebook uses SciPy's `trimmed_var` to reduce the influence of the lower and upper tails when ranking features by variance. The selected features can change depending on whether ordinary or trimmed variance is used.

### PCA

PCA projects data into a lower-dimensional space. The scatter plot uses the first two principal components for visual exploration; the axes are not original financial variables, and a two-dimensional plot may not preserve all structure in the original feature space.

## Interpretation and limitations

- Cluster labels are arbitrary identifiers, not rankings or naturally occurring categories.
- Cluster profiles describe patterns in the selected data and features; they should not be interpreted as causal explanations.
- High variance does not automatically mean a feature is useful for segmentation.
- Financial data can be highly skewed and contain extreme values. Review distributions and scaling choices before interpreting results.
- The notebook's filtering rules affect which households are included. In particular, the multi-feature analysis uses `TURNFEAR == 1` and `NETWORTH < 2e6`.
- The exact interpretation of coded variables should be confirmed against the official SCF codebook.
- Results depend on data cleaning, feature selection, scaling, random initialisation, and the chosen number of clusters.
- The notebook is an exploratory analysis, not financial advice and not a validated credit-risk model.

## Reproducibility

The notebook sets `random_state=42` for K-Means in multiple cells. To improve reproducibility further:

1. Record the dataset release and source.
2. Record Python and package versions.
3. Keep data paths relative rather than tied to one computer.
4. Document all filtering and missing-value decisions.
5. Re-run the notebook from a fresh kernel before publishing results.
6. Do not publish claims about specific cluster sizes or outcomes unless they are generated from the exact data and environment described.

## Troubleshooting

### `FileNotFoundError`

The dataset path is incorrect or the file is missing. Check `Path.cwd()`, place the file in `data/`, and update every `read_csv` call.

### `WinError 10049` or the dashboard does not open

For a local notebook, use `app.run(host="127.0.0.1", port=8050, debug=False)` and open <http://127.0.0.1:8050>. Confirm that the cell is still running and that port 8050 is not occupied.

### Dash callback argument mismatch

Callback function parameters must follow the same order as the `Input(...)` declarations. Ensure the radio item's initial `value` matches one of its boolean options (`True` or `False`).

### Missing package

Activate the project environment and run `python -m pip install -r requirements.txt`.

### K-Means or silhouette errors

Silhouette score requires between 2 and *n_samples - 1* distinct clusters. Check for enough observations and distinct cluster labels. Also inspect missing or non-numeric values before fitting the model.

## Possible extensions

- Add a data-validation and missing-value report.
- Compare alternative feature-selection methods.
- Evaluate cluster stability across multiple random seeds.
- Add cluster-size charts and clearer cluster profile tables.
- Compare K-Means with hierarchical clustering or DBSCAN.
- Export cluster assignments and summary profiles to CSV.
- Add automated checks and a clean, parameterised analysis script.
- Add a dashboard screenshot once the app has been tested locally.

## License

A `LICENSE` file is included as a placeholder for a repository owner to select and confirm an appropriate licence. Choose a licence that fits your intended reuse and ensure it does not conflict with the dataset's terms.
