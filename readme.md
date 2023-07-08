# Actions for my GitHub Actions workflows

- [Lint](#lint)
- [Run unit tests](#run-unit-tests)
- [Sonar](#sonar)
    - [Sonar (for unit and integration tests)](#sonar-for-unit-and-integration-tests)
    - [Sonar for unit tests only](#sonar-for-unit-tests-only)

## Lint

Run golanci-lint with .golangci.yml defined in [this repo](https://github.com/Projects-for-Fun/golangci).

Example:

```
jobs:
    lint:
        name: Lint
        runs-on: ubuntu-latest
        steps:
        
          - name: Checkout
            uses: actions/checkout@v3
            with:
              # Disabling shallow clone is recommended for improving relevancy of reporting
              fetch-depth: 0
            
          - name: Linting
            uses: Projects-for-Fun/go-github-actions/Lint@<tag>
```

## Run unit tests

`SAVE_REPORTS` saves the `coverage.out` and `report.json` files required by [sonar](#sonar).

Example:

```
jobs:
    unit-tests:
        name: Unit tests
        runs-on: ubuntu-latest
        steps:
    
          - name: Checkout
            uses: actions/checkout@v3
            with:
              # Disabling shallow clone is recommended for improving relevancy of reporting
              fetch-depth: 0

          - name: Run unit tests
            uses: Projects-for-Fun/go-github-actions/UnitTests@<tag>
            with:
                SAVE_REPORTS: true
```

## Sonar

### Sonar (for unit and integration tests)

Uploads the unit and integration tests reports to SonarCloud.

Requires `GITHUB_TOKEN` and `SONAR_TOKEN`.

It requires running integration tests and `coverage-it.out` and `report-it.json`.

Example:

```
jobs:
    sonar:
        name: Sonar
        runs-on: ubuntu-latest
        needs: [lint, unit-tests, integration-tests]
        steps:
        
            - name: Checkout
              uses: actions/checkout@v3

            - name: Sonar
              uses: Projects-for-Fun/go-github-actions/Sonar@<tag>
              with:
                GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
                SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
```

### Sonar for unit tests only

Uploads the `coverage.out` and `report.json` files generated under the [Lint](#lint) action to SonarCloud.

Requires `GITHUB_TOKEN` and `SONAR_TOKEN`.

Example:

```
jobs:
    sonar:
        name: Sonar
        runs-on: ubuntu-latest
        needs: [lint, unit-tests, integration-tests]
        steps:
        
            - name: Checkout
              uses: actions/checkout@v3

            - name: Sonar
              uses: Projects-for-Fun/go-github-actions/SonarForUnitTests@<tag>
              with:
                GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
                SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
```
