# Customer Segmentation Using K-Means Clustering

A machine learning and analytics project that groups customers based on
their income and purchasing behavior using **K-Means clustering**. The
project applies feature standardization, evaluates candidate cluster
counts using the Elbow Method and Silhouette Score, profiles the
resulting clusters, and prepares customer-level output for business
analysis and Power BI.

> **Dataset note:** This project uses a synthetic dataset created for
> learning and portfolio demonstration. The results are illustrative and
> should not be treated as findings about real customers.

## Project Overview

Customer segmentation helps businesses understand different customer
groups and plan more relevant marketing, retention, and engagement
strategies. This project uses four behavioral and financial features to
group 500 synthetic customer records into three clusters.

### Objectives

-   Load, inspect, and explore customer data.
-   Check data types, missing values, and descriptive statistics.
-   Standardize selected numerical features before clustering.
-   Evaluate values of `K` from 2 through 8.
-   Compare clustering results using inertia and silhouette scores.
-   Fit a K-Means model and assign each customer to a cluster.
-   Profile clusters and assign business-oriented segment labels.
-   Visualize customer groups and export a segmented customer dataset.
-   Make the resulting data available for further analysis in Power BI.

## Dataset

The project dataset contains **500 synthetic customer records** and the
following columns:

  -----------------------------------------------------------------------
  Column                              Description
  ----------------------------------- -----------------------------------
  `CustomerID`                        Unique customer identifier

  `Age`                               Customer age

  `Gender`                            Customer gender category

  `AnnualIncome_k`                    Annual income, represented in
                                      thousands

  `PurchaseFrequency`                 Number of purchases / purchase
                                      frequency measure

  `AvgOrderValue`                     Average order value

  `SpendingScore`                     Customer spending score

  `TotalSpend`                        Total spending measure

  `BusinessSegment`                   Existing synthetic business
                                      category in the source data
  -----------------------------------------------------------------------

The clustering model uses these four features:

``` python
features = [
    "AnnualIncome_k",
    "SpendingScore",
    "PurchaseFrequency",
    "AvgOrderValue"
]
```

`CustomerID`, `Age`, `Gender`, `TotalSpend`, and `BusinessSegment` are
retained in the customer data but are not used as input features by the
K-Means model in the supplied Python script.

## Technology Stack

-   **Language:** Python
-   **Data handling:** Pandas, NumPy
-   **Visualization:** Matplotlib
-   **Machine learning:** Scikit-learn
-   **Development environment:** Jupyter Notebook
-   **Dashboarding:** Microsoft Power BI

## Machine Learning Workflow

1.  **Load data** -- read the customer CSV file into a Pandas DataFrame.
2.  **Inspect data** -- review the dataset structure, data types,
    missing values, and descriptive statistics.
3.  **Select features** -- use annual income, spending score, purchase
    frequency, and average order value.
4.  **Scale features** -- apply `StandardScaler` so the selected
    features are on comparable scales.
5.  **Evaluate cluster counts** -- fit K-Means models for `K = 2` to
    `K = 8`.
6.  **Measure clustering quality** -- record inertia and calculate the
    silhouette score for each candidate `K`.
7.  **Choose K** -- use the highest silhouette score among the tested
    values; the current project result is **K = 3**.
8.  **Train final model** -- fit K-Means with `random_state=42` and
    `n_init=10`.
9.  **Profile clusters** -- calculate average feature values for each
    cluster.
10. **Label segments** -- order clusters using standardized
    cluster-level income and spending-score averages, then assign
    descriptive labels.
11. **Export and visualize** -- save the customer-level results and
    generate clustering plots.

## Model and Current Results

**Algorithm:** K-Means clustering\
**Selected number of clusters:** `3`\
**Candidate values evaluated:** `2` through `8`\
**Feature scaling:** `StandardScaler`\
**K-Means settings:** `random_state=42`, `n_init=10`

The current notebook output reports the following cluster averages:

  --------------------------------------------------------------------------
         Cluster  Annual Income Spending Score       Purchase  Average Order
                            (k)                     Frequency          Value
  -------------- -------------- -------------- -------------- --------------
               0          40.98           9.64          10.11          57.04

               1          43.66           6.98           6.66          46.96

               2          86.43           9.19           8.32          56.02
  --------------------------------------------------------------------------

The notebook maps these clusters to the following descriptive labels:

-   **Low Value**
-   **Developing**
-   **Potential High Value**

The supplied labeling logic also defines a `Premium` label for a fourth
cluster, but the current result has three clusters, so that label is not
used.

### How the segment labels are assigned

The code calculates each cluster's mean annual income and mean spending
score. It standardizes those cluster-level averages, adds the two
standardized values, sorts the clusters by that combined score, and
assigns the segment names in ascending order.

These labels are interpretive names created for this project; they are
not labels learned by K-Means itself.

## Visualizations

The notebook displays:

-   **Elbow Method:** compares inertia across candidate values of `K`.
-   **Silhouette Score:** compares the silhouette score across candidate
    values of `K`.
-   **Customer Segments:** plots annual income against spending score,
    with points grouped by assigned segment.

The Python script currently saves these plot files:

-   `elbow_method.png`
-   `customer_clusters.png`

The notebook displays the Elbow Method, Silhouette Score, and
customer-cluster plots inline. If you want separate image files for
every notebook chart, save each figure explicitly using
`plt.savefig(...)`.

