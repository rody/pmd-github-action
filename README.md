# About

Github Action to run [PMD](https://pmd.github.io) and analyse your source code.

Under the hood, it uses the following Docker image [rawdee/pmd](https://hub.docker.com/r/rawdee/pmd).

---

## Usage

#### `.github/workflows/check.yml`

```yaml
on: [push]

jobs:
  deploy_check_job:
    runs-on: ubuntu-latest
    name: Analyse Source Code with PMD

    steps:
      # Using checkout is required
      - name: Checkout
        uses: actions/checkout@v2

      # Run checks
      - name: PMD checks
        id: pmd-checks
        uses: rody/pmd-gh-action 
```

## Input

See [action.yml](action.yml)

## Examples

### Generate and publish a PMD report

```yaml
on: [push]

jobs:
  deploy_check_job:
    runs-on: ubuntu-latest
    name: Analyse Source Code with PMD

    steps:
      - name: Checkout
        uses: actions/checkout@v2

      # Generate report
      - name: Generate PMD report
        id: generate-pmd-report
        uses: rody/pmd-github-action@main
        with:
          reportfile: 'pmd-report.json' # relative to the repo root
          format: 'json'

          # optional:
          # make the action successful even when violations were found
          failOnViolation: false
    
      # Publish the report as a Github artifact    
      - name: Publish PMD report
        uses: actions/upload-artifact@v2
        with:
          name: PMD Report
          path: 'pmd-report.json'
          if-no-files-found: ignore
```

### Create annotations from the report

[pmd-annotations-github-action](https://github.com/rody/pmd-annotations-github-action) can be used to
create annotations from a PMD report.

  ``` yaml
  name: Analyse Source Code
on: [push]

jobs:
  analysis:
    name: Analysis
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Create PMD Report
        uses: rody/pmd-github-action@main
        with:
          rulesets: 'rulesets/apex/quickstart.xml'
          reportfile: 'pmd-report.json'
          format: 'json'
          failOnViolation: 'false'

      - name: Create PMD annotations
        uses: rody/pmd-annotations-github-action@main
        with:
          reportfile: 'pmd-report.json'
  ```
  

## Alternative

If you need a specific version of PMD, you can use directly the Docker
image [rawdee/pmd](https://hub.docker.com/r/rawdee/pmd) like this:

```yaml
on: [push]

jobs:
  deploy_check_job:
    runs-on: ubuntu-latest
    name: Analyse Source Code with PMD

    steps:
      - name: Checkout
        uses: actions/checkout@v2

      # Generate report
      - name: Generate PMD report
        id: generate-pmd-report
        uses: uses: docker://rawdee/pmd:latest
        with:
          args:
            dir: '.'
            rulesets: 'rulesets/apex/quickstart'

```

# License

The scripts and documentation in this project are released under the [MIT License](LICENSE)

# Contributions

Contributions are welcome!

## Code of Conduct

:wave: Be nice and respectful.
