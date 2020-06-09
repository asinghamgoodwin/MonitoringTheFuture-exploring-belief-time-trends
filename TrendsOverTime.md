MTF - exploring trends in beliefs over time
================

# Interesting questions

The *Monitoring The Future* survey has a lot of really interesting (and
sometimes weird) questions that have been asked year after year, for
decades. Some of the themes I’m curious about include:

**Attitudes about gender roles**

  - Item Number: 07930, A: Men and women should be paid the same money
    if they do the same work
  - Item Number: 07940, B: Women should be considered as seriously as
    men for jobs as executives or politicians
  - Item Number: 07950, C: A woman should have exactly the same job
    opportunities as a man
  - Item Number: 07960, D: A woman should have exactly the same
    educational opportunities as a man
  - Item Number: 07970, E: It is usually better for everyone involved if
    the man is the achiever outside the home and \* the woman takes care
    of the home and family
  - Item Number: 10470, A: One sees so few good or happy marriages that
    one questions it as a way of life
  - Item Number: 10480, B: It is usually a good idea for a couple to
    live together before getting married in order to find out whether
    they really get along
  - Item Number: 10490, C: Having a close intimate relationship with
    only one partner is too restrictive for the average person
  - Item Number: 10520, D: Being a father and raising children is one of
    the most fulfilling experiences a man can have
  - Item Number: 12170, E: Being a mother and raising children is one of
    the most fulfilling experiences a woman can have
  - Item Number: 10530, F: Most mothers should spend more time with
    their children than they do now
  - Item Number: 12180, G: Most fathers should spend more time with
    their children than they do now

**Attitudes about gendered discrimination**

  - Item Numbers: 12290-12350: …Whether you think women are
    discriminated against in each of the following areas…
  - Item Number: 10370, To what extent do you think the things listed
    below will prevent you from getting the kind of work you would like
    to have? B: Your sex

**Beliefs about social justice and institutions**

  - Item Number: 01610, D: The way people vote has a major impact on how
    things are run in this country
  - Item Number: 01620, E: People who get together in citizen action
    groups to influence government policies can have a real effect
  - Item Number: 01520, L: \[How important is it in your life\] Working
    to correct social and economic inequalities
  - Item Numers: 08380, 08390, 08430, 08440: How good or bad a job you
    feel each of the following organizations is doing for the country as
    a whole: Large corporations? Major labor unions? The national news
    media? The President and his administration?
  - Item Number: 12110, E: I get very upset when I see other people
    treated unfairly

Even though similar or the same questions get asked each year, they have
different variable names. I created mapping tables for the variables I’m
interested in, so that it’s easy to look up “what was X question’s
variable name in year Y?”

Some things I’m learning: \* `dplyr` would rather you not use rownames,
and instead convert into a regular column of names-as-characters \*
`mutate_at()` lets you mutate everything usng a function, and specifying
`vars(-$COLNAME)` applies that function to everything *except* the
specified column (without the `-`, it only mutates the specified
columns) \* it’s important to treat everything as characters rather than
factors *\<– I wish I understood this more\!* \* `pull()` takes a column
from a tibble and returns a
vector

``` r
twelve_core_mapping = read.csv(file = "MappingTables/12core1991-2018.csv",
                               check.names = FALSE,
                               colClasses = c("character")
                               ) %>% 
  mutate_at(.,
            vars(-variable),
            .funs = toupper
            ) %>% 
  rename(.,
         new_variable_name = variable
         )

twelve_form3_mapping = read.csv(file = "MappingTables/12form3_1991-2018.csv", check.names = FALSE)
twelve_form6_mapping = read.csv(file = "MappingTables/12form6_1991-2018.csv", check.names = FALSE)
twelve_form7_mapping = read.csv(file = "MappingTables/12form7_1991-2018.csv", check.names = FALSE)

knitr::kable(head(twelve_core_mapping))
```

