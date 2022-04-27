# Flexlate Update Action

Official Flexlate Github Action for updating templates

Use this action to automatically open a PR with any template updates.

## Inputs

- `gh_token`: The Github token to use for authentication
- `main_branch_name`: The repository main branch, defaults to `master`
- `pr_label`: The label(s) to use for the PR, defaults to two labels: `no auto merge,automated pr`

## Other Requirements

There are two actions you must run before the update action:

- [`actions/checkout`](https://github.com/actions/checkout): With `fetch-depth` set to `0` to fetch all branches so that Flexlate
  can do the update.
- [`actions/setup-python`](https://github.com/actions/setup-python): To allow the installation of Flexlate

You must run `actions/checkout` with You must also run `actions/setup-python`

## Examples

Here's an example workflow that uses the Flexlate Update Action to check daily to open a PR with
template updates. It also includes an option to trigger the update manually in the Github UI.

```yaml
name: Update Template using Flexlate

on:
  schedule:
    - cron: "0 3 * * *" # every day at 3:00 AM
  workflow_dispatch:

jobs:
  templateUpdate:
    runs-on: ubuntu-latest
    strategy:
      max-parallel: 1
      matrix:
        python-version: [3.8]

    steps:
      - uses: actions/checkout@v3
        with:
          ref: ${{ github.ref_name }}
          fetch-depth: 0
          token: ${{ secrets.gh_token }}
      - name: Set up Python ${{ matrix.python-version }}
        uses: actions/setup-python@v3
        with:
          python-version: ${{ matrix.python-version }}
      - uses: nickderobertis/flexlate-update-action@v1
        with:
          gh_token: ${{ secrets.gh_token }}
```

## Development Status

This project uses [semantic-release](https://github.com/semantic-release/semantic-release) for versioning.
Any time the major version changes, there may be breaking changes. If it is working well for you, consider
pegging to the current major version, e.g. `flexlate-update-action@v1`, to avoid breaking changes. Alternatively,
you can always point to the most recent stable release with the `flexlate-update-action@latest`.

## Developing

Clone the repo and then run `npm install` to set up the pre-commit hooks.

## Author

Created by Nick DeRobertis. MIT License.
