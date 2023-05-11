# Overview
This is a cookie cutter template for Data Science applications.

## Assumptions
* Using `conda-lock` with `mamba` to manage dependencies.
* You are using this cookiecutter template from scratch.
* Wherever you see the name `project` you should replace with your actual project name. For example, the `oatmeal` repo might have the path `oatmeal/oatmeal/main.py`. Note in this example both the repo and the application are named the same by convention.

## Get Started
1. Create and activate a new environment:
```bash
$ mamba create -n dev-project python=3.10 pre_commit
$ mamba activate dev-project
```
2. Add `python=3.10` and `pre_commit` to `environment/app-env.yml` and `environment/dev-env.yml` respectively. See the concept "Stacking Environments" below for more context.
 
Note: As you add direct dependenies, add them to the proper `environment/*.yml` file. For example, if it is an app dependency it goes into `environment/app-env.yml` whereas `pytest` would go into `environment/tes-env.yml`.

# Concepts
## Stacking
`conda-lock` allows you to "stack" environments so you can manage dependencies per environment. This is convenient because your dev environment needs your test and app environments. They stack like so (in size): dev > test > app.

For example, if the environment has matured, you can install the dev environment on a new machine by running this:
```bash
$ conda-lock -f dev-env.yml -f test-env.yml -f app-env.yml --lockfile infrastructure/environments/multiplatform/dev.conda-lock.yml
```