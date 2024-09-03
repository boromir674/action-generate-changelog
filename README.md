# Action Generate Changelog

> Generate `Release Changelog` from Commit Subjects.

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

### `format` (str)

### `categories` (str)

## Outputs

### `content` (str)

## Example

```yaml
```
