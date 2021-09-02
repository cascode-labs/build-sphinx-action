# ids-actions-build-conda
[![Test](https://github.com/cascode-labs/build-sphinx-action/actions/workflows/test.yml/badge.svg)](https://github.com/cascode-labs/build-sphinx-action/actions/workflows/test.yml)
![v0.1.0](https://img.shields.io/badge/v-0.1.0-blue)

A Github Action for building the [Sphinx](https://www.sphinx-doc.org/en/master/) 
documentation of an IDS project.

Features:
- if a "docs/setup_source.sh" script exists, it will be ran before building the docs.
- The project's conda package will be installed if it's already been uploaded as an artifact. 
- Supports both GitHub hosted and self-hosted runners.  

# Inputs
- **docs_path**: The path to the recipe from the repo root.  
  Optional, default: 'docs'
- **base_env_prefix**:  The prefix of the base Conda environment.  
  Optional, default: '/prj/ids/ids-conda/envs/anaconda'
- **artifact_name**:  The display name of the uploaded documentation artifact.  
  Optional, default: 'documentation'

# Outputs:
- **package-filepath**: The file path of the generated package.  It will return "None" if no package was created.

# Project Requirements
The Conda recipe's tests should copy their results, including any test or lint reports to the "test_results" folder.
Then the folder will be uploaded as an artifact.

The project also needs to have a build environment definition file at "envs/build.yml".  This should include all the 
packages required to build the package.