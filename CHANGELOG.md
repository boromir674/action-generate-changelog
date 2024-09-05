# Changelog

All notable changes to this project will be documented in this file.

This project adheres to [Semantic Versioning](https://semver.org/).

## 1.0.0 (2024-08-24)

### Changes

#### Feature
- use `raw Categories`, when `Markdown Headers` are created from comma-separated inputs.categories
- add 'composite' action with **Inputs** `commits`, `output_format`, and `categories`

#### Fix
- strip grouped commits from \<category\>: prefix pattern found in commit subject
- do not return Categories, that have no commits
- convert input.commits to UNICODE and store as env var
- use toJSON to process Inputs.commits JSON array data

#### Test
- change Test Data to expect commits stripped of \<category\>: prefix pattern
- add another JSON-output Test Case with minimal data
- test expected JSON array of Objects is returned when inputs.output_format == 'JSON'
- test order of commits is preserved
- render the Action Outputs.content in Job Summary

#### Docs
- document `Action inputs/outputs` and add example usage in **README.md**
- update `Action Logic Flowchart Diagram`, in **README.md**

#### CI
- allow Test Cases to continue running when another one fails
- print the Action Output Content in Job Summary, conditionally on format MD/JSON
- dynamically assert expected Markdown or JSON content
- setup CI/CD Pipeline and Parallel Tests, using Job Matrix


## 0.1.0

This is the **first** ever release of the **Action Generate Changelog** Open-Source Project.
- The project is hosted in a public repository on GitHub at [https://github.com/boromir674/action-generate-changelog](https://github.com/boromir674/action-generate-changelog)

### Initial Sources

- **CI/CD Pipeline** running on GitHub Actions at [https://github.com/boromir674/action-generate-changelog/actions](https://github.com/boromir674/action-generate-changelog)
