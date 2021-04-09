# Benchmark code for _Accelerated Component-wise Gradient Boosting using Efficient Data Representation and Momentum-based Optimization_

TODO:
- What happens here?
- There is also a docker (ref)
- Disclaimer on runtime
- Manual inspections

## File/Folder descriptions

#### Main file: `src/bm-run.R`

This file defines the used learner and tasks. The tasks are given as `data.frame` with different tags.
For example, the `albert` data is not available as task, therefore, the dataset was downloaded as
csv and an export script `load-albert.R` was written. The tag for `albert` is `file`.

The main purpose is also to build the configuration grid of all tasks and learners and runs the mlr benchmark
by starting a new session and sourcing `mlr-run.R` for one of the configuration. This ensures that one learner
is benchmarked with one task at a time. If an error occurs, this does not effect the other jobs.

#### Conduct benchmark for one configuration: `src/mlr-run.R`

This file is the workhorse of the benchmark. This includes loading all packages, the task, learner, parameter space,
and tuning/evaluation technique. The file sources the following files:
- Load tasks: `src/tasks.R`
- Load learners: `src/learners.R`
- Load parameter space: `src/param-sets.R`
- Load the final design: `src/design.R`

The benchmark is executed by calling `mlr3::benchmark` on the final design.

#### Extra learners

The learner `xgboost`, `ranger`, and `gamboost` are already available using `mlr3learners` and `mlr3extralearner` while
`compboost` and `interpret` is not. Therefore, the folder `src/mlr-bmr/learner-src/` contains the `mlr3` definition of these
two learners `classifCompboost.R` and `classifInterpretML_reticulate.R`. The source of `compboost` is available on GitHub
using the commig `ba044d3a6f6814080eb097acca2e59fd8bad9805` and can be install via `remotes`:
```
remotes::install_github("schalkdaniel/compboost", ref = "ba044d3a6f6814080eb097acca2e59fd8bad9805")
```

To use `interpret` requires further installation steps because it is called via the `python` package. The main reason for that is to use multicore processing which was not available at the time when running the benchmark:
1. Install `python3`, `python3-pip`, and `python3-venv`
2. Create an virtual environment:
  1. Create an new folder `~/venv/` in your home directory.
  2. Run `python3 -m venv ~/venv/ebm`
3. Run `source ~/venv/ebm/bin/activate`
4. Install packages `pip3 install pandas sklearn interpret` (note that it must be installed via the python executable in the virtual environment located at `~/venv/ebm/bin/python3`).
5. Also install the `R` package reticulate` to run interpret from `R`.

If the installation of `llvm` fails or requires a specific version visit https://apt.llvm.org/, call
`export LLVM_CONFIG=/usr/bin/llvm-config-10` in the case of llvm 10, and run `pip3 install llvmlite`.

All installation steps where executed on virtual machines using Debian 10 "Buster".

## Inspect benchmark results

TODO:
- Script to load benchmark results and create plots of the paper

