# github-actions-library

### This is to automate the CI operations for the code repos based on their build type. As of now this repo performs operations `build, unittest, static code analysis and artifact store`.

### It requires two files, one is config.yaml(Its default name, if the name is different then it can be passed as parameter) and one github workflow file.

# Cofiguration yaml file
### this file needs to be present at the root of the repo. Example:
```
build_type: node
static_code_analysis: sonar

branches: 
  - type: feature
    build: true
    unit_tests: true
    code_analysis: false
    store_artifact: true

  - type: develop
    build: true
    unit_tests: true
    code_analysis: true
    store_artifact: true
```
- build_type: Tells how to build the code. Currently only `node` build type is supported.
- static_code_analysis: Tells static code analysis tool to be used. Either `lint` or `sonar`.
- branches:
    - type: Type of the git branch. Supports `feature, develop, release and prod`.
    - build: true if build action needs to be performed. If this is not present then it's false.
    - unit_tests: true if unittest execution is required. If this is not present then it's false.
    - code_analysis: true if static code analysis is required. If this is not present then it's false.
    - store_artifact: true if artifact needs to be stored. If this is not present then it's false.


# Github workflow file
### File needs to be present under `.github/workflows` folder. File name for ex. node_action.yaml.
```
name: Github Actions
on: 
  push:
  workflow_dispatch:
jobs:
  Node-Express-App:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      - name: Execute CI Operations
        uses: haisrig/github-actions-library@main
```
- No changes required in the workflow file. 
- As per the above configuration the build can be triggered by a code push or manually from Actions tab in the Github UI.
- If the build triggers need to change then they can be specified under `on`.