| new\_variable\_name | 1991 | 1992 | 1993 | 1994 | 1995 | 1996 | 1997 | 1998 | 1999 | 2000 | 2001 | 2002 | 2003 | 2004 | 2005 | 2006 | 2007 | 2008 | 2009 | 2010 | 2011 | 2012           | 2013        | 2014           | 2015           | 2016           | 2017           | 2018           |
| :------------------ | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :------------- | :---------- | :------------- | :------------- | :------------- | :------------- | :------------- |
| id                  | V4   | V4   | V4   | V4   | V4   | V4   | V4   | V4   | V4   | V4   | V4   | V4   | V4   | V4   | V4   | V4   | V4   | V4   | V4   | V4   | V4   | RESPONDENT\_ID | V6          | RESPONDENT\_ID | RESPONDENT\_ID | RESPONDENT\_ID | RESPONDENT\_ID | RESPONDENT\_ID |
| form\_id            | V3   | V3   | V3   | V3   | V3   | V3   | V3   | V3   | V3   | V3   | V3   | V3   | V3   | V3   | V3   | V3   | V3   | V3   | V3   | V3   | V3   | V3             | V3          | V3             | V3             | V3             | V3             | V3             |
| weight              | V5   | V5   | V5   | V5   | V5   | V5   | V5   | V5   | V5   | V5   | V5   | V5   | V5   | V5   | V5   | V5   | V5   | V5   | V5   | V5   | V5   | ARCHIVE\_WT    | ARCHIVE\_WT | ARCHIVE\_WT    | ARCHIVE\_WT    | ARCHIVE\_WT    | ARCHIVE\_WT    | ARCHIVE\_WT    |
| sex                 | V150 | V150 | V150 | V150 | V150 | V150 | V150 | V150 | V150 | V150 | V150 | V150 | V150 | V150 | V150 | V150 | V150 | V150 | V150 | V150 | V150 | V2150          | V2150       | V2150          | V2150          | V2150          | V2150          | V2150          |
| race2               | V151 | V151 | V151 | V151 | V151 | V151 | V151 | V151 | V151 | V151 | V151 | V151 | V151 | V151 | .    | .    | .    | .    | .    | .    | .    | .              | .           | .              | .              | .              | .              | .              |
| race3               | .    | .    | .    | .    | .    | .    | .    | .    | .    | .    | .    | .    | .    | .    | V151 | V151 | V151 | V151 | V151 | V151 | V151 | V2151          | V2151       | V2151          | V2151          | V2151          | V2151          | V2151          |

This function takes a variable name from an MTF SAS file, and returns a
standardized (by me), more descriptive
name.

``` r
col_rename = function(old_name = old_name, year = year, mapping = mapping) {
  row_num = which(mapping[year] == old_name)
  
  mapping[row_num, 1]
}
```

``` r
require(haven) # lets us read SAS files
```

    ## Loading required package: haven

