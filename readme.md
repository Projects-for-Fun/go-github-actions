# Actions for my GitHub Actions workflows

## Linting

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
            
          - name: Linting
            uses: ./actions/linting
```

## Run unit tests

`SAVE_REPORTS` saves the coverage.out and and report.json required by sonar.

Example
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
            uses: ./actions/unitTests
            with:
                SAVE_REPORTS: true
```
