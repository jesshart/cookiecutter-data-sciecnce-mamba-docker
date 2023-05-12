# {{ cookiecutter.project_name }}
This is a cookie cutter template for Data Science applications deploying wit Docker.

## Assumptions
* Using `conda-lock` with `mamba` to manage dependencies.
* * `conda-lock` should be installed at a global local level like with `pipx`.
* You are using this cookiecutter template from scratch.
* Wherever you see the name `project` you should replace with your actual project name. For example, the `oatmeal` repo might have the path `oatmeal/oatmeal/main.py`. Note in this example both the repo and the application are named the same by convention.
* You should configure the `.gitignore` to fit your specific needs.

## Get Started
1. Create and activate a new environment:
```bash
$ mamba create -n dev-project python=3.10 pre_commit

$ mamba activate dev-project
```

2. Add `python=3.10` and `pre_commit` to `environment/app-env.yml` and `environment/dev-env.yml` respectively. See the concept "Stacking Environments" below for more context.

```yaml
# app-env.yml
channels:
  - conda-forge
dependencies:
  - python=3.10 # ADDED
platforms: # This is non-standard and recommended by conda-lock.
  - linux-64
  # - linux-aarch64 # aka arm64, use for Docker on Apple Silicon
  # - osx-64
  # - win-64
  - osx-arm64 # For Apple Silicon, e.g. M1/M2
```

Note: As you add direct dependencies, add them to the proper `environment/*-env.yml` file. For example, if it is an app dependency it goes into `environment/app-env.yml` whereas `pytest` would go into `environment/tes-env.yml`.

3. Install the pre-commit while in the root of the project:

```bash
$ pre-commit install
```

4. Once you find a happy stopping point you should have direct dependencies across your environment files. Your `Dockerfile` will need an `app-linux-64.lock` in order to run the application which is accomplished by redering the `app-env.yml` file.
```bash
$ conda-lock -f app-env.yml --lockfile infrastructure/environments/multiplatform/app.conda-lock.yml

$ conda-lock render app-conda-lock.yml -p linux-64 --filename-template infrastructure/locks/app-{platform}.lock
```

# Concepts
## Stacking
`conda-lock` allows you to "stack" environments so you can manage dependencies per environment. This is convenient because your dev environment needs your test and app environments. They stack like so (in size): dev > test > app.

For example, if the project has matured, you can install the dev environment on a new machine by running this:
```bash
$ conda-lock -f dev-env.yml -f test-env.yml -f app-env.yml --lockfile infrastructure/environments/multiplatform/dev.conda-lock.yml
```

