# poc-pytest-split-github-actions

[![Unit tests](https://github.com/GuillaumeFalourd/poc-pytest-split-github-actions/actions/workflows/unit-tests.yml/badge.svg)](https://github.com/GuillaumeFalourd/poc-pytest-split-github-actions/actions/workflows/unit-tests.yml)

POC using pytest split with Github Actions.

## Usage

This repository highlights how to use `pytest-split` with GitHub Actions, using **parallelism**. 

`pytest-split` makes it possible to split Python (pytest) test suite to multiple "sub suites". The splitting logic takes into account execution time of each individual test which makes it possible to split the tests in close to optimal manner considering the total execution time. When pytest-split is combined with parallel execution features of GitHub Actions, the benefits can be significant vs running all tests sequentially.

## Project structure

- The `project` folder containss 10 dummy functions. First 6 execute in around 10 seconds each while last 4 take around 60 seconds to execute each.

- The `tests` folder contains 10 tests: one for each dummy function.

## Tests

Executing the tests sequentially, the total execution time is around 5 minutes (6 * 10 seconds + 4 * 60 seconds). By using parallel execution via matrix strategy and pytest-split splitting feature, we can get significant boost to total execution time of CI.

This example is using parallelization level 5 which basically means that the whole test suite is split to 5 sub suites. When the split is done optimally, one sub suite contains 6 fast tests (around 10 seconds per test) and the other 4 sub suites contain one test which each take around 60 seconds to execute. Which means that all tests could finish in around 1 minute.

_Note: The parellelism seems to only work until pytest-split==0.1.6, newest version return an exit code 5._