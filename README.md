<!--
SPDX-FileCopyrightText: Copyright contributors to the Software Quality Assurance as a Service (SQAaaS) project.
SPDX-FileContributor: Pablo Orviz <orviz@ifca.unican.es>

SPDX-License-Identifier: GPL-3.0-only
-->

[![SQAaaS badge shields.io](https://github.com/EOSC-synergy/sqaaas-assessment-action.assess.sqaaas/raw/release/2.0.0/.badge/status_shields.svg)](https://sqaaas.eosc-synergy.eu/#/full-assessment/report/https://raw.githubusercontent.com/eosc-synergy/sqaaas-assessment-action.assess.sqaaas/release/2.0.0/.report/assessment_output.json)
[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)
[![REUSE status](https://api.reuse.software/badge/git.fsfe.org/reuse/api)](https://api.reuse.software/info/git.fsfe.org/reuse/api)
[![Code style: black](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/psf/black)

# SQAaaS assessment action

This action triggers the quality assessment of a source code repository.

## Inputs

The inputs below are optional as by default this action will take the current repository and branch.

### `repo`

The URL of the repository to assess.

### `branch`

The branch to fetch from the previous repository name. If a branch is not provided, the SQAaaS platform takes the default one.

### `qc_uni_steps`

Space-separated list of step identifiers (defined with the [sqaaas-step-action](https://github.com/eosc-synergy/sqaaas-step-action))
that take part in the assessment of the unit testing quality criterion.

## Outputs

### `report`

JSON payload containing the full QA report.

## Example usage

In most cases, one would use the action in order to run the SQAaaS quality assessment for the repository and branch
that triggered the action. To this end, you just need to use the action as follows:

```yaml
uses: eosc-synergy/sqaaas-assessment-action@v2
```

However, if required, the action can be used to assess alternative combinations of repositories and branches. Here, you
would need to use the optional inputs `repo` and `branch`, such as in:

```yaml
uses: eosc-synergy/sqaaas-assessment-action@v2
with:
  repo: 'https://github.com/eosc-synergy/sqaaas-assessment-action'
  branch: 'main'
```

If you want to add a customised step made through the [sqaaas-step-action](https://github.com/eosc-synergy/sqaaas-step-action) use:

```yaml
uses: eosc-synergy/sqaaas-assessment-action@v2
with:
  qc_uni_steps: tox_unit_step
```

For this latter case the definiton for the customised step could be similar to (note that step's `name` has to match the value of `qc_uni_steps`):

```yaml
uses: eosc-synergy/sqaaas-step-action@v1
with:
  name: tox_unit_step
  tool: tox
  tox_env: run_unit
```

## Report summary

This action provides a summary of the SQAaaS assessment report, as well as the link to the complete version of it:
![GH action's summary report](./imgs/summary_report.png)


## Dynamic SQAaaS status badge (shields.io-like)

A status badge can be used when using this action that will be dynamically updated according to the progress of the quality
assessment performed by the SQAaaS platform.

The following markdown excerpt shall be modified by adding your repository name (represented as \<repository-name\>):

```markdown
[![SQAaaS badge shields.io](https://github.com/EOSC-synergy/<repository-name>.assess.sqaaas/raw/<branch-name>/.badge/status_shields.svg)](https://sqaaas.eosc-synergy.eu/#/full-assessment/report/https://raw.githubusercontent.com/eosc-synergy/<your-repository-name>.assess.sqaaas/<branch-name>/.report/assessment_output.json)
```

so e.g.:

```markdown
[![SQAaaS badge shields.io](https://github.com/EOSC-synergy/sqaaas-assessment-action.assess.sqaaas/raw/main/.badge/status_shields.svg)](https://sqaaas.eosc-synergy.eu/#/full-assessment/report/https://raw.githubusercontent.com/eosc-synergy/sqaaas-assessment-action.assess.sqaaas/main/.report/assessment_output.json)
```

**Note**: as the SQAaaS status badges are stored in GitHub repositories, they are not immediately refreshed upon changes in the progress of the quality assessment (GitHub currently sets a `Cache-Control` of 300 seconds).
