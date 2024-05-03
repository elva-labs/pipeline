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
      - uses: elva-labs/pipeline/node/turbo-aws@1dfab0a8a811874e53190bf1bec0b5f76acedb32
        with:
          name: turbo-ci
          npm_token: ${{ secrets.CI_GH_TOKEN }}
          npm_registry: https://npm.pkg.github.com/
          npm_scope: "..."
          aws_role_arn: ${{ vars.DEPLOYMENT_ROLE }}
          aws_region: eu-north-1

      - name: Check
        run: pnpm check

      - name: Includes a changeset
        run: pnpm changeset status --since origin/main
```
