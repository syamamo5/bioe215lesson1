## < Project Organization notes >

________________________________________________________________

# R operaters

```
< Arithmetic Operators >

+     Addition
-	    Subtraction
* 	  Multiplication
/	    Division
^	    Exponent
%%  	Modulus (Remainder from division)
%/%	  Integer Division
```
```
< Comparison Operators >

==	  Equal	x == y	
!=	  Not equal	x != y	
>	    Greater than	x > y	
<	    Less than	x < y	
>=	  Greater than or equal to	x >= y	
<=	  Less than or equal to	x <= y
```
```
< Logical Operators >

&   	Element-wise Logical AND operator. It returns TRUE if both elements are TRUE
&&  	Logical AND operator - Returns TRUE if both statements are TRUE
|   	Elementwise- Logical OR operator. It returns TRUE if one of the statement is TRUE
||  	Logical OR operator. It returns TRUE if one of the statement is TRUE.
!	    Logical NOT - returns FALSE if statement is TRUE
```
```
< Miscellaneous Operators >

:	      Creates a series of numbers in a sequence  	x <- 1:10
%in%	  Find out if an element belongs to a vector 	x %in% y
%*%	    Matrix Multiplication	x <- Matrix1 %*% Matrix2

```
________________________________________________________________

# 2023/10/15 compthinking1

```
<- ...assignment operator     "Alt" + "-"


・Atomic vectors
<- c() ...composed by series of values, which can be either number or characters, but they all have to be the same types (character, double, integer, logical)


・list
# can be different types
# 貨物列車のようなもの、コンテナの中身は別々でも可



< Indexing >

x <- c(a = 1, b = 2, c = 5)

3 ways to index atomic vectors

1. position
x[1]
x[1:2]
x[c(1,3)]

2. by name
x["a"]
x[c("a", "c")]

3. by logic
filenames <- c("bob", "bob", "springer", "bailey", "bailey")
breed <- c("pittie", "pittie", "shepherd", "lab", "lab")
filenames == "bob"
filenames[filenames == "bob"]
breed[filenames == "bob"]



< list subsetting>

l <- list(a = 1:3,
          b = 4:6)

mean(l[1])

# subsetting lists with $ and [[]]  　"の"と同じ
l$a
l[["a"]]

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

_________________________________________________________________

1. selecting columns and filtering rows

# keeping columns

select(surveys, plot_id, species_id, weight)
select(surveys, plot_id, species_id, weight_g = weight)


# removing columns

select(surveys, -record_id, -species_id)


# keeping rows based on conditions

filter(surveys, year == 1995)
filter(surveys, year == 1995, plot_id == 7)
filter(surveys, month == 2 | day == 20)
```
```
_________________________________________________________________

2. Pipes %>%  (Shift + Ctrl + M)

surveys_psw <- surveys %>% 
  filter(year == 1995) %>% 
  select(plot_id, species_id, weight)


this is same as:

select(filter(surveys, year == 1995), plot_id, species_id, weight)

surveys_1995 <- filter(surveys, year == 1995)
surveys_psw <- select(surveys_1995, plot_id, species_id, weight)
```
```
_________________________________________________________________

3. Add/change columns with "mutate"

# Add a column

surveys %>% 
  mutate(weight_kg = weight / 1000)


# Add multiple columns based on each other

surveys %>% 
  mutate(weight_kg = weight / 1000,
         weight_lb = weight_kg * 2.2)


# So many NAs! Add a filter before mutate.

surveys %>% 
  filter(!is.na(weight)) %>% 
  mutate(weight_kg = weight / 1000,
         weight_lb = weight_kg * 2.2)


# Convert year, month, and day to dates

surveys %>% 
  select(year, month, day) %>% 
  mutate(date_str = paste(year, month, day, sep = "-"),
         date = as.Date(date_str))
```
```
_________________________________________________________________

4. Split-apply-combine with "summarize"

#What’s the average weight of the observed animals by sex?

surveys %>% 
  group_by(sex) %>% 
  summarize(mean_weight = mean(weight, na.rm = TRUE))


#Group by multiple columns, e.g. sex and species.

surveys %>% 
  group_by(species_id, sex) %>% 
  summarize(mean_weight = mean(weight, na.rm = TRUE))

### Note###___________________________________________________________

Notice the warning message: our output is still grouped by species_id.
By default, summarize() only removes one level of grouping.
This usually leads to unexpected results.
As the warning suggests, use .groups to drop all groups.

______________________________________________________________________

so,

surveys %>% 
  group_by(species_id, sex) %>% 
  summarize(mean_weight = mean(weight, na.rm = TRUE),
            .groups = "drop")


# NaN is the result of calling mean() on an empty vector. Let’s remove NAs before summarizing.

surveys %>% 
  filter(!is.na(weight)) %>% 
  group_by(species_id, sex) %>% 
  summarize(mean_weight = mean(weight),
            .groups = "drop")


# Can also generate multiple summaries per group.

surveys %>% 
  filter(!is.na(weight)) %>% 
  group_by(species_id, sex) %>% 
  summarize(mean_weight = mean(weight),
            min_weight = min(weight),
            .groups = "drop")
```
```
_________________________________________________________________

5. Sort with "arrange"

surveys %>% 
  filter(!is.na(weight)) %>% 
  group_by(species_id, sex) %>% 
  summarize(mean_weight = mean(weight),
            min_weight = min(weight),
            .groups = "drop") %>% 
  arrange(desc(mean_weight))
```
```
_________________________________________________________________

6. Joining data

Joining columns: left_join(), inner_join()

# Say we have more information about some of our taxa.

count(surveys, taxa)
taxa_iucn <- data.frame(
  taxa = c("Bird", "Rabbit", "Rodent"),
  iucn = c("NT", "LC", "LC")
)
taxa_iucn


# Left join surveys with taxa info by their shared column (the “key”)

surveys_iucn <- left_join(surveys, taxa_iucn, by = "taxa")
head(surveys_iucn)
```
```
_________________________________________________________________

Some Utility functions!

# count() is a shortcut to getting the size of groups

surveys %>% 
  count(sex, species) %>% 
  arrange(species, desc(n))


# drop_na() is a shortcut for removing rows with missing values

surveys %>% 
  drop_na(weight, sex) %>% 
  group_by(species_id, sex) %>% 
  summarize(mean_weight = mean(weight),
            min_weight = min(weight),
            .groups = "drop") %>% 
  arrange(desc(mean_weight))
```
________________________________________________________________

