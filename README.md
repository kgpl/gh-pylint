# PyLint - GitHub Action

GitHub action that lets you *easily* lint **one** or **multiple** packages/files of your project. 

  > Adds a **dynamic badge** to your `README.md` that lets you display the obtained score!
  
  > User customizable $Fail Score$ (defaults to 5/10).
  > > Auto Calculates step size based on $Fail Score$.
  >
  > > Fails job after updating badge.


Each time the action is run, packages specified will be linted and a badge in the `README.md` is updated dynamically
following one of the below rules:

|      Range PyLint score                                             | Status          | Badge                                               |
|:-------------------------------------------------------------------:|:---------------:|:---------------------------------------------------:|
|     *PyLint score* $<= Fail Score$                                  | Fail            | ![pylint-darkred](/assets/images/darkred.svg)       |
|     $FailScore < *PyLint score* <= (FailScore + 1*step)$          | Need Improvement| ![pylint-red](/assets/images/red.svg)               |
|     $(FailScore + 1*step) < *PyLint score* <= (FailScore + 2*step)$  | Below Average   | ![pylint-orange](/assets/images/orange.svg)         |
|     $(FailScore + 2*step) < *PyLint score* <= (FailScore + 3*step)$  | Average         | ![pylint-yellow](/assets/images/yellow.svg)         |
|     $(FailScore + 3*step) < *PyLint score* $<= (FailScore + 4*step)$  | Good            | ![pylint-lightgreen](/assets/images/lightgreen.svg) |
|     $(FailScore + 4*step) < *PyLint score* <= 10$                   | Awesome         | ![pylint-green](/assets/images/green.svg)           |

The action can be triggered by a **`Pull request`**, a **`Push`** or manually with **`workflow_dispatch`**. 
If the score is changed, the `github_action` bot will change your badge with an automatic commit.

* **IMPORTANT!** Follow the ['Preliminary steps' section](#preliminary-steps) in order to allow the bot to update your 
README.md with the pylint badge!


A quick example on how you would typically use this *action* (more examples in [scenario section](#scenario))
```yaml
- uses: kgpl/gh-pylint@v1
  with:
    package-path: src  # lint src package
    python-version: 3.11  # python version which will lint the package
```

## Preliminary steps

To use this action you should perform two simple **first-time-only** operations:

1. In order to have a dynamic updated badge, before using for the first time this action, you should put a ***placeholder
badge*** in your `README.md` which will be substituted by the actual one as soon as you run this action. If placeholder is not there, badge will be automatic appended to begining of second line.\
The placeholder badge should be in one of the following formats:
<p align="center"><b><code>![pylint]()</code></b> or <b><code>[![pylint]()](https://redirect/link)</code></b></p>

2. Be sure to set ***write permissions*** to GitHub actions in your repo settings!
You can change it in `Settings > Actions > General`, then go to subsection **Workflow Permissions** and thick the
***Read and write permission*** option

## Usage

```yaml
- uses: kgpl/gh-pylint@v1
  with:
    
    # Path of the package(s) to lint, relative to the repository root. 
    # If more than one package should be linted, simply specify all of them 
    # with the multi-line notation like so:
    # package-path: |
    #   src
    #   other_src
    #   ...
    # 
    # Required
    package-path: src
    
    # Version of the Python interpreter which will install all requirements of your project 
    # and lint the package(s) specified with the `package-path` argument
    #
    # Required
    python-version: 3.11

    # Path of the requirements of your project, relative to the repository root. 
    # This can be easily changed in case you have `requirements-dev.txt`
    #
    # Optional, Default: requirements.txt
    requirements-path: requirements.txt
    
    # Path of the README.md to update with the pylint badge, relative to the repository root.
    #
    # Optional, Default: README.md
    readme-path: README.md

    # Score below which Linting should fail
    #
    # Optional, Default: 5
    fail-below: 5
```

## Scenario

* [Single package to lint](#single-package-to-lint)
* [Multiple packages to lint](#multiple-packages-to-lint)
* [Different path for requirements file](#different-path-for-requirements-file)
* [Different path for README.md file](#different-path-for-readmemd-file)
* [Fail below Lint score](#fail-below-lint-score)

### Single package to lint

```yaml
- uses: kgpl/gh-pylint@v1
  with:
    package-path: src
    python-version: 3.11
```

### Multiple packages to lint

```yaml
- uses: kgpl/gh-pylint@v1
  with:
    package-path: |
      src
      app
      other_src/inner_src
    python-version: 3.11
```

### Different path for requirements file

```yaml
- uses: kgpl/gh-pylint@v1
  with:
    package-path: src
    python-version: 3.11
    requirements-path: requirements/requirements-dev.txt
```

### Different path for README.md file

```yaml
- uses: kgpl/gh-pylint@v1
  with:
    package-path: src
    python-version: 3.11
    readme-path: models/README.md
```

### Fail below Lint Score

```yaml
- uses: kgpl/gh-pylint@v1
  with:
    package-path: src
    python-version: 3.11
    fail-below: 8


```
## Credits

This is a composite github action which uses the following godly working actions:

* [actions/checkout](https://github.com/actions/checkout)
* [actions/setup-python](https://github.com/actions/setup-python)
* [EndBug/add-and-commit](https://github.com/EndBug/add-and-commit)

This Action Repo is forked from: [Silleellie/pylint-github-action](https://github.com/Silleellie/pylint-github-action)
