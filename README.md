# Predicting Vehicle Selling Prices
 
Authors: Jerry John Doviavu, Vaibhav Sharma, and Ceilidh Torrance.
 
## Project summary
 
How well can vehicle year and odometer readings predict selling prices? Using a reproducible sample of 5,000 records from the Vehicle Sales Data dataset, we compared linear regression with a baseline that predicts the median training price. Linear regression reduced mean cross-validation mean absolute error (MAE) by approximately 26.3%, from 7,092.95 USD to 5,229.50 USD. On the held-out test set, MAE was 5,319.82 USD and R² was approximately 0.40.
 
Vehicle year and odometer readings provide useful predictive information, but substantial errors remain. The model underestimates expensive vehicles and produces some unrealistic negative predictions, limiting its usefulness for precise individual valuations. Test performance was evaluated only for vehicles with recorded selling prices.
 
## Report
 
[Read the published analysis report](https://ubc-mds-2026-27.github.io/DSCI-522-Vehicle-Price-Predictions/).
 
The [Quarto source](analysis/vehicle-price-prediction.qmd) and [rendered HTML](analysis/vehicle-price-prediction.html) are included in this repository. To reproduce the report locally, follow the instructions below.
 
## Data
 
The analysis uses [Vehicle Sales Data on Kaggle](https://www.kaggle.com/datasets/syedanwarafridi/vehicle-sales-data), specifically `car_prices.csv`.
 
The report downloads the CSV into `data/raw/` when it is missing and reuses it when it already exists. It selects 5,000 records using random seed 522 and splits them into 4,000 training observations and 1,000 test observations before exploratory analysis or learned preprocessing. The splits are saved in `data/processed/`.
 
The predictors are vehicle year and odometer reading, and the target is selling price. Missing predictor values are handled with median imputation inside the modelling pipeline. Imputation values are learned only from the relevant training data, including within each cross-validation fold.
 
## Dependencies
 
We run the analysis in a Docker container, so everyone uses the same software and package versions. [Docker](https://www.docker.com/) is the only program you need to install to run the analysis. The image is built from the [Dockerfile](Dockerfile) in this repository. It installs Python 3.12, Quarto 1.11.9, and uv 0.13.7, and then the Python packages declared in [pyproject.toml](pyproject.toml), with the exact versions locked in [uv.lock](uv.lock). The image is published on Docker Hub at [ceilidhmtorrance/dsci-522-vehicle-price-predictions](https://hub.docker.com/r/ceilidhmtorrance/dsci-522-vehicle-price-predictions).
 
The analysis was developed and tested on macOS using Python 3.12, uv 0.12.5, and Quarto 1.10.18. Git is also required to clone the repository. In the Docker container, the analysis runs on Python 3.12, uv 0.13.7, and Quarto 1.11.9.
 
The direct Python dependencies are pandas, scikit-learn, matplotlib, Jupyter, and kagglehub. Dependency requirements are declared in [pyproject.toml](pyproject.toml), and resolved versions are recorded in [uv.lock](uv.lock). The container installs them for you. Without Docker, run `uv sync --locked` to recreate the Python environment.
 
To run without Docker, Git, uv, and Quarto must be installed separately.
 
## How to run the analysis
 
There are two ways to run the analysis. Option 1 uses Docker and is the recommended way, because it needs no other software. Option 2 uses uv and Quarto installed on your own computer. You only need to follow one of them.
 
### Option 1: Run in a Docker container (recommended)
 
#### Set up Docker and clone the repository
 
The first time you run the project, install [Git](https://git-scm.com/downloads) and [Docker](https://www.docker.com/get-started/), and start Docker Desktop and wait until it shows that Docker is running. Then clone the repository and move into it:
 
```bash
git clone https://github.com/UBC-MDS-2026-27/DSCI-522-Vehicle-Price-Predictions.git
cd DSCI-522-Vehicle-Price-Predictions
```
 
Run all the commands below from the root of this repository.
 
#### 1. Make an ssh key
 
If you do not have an ssh key yet, make one and press Enter at each question:
 
```bash
ssh-keygen -t ed25519
```
 
If you have never used Git on this computer, also run `git config --global user.name "Your Name"` once, so that the `~/.gitconfig` file exists. The container uses your ssh key to let you log in. If you skip this step, Docker creates a *folder* named `~/.ssh/id_ed25519.pub` instead, and connecting to the container over ssh fails until you delete that folder and make the key.
 
#### 2. Download the image
 
```bash
docker compose pull
```
 
#### 3. Render the report in the container
 
```bash
docker compose run --rm analysis-env uv run quarto render analysis/vehicle-price-prediction.qmd
```
 
This executes the analysis and creates `analysis/vehicle-price-prediction.html`, replacing the version in this repository. The raw data file `data/raw/car_prices.csv` is included in this repository, so the report reuses it and does not need a Kaggle login. If you delete that file, the report downloads it again, which needs an internet connection. The report also overwrites the training and test files in `data/processed/`. Because of the `--rm` option, the container removes itself when the render finishes, so there is nothing to stop afterwards.
 
### Option 2: Run without Docker
 
These are the original instructions. You do not need them if you use Option 1.
 
#### Prerequisites
 
Install [Git](https://git-scm.com/downloads), [uv](https://docs.astral.sh/uv/getting-started/installation/), and [Quarto](https://quarto.org/docs/get-started/)
 
The project uses Python 3.12. Python dependencies are declared in [pyproject.toml](pyproject.toml), and resolved package versions are recorded in [uv.lock](uv.lock). The Python version request is recorded in `.python-version`.
 
#### Clone the repository
 
Run these commands in your terminal:
 
```bash
git clone https://github.com/UBC-MDS-2026-27/DSCI-522-Vehicle-Price-Predictions.git
cd DSCI-522-Vehicle-Price-Predictions
```
 
Run all remaining commands from this repository folder.
 
#### Recreate the environment
 
```bash
uv sync --locked
```
 
This creates a local `.venv` and installs the dependencies recorded in the lockfile. The virtual environment is not committed to Git.
 
The direct Python dependencies are pandas, scikit-learn, matplotlib, Jupyter, and kagglehub.
 
#### Verify the dependencies
 
```bash
uv run python -c "import pandas, sklearn, matplotlib, jupyter, kagglehub; print('All five packages imported successfully')"
quarto --version
```
 
Git, uv, and the Quarto executable are installed separately from the project's Python dependencies.
 
#### Render the report
 
```bash
uv run quarto render analysis/vehicle-price-prediction.qmd
```
 
This executes the analysis and creates `analysis/vehicle-price-prediction.html` and its supporting assets. Open the HTML file in a browser to read the report.
 
Use `uv run` to execute commands in the project environment; manual activation is not required. Internet access is needed when the raw CSV must be downloaded.
 
## Contributing
 
If you want to change the analysis, add a package, or update the published report, please see [CONTRIBUTING.md](CONTRIBUTING.md).
 
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
| `README.md` | Project overview and reproduction instructions |
| `LICENSE.md` | Code and report licenses |
| `CONTRIBUTING.md` | Contribution and review guidelines |
| `.gitignore` | Patterns excluding local and temporary files from tracking |
| `Dockerfile` | Recipe for the container image |
| `docker-compose.yml` | Settings to run the published image |
| `.github/workflows/publish-dockerhub.yml` | GitHub Actions workflow that builds and publishes the image |
| `docs/index.html` | Copy of the rendered report, published with GitHub Pages |
 
## Licenses
 
The software code is licensed under the MIT License. The analysis report is licensed under Creative Commons Attribution-NonCommercial-NoDerivatives 4.0 International (CC BY-NC-ND 4.0). See [LICENSE.md](LICENSE.md) for the project’s licensing documentation.
 
The Vehicle Sales Data dataset is listed on [Kaggle](https://www.kaggle.com/datasets/syedanwarafridi/vehicle-sales-data) under the MIT License. Its licensing is separate from the licenses applied to this project's code and report.
 
## AI use statement
 
### Vaibhav Sharma
 
I used ChatGPT as a supporting tool during this project to help understand some concepts, debug errors, and get suggestions for improving my code. I reviewed the AI-generated suggestions, made changes where needed, and tested the code myself to make sure it worked correctly. The final implementation and decisions were made by me.
 
### Jerry John Doviavu
 
I used ChatGPT to assist with Python environment setup, Git and GitHub command explanations, analysis code, debugging, interpretation of model results, and drafting documentation and review feedback. Generated suggestions were checked through local execution and teammate review where applicable. Team members performed the GitHub issue, pull request, review, and merge actions themselves.
 
Example requests included:
 
- “Before fitting it, what are some other ways of replacing the missing values?”
- “Check if we got it right”—asking ChatGPT to inspect the revised data-download code.
- “Can we include the errors and the solutions in the review, in a way of suggestion?”—asking for help drafting actionable review feedback.
### Ceilidh Torrance
 
I used ChatGPT to assist with outlining, editing, and validating the report. The actual writing was completed by me, and all pull requests were reviewed, approved, and merged by team members themselves. 
 
Example requests include:
 
- "Could you provide the scaffolding for writing the summary."
- "Can you please compare this section to the report outline and let me know what needs to be revisited and what points are missing."
## References
 
Anwar, S.(n.d.). *Vehicle Sales Data* [Dataset]. Kaggle. https://www.kaggle.com/datasets/syedanwarafridi/vehicle-sales-data. Accessed November 21, 2026.
 
