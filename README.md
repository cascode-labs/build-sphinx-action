[![Test](https://github.com/cascode-labs/build-sphinx-action/actions/workflows/test.yml/badge.svg)](https://github.com/cascode-labs/build-sphinx-action/actions/workflows/test.yml)
![v0.1.0](https://img.shields.io/badge/v-0.1.0-blue)

<br />
<p align="center">
  <h1 align="center">build-spinx-action</h1>
  <p align="center">
    A GitHub action that creates documentation using <a href="https://www.sphinx-doc.org/en/master/">Sphinx</a> 
    <br />
    <br />
    <a href="https://github.com/marketplace/actions/build-sphinx-action">Action Marketplace</a>
    ·
    <a href="https://github.com/cascode-labs/build-sphinx-action/issues">Report Bug</a>
    ·
    <a href="https://github.com/cascode-labs/build-sphinx-action/issues">Request Feature</a>
  </p>
</p>

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
- **conda_build_env_filepath**: Yaml Conda build environment definition file
  default: 'action_default' which uses the environment described in a section below.
- **base_env_prefix**:  The prefix of the base Conda environment.  
  THis should be set for self-hosted runners.  GitHub-hosted runners should use the conda-incubator/setup-miniconda@v2 
  action to initialize conda.
  Optional, default: '/usr/share/miniconda'
- **artifact_name**:  The display name of the uploaded documentation artifact.  
  Optional, default: 'documentation'
- **package_folder_path**: Path to the folder containing the project's Conda package(s) to be installed in the build 
  environment.  This is helpful if you are trying to auto-document those packages.
  default: 'conda_package'
- **package_name**: Name of the project's Conda package.
  default: ${{ github.event.repository.name }}  

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
## Roadmap

See the 
[open issues](https://github.com/cascode-labs/build-conda-action/issues)
for a list of proposed features (and known issues).
[Milestones](https://github.com/cascode-labs/build-conda-action/milestones)
outline the release version of each issue.

## Contributing

Contributions are what make the open source community such an amazing place to 
learn, inspire, and create. Any contributions you make are 
**greatly appreciated**.

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

### Testing
This repo contains a test workflow with each job of the workflow as a different
test case.

## Default Environment
A list of the packages included in the [conda environment](envs/build-docs.yml) used to build the sphinx project used by default. envs/build-docs.yml package descriptions:
  * [conda](https://docs.conda.io/en/latest/): required to create this documentation environment 
  * [sphinx](https://www.sphinx-doc.org/en/master/): documentation package used to create the docs
  * [recommonmark](https://recommonmark.readthedocs.io/en/latest/): additional package to support markdown language.
  * [sphinx_rtd_theme](https://sphinx-rtd-theme.readthedocs.io/en/stable/): recommended sphinx theme
  * [sphinx-panels](https://sphinx-panels.readthedocs.io/en/latest/): additional sphinx feature for panels
  * [sphinx-autobuild](https://pypi.org/project/sphinx-autobuild/): A package for automatically rebuilding a sphinx 
    project whenever a change is detected.
  * [sphinx-click](https://sphinx-click.readthedocs.io/en/latest/): additional sphinx feature that allows automatic 
    documentation of [click](https://click.palletsprojects.com/en/8.0.x/) CLI applications. 
  * [sphinx-copybutton](https://sphinx-copybutton.readthedocs.io/en/latest/): additional sphinx feature to allow 
    easy copying of code blocks.
  * [pip](https://pip.pypa.io/en/stable/): Python package installation tool, used to install the [pypi](https://pypi.org/) packages below.
    * [sphinx-tabs](https://sphinx-tabs.readthedocs.io/en/latest/): additional spinx feature for including tabs in your documentation
    