``` r
require(stringr) # lets us subset a string using negative numbers

# Choices I made here, that could be different:
# * use every variable from the mapping tibble
# * so far, not validating input.
# * I convert year to a character, and always use it as a character. This is helpful since it's a column name, but maybe not intuitve when reading the code (you expect year to be a number)

extract_demographics = function(path = path, years = years, mapping = mapping) {
  if (str_sub(path, -1) != "/") {
    path = str_c(path, "/")
  }
  
  # start with a blank tibble that we'll fill up by binding each year's resulting tibble
  demographics_all_years = tibble()
  
  for (year in years) {
    year = as.character(year)
    
    file_name = str_c(path, "y", year, "_1.sas7bdat")
    #this_year_old_names = mapping[year]
      
    # variables to include in select() and rename() statements
    included_variable_old_names = pull(filter(mapping[year], !!sym(year) != "."))
    included_variable_new_names = map_chr(.x = included_variable_old_names,
                                          .f = col_rename,
                                          year = year,
                                          mapping = mapping
                                      )

    # variables that didn't exist for this given year, so we'll add them in as all NA using mutate()
    # QUESTION: is there a less-code way to do this? maybe somehow getting a list of all variables names, and then splitting on whether or not they're "."?
    excluded_variable_old_names = pull(filter(mapping[year], !!sym(year) == "."))
    excluded_variable_new_names = map_chr(.x = excluded_variable_old_names,
                                          .f = col_rename,
                                          year = year,
                                          mapping = mapping
                                      )

    cat(paste(included_variable_new_names), "\n")
    cat(paste(included_variable_old_names), "\n")
    cat(paste(excluded_variable_new_names), "\n")
    
    # form_id doesn't work -- why not?? look up v3 in codebook 2018
    # question: should year and grade be integers? factors? characters?
    this_year_data = read_sas(data_file = file_name) %>% 
      select(.,
             one_of(included_variable_old_names)
             ) %>%
      #rename_with(., tolower, .cols=("V3")) %>% 
      # rename_all(.,
      #            .funs = function(old_name) col_rename(old_name = old_name,
      #                                                  year = year,
      #                                                  mapping = mapping)
      #            ) %>%
      mutate(.,
             #year = year,
             grade = 12,
             )
      # select(., 
      #        gpa = as.character(mapping[["gpa", as.character(year)]])
      #        )
      # 
      #       # sex = as.character(mapping[["sex", as.character(year)]]),
             # race2 = as.character(mapping[["race2", as.character(year)]]),
             # race3 = as.character(mapping[["race3", as.character(year)]]),
             # dad_edu = as.character(mapping[["dad_edu", as.character(year)]]),
             # mom_edu = as.character(mapping[["mom_edu", as.character(year)]]),
             # gpa = as.character(mapping[["gpa", as.character(year)]]),
             # weight = as.character(mapping[["weight", as.character(year)]])
             #)
    # %>% 
    #   mutate(.,
    #          a,
    #          b,
    #          c
    #          )

    # need to sort alphabetically before binding rows (or find a better way to combine data frames vertically, by comlumn name)
    this_year_data = this_year_data %>% select(sort(tidyselect::peek_vars()))
    
    # will I have trouble with factors?
    demographics_all_years = bind_rows(demographics_all_years, this_year_data)
  }
  
  demographics_all_years
}

t1 = extract_demographics(path = "~/Documents/Code/MTF/MTFData/12th_grade/",
                          years = 2018:2018,
                          mapping = twelve_core_mapping)
```

    ## id form_id weight sex race3 dad_edu mom_edu gpa 
    ## RESPONDENT_ID V3 ARCHIVE_WT V2150 V2151 V2163 V2164 V2179 
    ## race2

``` r
knitr::kable(head(t1))
```

| ARCHIVE\_WT | grade | RESPONDENT\_ID | V2150 | V2151 | V2163 | V2164 | V2179 | V3 |
| ----------: | ----: | -------------: | ----: | ----: | ----: | ----: | ----: | -: |
|    1.608167 |    12 |          10001 |     1 |     2 |     6 |     5 |     9 |  1 |
|    1.357766 |    12 |          10002 |     1 |     2 |     6 |     5 |     9 |  1 |
|    1.546913 |    12 |          10003 |     1 |     2 |     6 |     5 |     7 |  1 |
|    1.542995 |    12 |          10004 |     2 |   \-9 |     6 |     5 |     9 |  1 |
|    1.451582 |    12 |          10005 |     1 |     2 |     6 |     6 |     8 |  1 |
|    1.451481 |    12 |          10006 |     2 |     2 |     6 |     5 |     9 |  1 |

*not sure if I’ll use this, but don’t delete yet*

``` r
identifiers = c() # id, grade, year
demographics = c() # sex, region, SES (as parents' edu status)
questions = c() # list of questions
health_outcomes = c() # mental health? internalizing/externalizing behaviors?
```