## Power BI Dashboard

The exported customer-level dataset can be used as a source for a Power
BI report.

**Suggested report visuals:**

  -----------------------------------------------------------------------
  Visual                              Suggested fields
  ----------------------------------- -----------------------------------
  KPI card                            Total Customers

  KPI card                            Average Annual Income

  KPI card                            Average Spending Score

  KPI card                            Average Total Spend

  Donut chart                         Customer count by `Segment`

  Scatter chart                       `AnnualIncome_k`
                                      vs. `SpendingScore`, legend:
                                      `Segment`

  Column chart                        Average `TotalSpend` by `Segment`

  Bar chart                           Average `PurchaseFrequency` by
                                      `Segment`

  Slicers                             `Gender`, `Age`, `Segment`
  -----------------------------------------------------------------------

To build the dashboard, import the generated `customer_segments.csv`
file (or the Power BI-specific CSV if you maintain a separate export)
into Power BI. Confirm the column names and data types after import.

## Repository Structure

The repository may be organized as follows. The exact files present in
your GitHub repository should match the paths you commit.

``` text
customer-segmentation/
├── dataset/
│   ├── customer_data.csv
│   └── customer_segments.csv
├── notebooks/
│   └── Customer_Segmentation.ipynb
├── graphs/
│   ├── elbow_method.png
│   ├── silhouette_score.png
│   └── customer_clusters.png
├── powerbi/
│   └── customer_segments_powerbi.csv
├── customer_segmentation.py
├── requirements.txt
├── README.md
└── .gitignore
```

**Path note:** The supplied `customer_segmentation.py` currently reads
`customer_data.csv` from the current working directory and writes
`customer_segments.csv`, `elbow_method.png`, and `customer_clusters.png`
to that same directory. If your input file is stored in `dataset/`,
update the script's `pd.read_csv(...)` path accordingly. Likewise,
update output paths if you want generated files saved inside `dataset/`
or `graphs/`.

## Getting Started

### 1. Clone the repository

``` bash
git clone https://github.com/suhas2357/customer-segmentation.git
cd customer-segmentation
```

### 2. Create and activate a virtual environment

**Windows PowerShell:**

``` powershell
python -m venv venv
.\venv\Scripts\Activate.ps1
```

**macOS / Linux:**

``` bash
python -m venv venv
source venv/bin/activate
```

### 3. Install dependencies

``` bash
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

### 4. Run the Python script

Make sure `customer_data.csv` is in the project root (the same directory
from which you run the script), then execute:

``` bash
python customer_segmentation.py
```

The script prints the selected `K` and the average feature values for
each assigned segment. It also writes the segmented CSV and the two plot
images to the working directory.

### 5. Run the Jupyter Notebook

``` bash
jupyter notebook
```

Open:

``` text
notebooks/Customer_Segmentation.ipynb
```

Run the notebook cells in order.

### Optional: Register the virtual environment as a Jupyter kernel

If Jupyter is using a different Python installation, install and
register the project's environment:

``` powershell
python -m pip install ipykernel
python -m ipykernel install --user --name customer-segmentation --display-name "Python (Customer Segmentation)"
```

Then select **Python (Customer Segmentation)** from the notebook's
kernel selector.

## Requirements

The project requires the packages used by the supplied script and
notebook:

``` text
pandas
numpy
matplotlib
scikit-learn
jupyter
ipykernel
```

Install from the repository's `requirements.txt`:

``` bash
python -m pip install -r requirements.txt
```

## Important Implementation Notes

-   The Python script calculates `best_k` dynamically by selecting the
    tested `K` with the highest silhouette score.
-   In the supplied notebook, the final model cell currently sets
    `best_k = 3` directly. This matches the current reported result, but
    it does not dynamically select the best value from the
    silhouette-score list. To make the notebook fully automatic,
    calculate `best_k` from `silhouettes` as in the script.
-   The script saves the Elbow Method and customer-cluster plots. The
    notebook also displays the Silhouette Score plot, but the supplied
    script does not currently save a separate silhouette-score image.
-   The current script writes output files to its working directory. It
    does not currently create the `graphs/`, `dataset/`, or `powerbi/`
    directories automatically.

## Business Interpretation

The segment names are intended to support exploratory business
discussion:

-   **Potential High Value:** may be suitable for cross-selling,
    upselling, and personalized product recommendations.
-   **Developing:** may respond to relevant promotions, engagement
    campaigns, and recommendations.
-   **Low Value:** may be approached with cost-efficient engagement and
    retention campaigns.

These are hypotheses based on the synthetic data and the selected
clustering features. Validate segment behavior and campaign
effectiveness with real customer data before making business decisions.

## Resume Project Description

**Customer Segmentation using K-Means Clustering** --- Developed a
Python-based customer segmentation project using Pandas and Scikit-learn
to group 500 synthetic customer records based on income and purchasing
behavior. Applied feature standardization, evaluated cluster counts
using the Elbow Method and Silhouette Score, implemented K-Means
clustering, profiled customer groups, and prepared segmented data and
visualizations for Power BI reporting.

## Disclaimer

This project is built with synthetic data for educational and portfolio
purposes. Cluster assignments and business segment labels are analytical
interpretations, not verified customer classifications. Results should
be evaluated and validated using representative real-world data before
production or business deployment.

## Repository

[View the Customer Segmentation project on
GitHub](https://github.com/suhas2357/customer-segmentation)
