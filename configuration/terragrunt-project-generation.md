# Terragrunt project generation

Digger using a single terragrunt project if you just list it as a project as follows:\


```
projects:
- name: dev
  dir: dev
  terragrunt: true
```

This will perform a `terragrunt apply` after changes are detected within this directory.

However in many cases with terragrunt you don't want to mention all of your terragrunt components since there can be tens or hundreds of those (not to mention all the dependencies of those). In this case you can just liase it to digger and it will perform dynamic generation of projects for you and trigger the relevant `terragrunt apply` commands on all impacated projects per pull request. It will also handle dependencies of these projects. You can configure this using the following:

```
generate_projects:
  terragrunt_parsing:
    parallel: true
    createProjectName: true
    createWorkspace: true
    defaultWorkflow: default
workflows:
  default:
    plan:
      steps:
        - init
        - plan
        - run: echo "Hello World"
```

And the workflow for this needs to use `setup-terragrunt: true` as follows:

```
name: Digger

on:
  pull_request:
    branches:
      - main
      - master
    types: [ closed, opened, synchronize, reopened ]
  issue_comment:
    types: [created]
    if: contains(github.event.comment.body, 'digger')
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:

      - name: digger run
        uses: diggerhq/digger@v0.1.31 # << update me to latest version
        with:
          setup-google-cloud: true # FOR aws use setup-aws instead
          google-auth-credentials: '${{ secrets.DIGGER_GCP_CREDENTIALS }}'
          setup-terragrunt: true
          terragrunt-version: 0.44.1
        env:
          LOCK_PROVIDER: gcp
          GOOGLE_STORAGE_BUCKET: please-create-me-for-storing-locks
          GOOGLE_ENCRYPTION_KEY: ${{ secrets.GOOGLE_ENCRYPTION_KEY }}
          GITHUB_CONTEXT: ${{ toJson(github) }}
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

