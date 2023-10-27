## < Project Organization notes >
________________________________________________________________

# 2023/10/15 compthinking1

```
<- ...assignment operator     "Alt" + "-"

<- c() ...Vector, composed by series of values, which can be either number or characters

length() ... vectorの個数

```
________________________________________________________________

# 2023/10/16 working with data

```
For every new project,
  1. usethis::use_git()
  2. usethis::git_branch_rename()
  3. usethis::use_github()
on console to connect to GitHub

```
```
< Data transformations with dplyr >

library(tidyverse)

1. selecting columns and filtering rows

# keeping columns
select(surveys, plot_id, species_id, weight)
select(surveys, plot_id, species_id, weight_g = weight)






```