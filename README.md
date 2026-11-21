# Saved observation paths for JSS manuscript 5773

Observation data for **Fast and Stable Estimation for Continuous Linear State Space Models in the yuima Package**, by Masahiro Kurisaki and Kosuke Kito (JSS manuscript 5773).

These data accompany the article's replication materials. They allow the accuracy experiments to be rerun with exactly the stored observation values, without generating new observations.

## Download

Download [`observation_paths.tar.gz` from release v1.0.0](https://github.com/makrisaki/yuima-jss5773-data/releases/download/v1.0.0/observation_paths.tar.gz) (257,074,284 bytes, approximately 257 MB).

The archive is attached to the GitHub Release. The repository itself contains this README and `SHA256SUMS`.

## Contents

The archive contains five files under `Data/`:

| File | Accuracy experiment |
| --- | --- |
| `accuracy_test_1dim_h_paths.rds` | One-dimensional model, varying step size |
| `accuracy_test_1dim_T_paths.rds` | One-dimensional model, varying observation horizon |
| `accuracy_test_drop_1dim_paths.rds` | One-dimensional model, comparison with discarded initial terms |
| `accuracy_test_2dim_h_paths.rds` | Two-dimensional model, varying step size |
| `accuracy_test_2dim_T_paths.rds` | Two-dimensional model, varying observation horizon |

Each RDS file is a list of 50 observation matrices in seed order, 1 through 50. Each matrix has 100,001 rows, with observations at times 0 through 100 at spacing 0.001. The one-dimensional files have one observed-variable column; the two-dimensional files have two. The replication scripts select the required observation horizon and step size from these stored paths.

## Extract into the replication materials

Download the article's replication materials separately. Place the archive in their root directory, which contains `Code/` and `Figures/`. Set that directory as the R working directory and run:

```r
untar("observation_paths.tar.gz", exdir = ".")
```

The five RDS files will then be available in `Data/`. The CSV results needed to regenerate the figures are supplied separately in the replication materials' `Code/data.tar.gz`.

## Re-estimate from saved observations

In `Code/section_4_3_generate_data.R`, add the matching `observation_file` argument to each experiment's existing `parallel_accuracy_test()` call. For example, for the one-dimensional step-size experiment, add:

```r
observation_file = "Data/accuracy_test_1dim_h_paths.rds"
```

The five calls include comments giving the corresponding file names. Keep the original model, grid, initial values, parameter bounds, and other arguments for each experiment. With `observation_file` supplied, the function reads the saved paths and re-estimates the parameters; it does not generate new observations. The resulting estimates are written to the call's `output_file` CSV.

Without `observation_file`, the script generates observations again. Saving and reusing paths separates observation-generation differences from numerical differences during estimation.

## Numerical environment

The reference experiment used R 4.4.2, yuima 1.15.34, and RcppArmadillo 14.2.0-1 when compiling yuima. The FKF version reported in the manuscript is 0.2.6. The replication scripts retain the default L-BFGS-B controls in `optim()`.

The RcppArmadillo version **at yuima compilation time** matters: replacing RcppArmadillo after installing a compiled yuima package does not change the code already compiled into yuima. The RDS files preserve the stored observation values, but numerical estimation can still vary across software and computing environments.

## Verify the archive

SHA-256:

```text
5c7a42336c9385823f70d97be85366f54efeca179b1f4854f774385f8a4d4805  observation_paths.tar.gz
```

On Windows PowerShell:

```powershell
Get-FileHash -Algorithm SHA256 observation_paths.tar.gz
```

With `sha256sum` available, download `SHA256SUMS` and run:

```sh
sha256sum -c SHA256SUMS
```
