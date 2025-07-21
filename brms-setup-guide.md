# BRMS and STAN Setup Guide for SDS Cluster

This guide provides instructions for setting up BRMS with STAN on the University of Mannheim Social Data Science cluster server.

## Server Information

- **URL**: https://sdscluster.uni-mannheim.de
- **Login**: Use your assigned SSC credentials

## Prerequisites

**Note**: STAN and brms only need to be installed once from R. After the initial setup, you can use them directly in future sessions.

## Installation Steps

### Step 1: Install System Dependencies (Terminal)

Run the following commands from the terminal:

```bash
sudo apt update
sudo apt install -y \
    libxml2-dev \
    libfontconfig1-dev \
    libfreetype6-dev \
    libharfbuzz-dev \
    libfribidi-dev \
    libpng-dev \
    libtiff5-dev \
    libjpeg-dev \
    libgit2-dev \
    libglpk-dev \
    libcurl4-openssl-dev \
    libssl-dev \
    libgmp-dev \
    libgsl-dev \
    libgl1-mesa-dev \
    cmake \
    libtool \
    automake \
    gfortran \
    liblapack-dev \
    libblas-dev
```

### Step 2: Install IRkernel for Jupyter (Terminal)

Call R from bash to install the R kernel for Jupyter notebooks:

```bash
R -e "install.packages('IRkernel'); IRkernel::installspec()"
```

### Step 3: Install STAN and BRMS (R/Jupyter Notebook)

Run the following commands in R (either in RStudio or a Jupyter notebook):

```r
# Install devtools and cmdstanr
install.packages("devtools", repos = "https://cloud.r-project.org")
devtools::install_github("stan-dev/cmdstanr")

# Install CmdStan
library(cmdstanr)
cmdstanr::install_cmdstan()

# Verify installation
cmdstanr::cmdstan_version()

# Install brms
install.packages('brms')
library(brms)
```

### Step 4: Test Installation

Test your installation with a simple BRMS model:

```r
# Example model using built-in dataset
fit <- brm(
  formula = count ~ zAge + zBase * Trt + (1|patient),
  data = epilepsy, 
  family = poisson(),
  backend = "cmdstanr",
  chains = 2, 
  cores = 2, 
  iter = 500
)

# View results
summary(fit)
```

## Troubleshooting

- If the installation doesn't work as expected, consider using alternative installation methods or consulting the [BRMS documentation](https://paul-buerkner.github.io/brms/)
- For Jupyter notebook integration issues, ensure that the IRkernel is properly installed and registered
- If you encounter permission issues, contact the server administrator

## Additional Notes

- This setup enables STAN to run on the server via Jupyter notebooks
- After initial installation, you only need to load the libraries (`library(brms)`, `library(cmdstanr)`) in future sessions
- The server supports parallel processing, so you can adjust `chains` and `cores` parameters based on your computational needs

---

*Last updated: July 2025*  
*University of Mannheim - Chair of Social Data Science*
