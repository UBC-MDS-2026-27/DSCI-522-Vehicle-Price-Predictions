# Predicting Vehicle Selling Prices

**Authors:** Jerry John Doviavu, Vaibhav Sharma, and Ceilidh Torrance

This is a data analysis project for DSCI 522 (Reproducible and Trustworthy Data Science Workflows), a course in the Master of Data Science program at the University of British Columbia.

## About

We ask how well a vehicle's model year and odometer reading can predict the price it sells for at auction. Using a reproducible sample of 5,000 records from the Vehicle Sales Data dataset, we compared a linear regression model with a baseline that always predicts the median training price. Linear regression reduced the mean cross-validation mean absolute error (MAE) by approximately 26.3%, from 7,092.95 USD to 5,229.50 USD. On the held-out test set, the MAE was 5,319.82 USD and R² was approximately 0.40.

Vehicle year and odometer readings provide useful predictive information, but substantial errors remain. The model underestimates expensive vehicles and produces some unrealistic negative predictions, which limits its usefulness for precise individual valuations. Test performance was evaluated only for vehicles with a recorded selling price.

The data come from [Vehicle Sales Data on Kaggle](https://www.kaggle.com/datasets/syedanwarafridi/vehicle-sales-data) (Anwar, n.d.), specifically the file `car_prices.csv`, which lists wholesale used vehicle sales from auctions. The analysis downloads the file into `data/raw/` only when it is missing. It then selects 5,000 records with the random seed 522 and splits them into 4,000 training and 1,000 test observations before any exploratory analysis or learned preprocessing. The predictors are the vehicle year and the odometer reading, and the target is the selling price. Missing predictor values are replaced by the training median inside the modelling pipeline.

## Report

The final report can be read [here](https://ubc-mds-2026-27.github.io/DSCI-522-Vehicle-Price-Predictions/). The Quarto source is [`analysis/vehicle-price-prediction.qmd`](analysis/vehicle-price-prediction.qmd).

## Usage

We run the analysis in a Docker container, so everyone uses the same software and package versions. The image is published on Docker Hub at [ceilidhmtorrance/dsci-522-vehicle-price-predictions](https://hub.docker.com/r/ceilidhmtorrance/dsci-522-vehicle-price-predictions).

### Setup

The first time you run the project, install and launch [Docker](https://www.docker.com/get-started/). Then clone this repository and move into it:

``` bash
git clone https://github.com/UBC-MDS-2026-27/DSCI-522-Vehicle-Price-Predictions.git
cd DSCI-522-Vehicle-Price-Predictions
```

Run all the commands below from the root of this repository.

### 1. Make an ssh key

If you do not have an ssh key yet, make one and press Enter at each question:

``` bash
ssh-keygen -t ed25519
```

The container uses this key to let you log in. If you skip this step, Docker creates a *folder* named `~/.ssh/id_ed25519.pub` instead, and connecting to the container over ssh fails until you delete that folder and make the key.

### 2. Download the image

``` bash
docker compose pull
```

### 3. Run the analysis and render the report

``` bash
docker compose run --rm analysis-env uv run quarto render analysis/vehicle-price-prediction.qmd
```

Rendering runs the whole analysis from the start. The raw data file `data/raw/car_prices.csv` is included in this repository, so the report reuses it and does not need a Kaggle login. If you delete that file, the report downloads it again from Kaggle, which needs an internet connection. The report overwrites the training and test files in `data/processed/` and writes the rendered report to `analysis/vehicle-price-prediction.html`, replacing the version in this repository. Because of the `--rm` option, the container removes itself when the render finishes, so there is nothing to stop afterwards.

## Contributing

If you want to change the analysis, add a package, or update the published report, please see [`CONTRIBUTING.md`](CONTRIBUTING.md).

## Dependencies

- [Docker](https://www.docker.com/) is the only program you need to install to run the analysis.
- The image is built from the [`Dockerfile`](Dockerfile) in this repository. It installs Python 3.12, Quarto 1.11.9, and uv 0.13.7, and then the Python packages listed in [`pyproject.toml`](pyproject.toml): ipykernel, jupyter, kagglehub, matplotlib, nbclient, pandas, pyyaml, and scikit-learn. The exact versions of all packages are locked in [`uv.lock`](uv.lock).

## Project organisation

| Location | Purpose |
|---|---|
| `analysis/vehicle-price-prediction.qmd` | Analysis code and narrative |
| `analysis/vehicle-price-prediction.html` | Rendered report |
| `docs/index.html` | Copy of the rendered report, published with GitHub Pages |
| `data/raw/` | Original downloaded data |
| `data/processed/` | Saved training and test splits |
| `Dockerfile` | Recipe for the container image |
| `docker-compose.yml` | Settings to run the published image |
| `.github/workflows/publish-dockerhub.yml` | GitHub Actions workflow that builds and publishes the image |
| `pyproject.toml`, `uv.lock`, `.python-version` | Python version and pinned packages |
| `LICENSE.md`, `CODE_OF_CONDUCT.md`, `CONTRIBUTING.md` | License, behaviour expectations, and contribution guide |

## License

The software code in this repository is licensed under the MIT License. The analysis report is licensed under Creative Commons Attribution-NonCommercial-NoDerivatives 4.0 International (CC BY-NC-ND 4.0). See [`LICENSE.md`](LICENSE.md) for more information. The Vehicle Sales Data dataset is listed on [Kaggle](https://www.kaggle.com/datasets/syedanwarafridi/vehicle-sales-data) under the MIT License, which is separate from the licenses of our code and report.

## AI use statement

### Vaibhav Sharma

I used Claude (Anthropic) as a supporting tool. It helped me understand Git, GitHub, Docker, and Quarto steps, write and debug the plotting code and captions for the exploratory figures, and proofread the report and project files. I ran the code myself, checked the figures and numbers against the data, and edited all text before using it. [Add anything else you used it for in Milestone 2, such as drafting this README and CONTRIBUTING.md, and edit this sentence so it is true.]

Example requests included:

- "[Paste one real prompt you sent]"
- "[Paste another real prompt you sent]"

### Jerry John Doviavu

I used ChatGPT to assist with Python environment setup, Git and GitHub command explanations, analysis code, debugging, interpretation of model results, and drafting documentation and review feedback. Generated suggestions were checked through local execution and teammate review where applicable. Team members performed the GitHub issue, pull request, review, and merge actions themselves.

Example requests included:

- "Before fitting it, what are some other ways of replacing the missing values?"
- "Check if we got it right," asking ChatGPT to inspect the revised data-download code.
- "Can we include the errors and the solutions in the review, in a way of suggestion?" asking for help drafting actionable review feedback.

### Ceilidh Torrance

I used ChatGPT to assist with outlining, editing, and validating the report. The actual writing was completed by me, and all pull requests were reviewed, approved, and merged by team members themselves.

Example requests included:

- "Could you provide the scaffolding for writing the summary."
- "Can you please compare this section to the report outline and let me know what needs to be revisited and what points are missing."

## References

Anwar, S. (n.d.). *Vehicle Sales Data* [Dataset]. Kaggle. https://www.kaggle.com/datasets/syedanwarafridi/vehicle-sales-data. Accessed November 26, 2026.

