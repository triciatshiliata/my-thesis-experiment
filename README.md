# Parallel vs. Sequential Cypress Testing in GitHub Actions

An experiment measuring how parallelizing an end-to-end test suite across multiple runners affects CI pipeline time. This repo is the experimental setup from my undergraduate thesis at the University of North Texas.

## Research question
Does running a Cypress suite in parallel across four GitHub Actions runners reduce pipeline time compared with running it sequentially, and how consistent are the results?

## Method
- **Workload:** 120 Cypress end-to-end tests (12 spec files) adapted from the [Cypress Kitchen Sink](https://github.com/cypress-io/cypress-example-kitchensink) example app
- **Sequential baseline:** all tests run on one runner
- **Parallel configuration:** tests split across 4 runners using a GitHub Actions matrix with Cypress Cloud parallelization
- **Sample size:** 10 runs of each configuration, from manually triggered workflow runs (#3 to #12 in the Actions tab). Runs #1 and #2 were failed setup runs and are excluded.

## Results
- **Sequential:** 139.9 s average pipeline time, 12.2% coefficient of variation
- **Parallel (4 runners):** 48.8 s average pipeline time, 40.6% coefficient of variation
- **Average speedup:** 2.87x, about a 65% reduction in pipeline time
- Parallel runs were faster on average but less consistent. The first parallel run was a cold start (92 s), and Cypress Cloud did not always distribute the spec files evenly across the runners.

## Repo contents
- `.github/workflows/experiment.yml`: the workflow containing both configurations
- `cypress/`: the test suite
- `cypress.config.js`: Cypress configuration

## Running it yourself
1. Fork the repo.
2. Create a free project in [Cypress Cloud](https://www.cypress.io/cloud) and add your record key as a repository secret named `CYPRESS_RECORD_KEY`.
3. Replace `projectId` in `cypress.config.js` with your own.
4. Go to **Actions → Thesis Experiment → Run workflow**.

## Author
Patricia Tshiliata, BS Computer Science, University of North Texas
