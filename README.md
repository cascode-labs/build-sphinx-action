# ids-actions-build-conda
[![Test](https://github.com/cascode-labs/build-sphinx-action/actions/workflows/test.yml/badge.svg)](https://github.com/cascode-labs/build-sphinx-action/actions/workflows/test.yml)
![v0.1.0](https://img.shields.io/badge/v-0.1.0-blue)

A Github Action for building the [Sphinx](https://www.sphinx-doc.org/en/master/) 
documentation of an IDS project.

Features:
- if a "docs/setup_source.sh" script exists, it will be ran before building the docs.
- The project's conda package will be installed if it's already been uploaded as an artifact. 
- Supports both GitHub hosted and self-hosted runners.  
## Getting Started
> All inputs are optional. Just follow the project configuration guidelines below.

### Standard Documentation Configuration
- Place environment yml file for documention in envs/build-docs.yml.
- Put any and all documentation files in the docs/source directory. _except readme file_

### Basic Usage
```yaml
job-name:
  runs-on: 'ubuntu-latest'
  steps:
    - uses: actions/checkout@v2
    # Builds the documentation package using the standard configuration
    - uses: cascode-labs/build-sphinx-action/action.yml@v0
```

## Custom Configuration
### Inputs
- **docs_path**: The path to the recipe from the repo root.  
  Optional, default: 'docs'
- **base_env_prefix**:  The prefix of the base Conda environment.  
  Optional, default: '/prj/ids/ids-conda/envs/anaconda'
- **artifact_name**:  The display name of the uploaded documentation artifact.  
  Optional, default: 'documentation'
- **readme_path**: The path to the README file 
  Optional, default: 'README.md'

### Outputs:
- **package-filepath**: The file path of the generated package.  It will return "None" if no package was created.

### Artifacts Uploaded

The following are uploaded to the run as artifacts.

- **documentation**: The html file for the newly created docs.


### Example customized workflow
```yaml
job-name:
  runs-on: 'ubuntu-latest'
  steps:
    - uses: actions/checkout@v2
    # Initializes Conda on a GitHub hosted Runner
    - uses: conda-incubator/setup-miniconda@v2
      with:
        python-version: 3.7
    # Runs the action with the following inputs or defaults if not specified.
    - uses: cascode-labs/build-sphinx-action/action.yml@v0
      with:
        DOCS_PATH: '{NEW_DOCS_PATH}'
        CONDA_BUILD_ENV_FILEPATH: '{NEW_CONDA_BUILD_ENV_FILEPATH}'
        BASE_ENV_PREFIX: '{NEW_BASE_ENV_PREFIX}'
        ARTIFACT_NAME: '{NEW_ARTIFACT_NAME}'
        README_PATH: '{NEW_README_PATH}'
        PACKAGE_FOLDER_PATH: '{NEW_PACKAGE_FOLDER_PATH}'
        PACKAGE_NAME: '{NEW_PACKAGE_NAME}'
```
# Project Requirements
The Conda recipe's tests should copy their results, including any test or lint reports to the "test_results" folder.
Then the folder will be uploaded as an artifact.

The project also needs to have a build environment definition file at "envs/build.yml".  This should include all the 
packages required to build the package.
