# Predicting Vehicle Selling Prices

Authors: Jerry John Doviavu, Vaibhav Sharma, and Ceilidh Torrance.

## Project summary

How well can vehicle year and odometer readings predict selling prices? Using a reproducible sample of 5,000 records from the Vehicle Sales Data dataset, we compared linear regression with a baseline that predicts the median training price. Linear regression reduced mean cross-validation mean absolute error (MAE) by approximately 26.3%, from 7,092.95 to 5,229.50. On the held-out test set, MAE was 5,319.82 and R² was approximately 0.40.

Vehicle year and odometer readings provide useful predictive information, but substantial errors remain. The model underestimates expensive vehicles and produces some unrealistic negative predictions, limiting its usefulness for precise individual valuations. Test performance was evaluated only for vehicles with recorded selling prices.

## Report

[Read the published analysis report](https://ubc-mds-2026-27.github.io/DSCI-522-Vehicle-Price-Predictions/analysis/vehicle-price-prediction.html).

The [Quarto source](analysis/vehicle-price-prediction.qmd) and [rendered HTML](analysis/vehicle-price-prediction.html) are included in this repository. To reproduce the report locally, follow the instructions below.

## Data

The analysis uses [Vehicle Sales Data on Kaggle](https://www.kaggle.com/datasets/syedanwarafridi/vehicle-sales-data), specifically `car_prices.csv`.

The report downloads the CSV into `data/raw/` when it is missing and reuses it when it already exists. It selects 5,000 records using random seed 522 and splits them into 4,000 training observations and 1,000 test observations before exploratory analysis or learned preprocessing. The splits are saved in `data/processed/`.

The predictors are vehicle year and odometer reading, and the target is selling price. Missing predictor values are handled with median imputation inside the modelling pipeline. Imputation values are learned only from the relevant training data, including within each cross-validation fold.

## Dependencies

The analysis was developed and tested on macOS using Python 3.12, uv 0.12.5, and Quarto 1.10.18. Git is also required to clone the repository.

The direct Python dependencies are pandas, scikit-learn, matplotlib, Jupyter, and kagglehub. Dependency requirements are declared in [pyproject.toml](pyproject.toml), and resolved versions are recorded in [uv.lock](uv.lock). Run `uv sync --locked` to recreate the Python environment.

Git, uv, and Quarto must be installed separately.

## How to run the analysis

### Prerequisites

Install [Git](https://git-scm.com/downloads), [uv](https://docs.astral.sh/uv/getting-started/installation/), and [Quarto](https://quarto.org/docs/get-started/)

The project uses Python 3.12. Python dependencies are declared in [pyproject.toml](pyproject.toml), and resolved package versions are recorded in [uv.lock](uv.lock). The Python version request is recorded in `.python-version`.

### Clone the repository

Run these commands in your terminal:

```bash
git clone https://github.com/UBC-MDS-2026-27/DSCI-522-Vehicle-Price-Predictions.git
cd DSCI-522-Vehicle-Price-Predictions
```

Run all remaining commands from this repository folder.

### Recreate the environment

```bash
uv sync --locked
```

This creates a local `.venv` and installs the dependencies recorded in the lockfile. The virtual environment is not committed to Git.

The direct Python dependencies are pandas, scikit-learn, matplotlib, Jupyter, and kagglehub.

### Verify the dependencies

```bash
uv run python -c "import pandas, sklearn, matplotlib, jupyter, kagglehub; print('All five packages imported successfully')"
quarto --version
```

Git, uv, and the Quarto executable are installed separately from the project's Python dependencies.

### Render the report

```bash
uv run quarto render analysis/vehicle-price-prediction.qmd
```

This executes the analysis and creates `analysis/vehicle-price-prediction.html` and its supporting assets. Open the HTML file in a browser to read the report.

Use `uv run` to execute commands in the project environment; manual activation is not required. Internet access is needed when the raw CSV must be downloaded.


## Project organisation

| Location | Purpose |
|---|---|
| `analysis/vehicle-price-prediction.qmd` | Analysis code and narrative |
| `analysis/vehicle-price-prediction.html` | Rendered report |
| `analysis/vehicle-price-prediction_files/` | Supporting report assets |
| `data/raw/` | Original downloaded data |
| `data/processed/` | Saved training and test splits |
| `pyproject.toml` | Python requirements and direct dependencies |
| `uv.lock` | Resolved dependency versions |
| `.python-version` | Python version request |
| `CODE_OF_CONDUCT.md` | Community participation expectations |

## Licenses

The software code is licensed under the MIT License. The analysis report is licensed under Creative Commons Attribution-NonCommercial-NoDerivatives 4.0 International (CC BY-NC-ND 4.0). See [LICENSE.md](LICENSE.md) for the project’s licensing documentation. The proposed licenses are MIT for the software code and Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0) for the report. This section will be updated to match the approved `LICENSE.md`.

The Vehicle Sales Data dataset is listed on [Kaggle](https://www.kaggle.com/datasets/syedanwarafridi/vehicle-sales-data) under the MIT License. Its licensing is separate from the licenses applied to this project's code and report.

## AI use statement

Jerry used ChatGPT to assist with Python environment setup, Git and GitHub command explanations, analysis code, debugging, interpretation of model results, and drafting documentation and review feedback. Generated suggestions were checked through local execution and teammate review where applicable. Team members performed the GitHub issue, pull request, review, and merge actions themselves.

Example prompts included:

- “Before fitting it, what are some other ways of replacing the missing values?”
- “Is this not a case of data leakage?”
- “There's going to be an oral check-in to explain the process and the code, so I understand what we are doing.”

Other team members should add their own AI-use disclosures, if applicable, before submission.

## References

syedanwarafridi. (n.d.). *Vehicle Sales Data* [Dataset]. Kaggle. https://www.kaggle.com/datasets/syedanwarafridi/vehicle-sales-data. Accessed November 21, 2026.