# 2023/10/23 compthinking2

Functions are “canned scripts” that automate more complicated sets of commands including operations assignments

```
mean <- function(x) {
  result <- sum(x) / length(x)
  return(result)
}


Function name = mean

Keyword function = function

Parameters = x

Body = Everything in the {}



first_last_chr <- function(s) {
  first_chr <- substr(s, 1, 1)
  last_chr <- substr(s, nchar(s), nchar(s))
  result <- paste(first_chr, last_chr, sep = "")
  return(result)
}
text <- "Amazing!"
first_last_chr(text)


Function name = first_last_chr

Keyword function = function

Parameters = s

Body = Everything in the {}
```
________________________________________________________________

# 2023/10/30 Troubleshooting

```
< Debugging >

# 1. replex

# minimum reproducible example, or a reprex for short. Distilling a bug into the smallest amount of data and code that produces the bug is the single most valuable skill for debugging!

  # Key to minimizing code
    1. Small and simple inputs
    2. No extraneous packages
    3. No unnecessary function calls
```
```
install.packages("reprex")
library(reprex)

#{}...code chunk

reprex({library(palmerpenguins)
  body_condition <- resid(lm(body_mass_g ~ flipper_length_mm, penguins))
  summary(body_condition)
})
```
```
# 2. browser function

use "blowser()" to stop in the middle of an execution. 
```

































