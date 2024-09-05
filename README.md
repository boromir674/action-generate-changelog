# Action Generate Changelog

> Generate `Release Changelog` from Commit Subjects.

[![GitHub branch status](https://img.shields.io/github/checks-status/boromir674/action-generate-changelog/main?label=CI%2FCD)](https://github.com/boromir674/action-generate-changelog/actions/workflows/cicd.yml)
![GitHub Tag](https://img.shields.io/github/v/tag/boromir674/action-generate-changelog?sort=semver)
![GitHub Release](https://img.shields.io/github/v/release/boromir674/action-generate-changelog?sort=semver&color=blue&link=https%3A%2F%2Fgithub.com%2Fboromir674%2Faction-generate-changelog%2Freleases%2Ftag%2Fv0.1.0)
[![License](https://img.shields.io/github/license/boromir674/action-generate-changelog)](https://github.com/boromir674/action-generate-changelog/blob/main/LICENSE)
[![GitHub commits since tagged version (branch)](https://img.shields.io/github/commits-since/boromir674/action-generate-changelog/v0.1.0/main?color=blue&logo=github)](https://github.com/boromir674/action-generate-changelog/compare/v0.1.0..main)
![GitHub commits since latest release (by SemVer)](https://img.shields.io/github/commits-since/boromir674/action-generate-changelog/latest?color=blue&logo=semver&sort=semver)



```mermaid
%%{init: {"flowchart": {"htmlLabels": false}} }%%
flowchart LR

%% CONDITIONS
%%% NOTE: so far it seems that it is a redundant use case to support group=false
%% if_group{"`if **group** = 'true'`"}
%% if_format_is_md{"`if **format** = 'MD'`"}

if_format_is_md_2{"`if **format** = 'MD'`"}
if_categories_input_is_supplied{"`**categories** given
in action input?`"}
if_user_categories_files_found{"`**.github/categories.md**
file exists?`"}

%% FINAL NODES
%% gen_output_same_as_input(("`**OUTPUT:**
%% **JSON** Array of strings
%% same as input`"))
%% gen_output_md_list_from_input_json_array(("`**OUTPUT:**
%% **MD list** of commits`"))

gen_output_as_json_array(("`**OUTPUT:**
**JSON** Array of Objects`"))
gen_output_md_sections(("`**OUTPUT:**
**MD Sections** each with
a **list** of commits`"))

%% OTHER NODES
use_categories_input("`Use **Categories**
from action inputs`")
use_categories_from_user_file("`Use **Categories**
from user file`")
use_categories_from_built_in_default("`Use **Built-in Default**
**Categories**`")
group_commits_using_categories("`**Group Commits**
using Categories`")


%% FLOW / LOGIC GRAPH

%%% group flag is FALSE
%%% NOTE: so far it seems that it is a redundant use case to support group=false
%% if_group -- NO --> if_format_is_md
%% if_format_is_md -- NO --> gen_output_same_as_input
%% if_format_is_md -- YES --> gen_output_md_list_from_input_json_array

%%% group flag is TRUE
%% (see NOTE) if_group -- YES --> if_categories_input_is_supplied

if_categories_input_is_supplied -- YES --> use_categories_input
use_categories_input --> group_commits_using_categories

if_categories_input_is_supplied -- NO --> if_user_categories_files_found
if_user_categories_files_found -- YES --> use_categories_from_user_file

use_categories_from_user_file --> group_commits_using_categories

if_user_categories_files_found -- NO --> use_categories_from_built_in_default
use_categories_from_built_in_default --> group_commits_using_categories

group_commits_using_categories --> if_format_is_md_2
if_format_is_md_2 -- NO --> gen_output_as_json_array
if_format_is_md_2 -- YES --> gen_output_md_sections

```

## Features

## Inputs

### `commits` (str)
> Commit Subjects as JSON array of strings.

Example **value**:
```json
[
    "feat: add feature X2",
    "test: add Test Case for feature X2",
    "fix: fix feature X1",
    "feat: add feature X1"
]
```

### `format` (str)
> The format, 'MD' (for Markdown) or 'JSON', to use for the Action Output

Example **value**: `MD` or `JSON`

### `categories` (str)
> Comma-separated Categories to use for grouping the Commits.

Example **value**:
```
feat,fix,refactor,test,build,docs,ci,style,chore
```
## Outputs

### `content` (str)
> Either **Markdown** Sections, each with a list, or **JSON** array of Objects

#### Example **Markdown value**
```markdown
## feat
- add feature `X2`
- add feature `X1`

## fix
- handle corner case for input X in function Y

## test
- test feature X returns expected data

## docs
- add Documentation Sources for `mkdocs` build

## ci
- add CI/CD Pipeline

```

#### Example **JSON value**
```json
[
  {
    "category": "feat",
    "commits": [
      "add feature X2",
      "add feature X1"
    ]
  },
  {
    "category": "fix",
    "commits": [
      "handle corner case for input X in function Y"
    ]
  },
  {
    "category": "test",
    "commits": [
      "test feature X returns expected data"
    ]
  },
  {
    "category": "ci",
    "commits": [
      "add CI/CD Pipeline"
    ]
  }
]
```

## Examples

> Retrieve Value with `${{ steps.groups.commits.output.content }}`, in your next Job steps.

## With static input
```yaml
jobs:
  my_jobs:
  runs-on: ubuntu-latest
  steps:
    - name: Group Commits under Markdown Sections
      id: group_commits
      uses: boromir674/action-generate-changelog@v1.0.0
      with:
          commits: |
            '[
                "feat: add feature X2",
                "test: test feature X2 returns expected data",
                "fix: handle corner case for X1 function input",
                "feat: add feature X1",
                "ci: add CI/CD Pipeline"
            ]'
          output_format: 'MD'
          categories: 'feat,fix,refactor,test,build,docs,ci,style,chore'
```
Returns `Markdown Content` as **multi-line Unicode** string

```markdown
## feat
- add feature X2
- add feature X1

## fix
- handle corner case for X1 function inpu

## test
- test feature X2 returns expected data

## ci
- add CI/CD Pipeline

```
