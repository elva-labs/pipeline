# Example

```yml
name: CI Branch

on:
  pull_request:
    types: [opened]

  push:
    branches:
      - "**" # matches every branch
      - "!main" # excludes master

jobs:
  ci:
    runs-on: ubuntu-latest
    environment: "dev"
    timeout-minutes: 15
    steps:
      - uses: elva-labs/pipeline/node/turbo-aws@0b80e0575832115442ce8a21719c3ad879bd749a
        with:
          name: turbo-ci
          npm_token: ${{ secrets.CI_GH_TOKEN }}
          npm_registry: https://npm.pkg.github.com/
          npm_scope: "@your-private-scope"
          aws_role_arn: ${{ vars.DEPLOYMENT_ROLE }}
          aws_region: eu-north-1
          fetch_depth: 0 # Needed to allow changeset to compare against main

      - name: Check
        run: pnpm check

      - name: Includes a changeset
        run: pnpm changeset status --since origin/main

permissions:
  id-token: write
  contents: read
```
