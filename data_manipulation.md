Data Manipulation
================

September 19 Lecture: This document will show how to manipulate data.

``` r
litters_df = 
  read.csv("data/FAS_litters.csv", na=c("NA", ".", ""))

litters_df = janitor::clean_names(litters_df)

pups_df = 
  read_csv("data/FAS_pups.csv", na=c("NA", ".", ""))
```

    ## Rows: 313 Columns: 6
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): Litter Number
    ## dbl (5): Sex, PD ears, PD eyes, PD pivot, PD walk
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
pups_df = janitor::clean_names(pups_df)
```

## ‘select’

Use ‘select()’ to select variables

``` r
select(litters_df, group, litter_number, gd0_weight)

select(litters_df, group:gd18_weight)

select(litters_df, -pups_survive)

select(litters_df, -(group:gd18_weight))

select(litters_df, starts_with("gd"))

select(litters_df, contains("pups"))

select(litters_df, GROUP=group)

rename(litters_df, GROUP=group)

select(litters_df, litter_number, gd0_weight, everything())

relocate(litters_df, litter_number, gd0_weight)
```

``` r
select(pups_df, litter_number, sex, pd_ears)
```

## `filter`

use ==, \>=, \<=, \>, \<, !=

``` r
filter(litters_df, gd_of_birth == 20)

filter(litters_df, pups_born_alive > 5)

filter(litters_df, group=="Low8")
filter(litters_df, group %in% c("Low8", "Low7"))
filter(litters_df, group %in% c("Low8", "Low7"), pups_born_alive == 8)
```

``` r
drop_na(litters_df) # drop any row that has any missing data

drop_na(litters_df, gd0_weight)
```

``` r
filter(pups_df, sex==1)

filter(pups_df, pd_walk < 11, sex ==2)
```

## `mutate`

``` r
mutate(litters_df, wt_gain = gd18_weight - gd0_weight)

mutate(litters_df, sq_pups = pups_born_alive^2)

mutate(litters_df, group = str_to_lower(group))

mutate(
  litters_df,
  wt_gain = gd18_weight - gd0_weight,
  group = str_to_lower(group)
)
```

## `arrange`

``` r
arrange(litters_df, gd0_weight)

arrange(litters_df, desc(gd0_weight))

arrange(litters_df, pups_born_alive, gd0_weight)
```

## `PIPING`

``` r
litters_df = 
  read.csv("data/FAS_litters.csv", na=c("NA", ".", ""))

litters_df = janitor::clean_names(litters_df)

litters_df_var = select(litters_df, -pups_born_alive)

litters_with_filter = filter(litters_df_var, group == "Con7")

litters_df = mutate(litters_df, wt_gain = gd18_weight - gd0_weight)


# or do this: 
# use command shift m to get %>% 

litters_df =
  read_csv("data/FAS_litters.csv", na=c("NA", ".", "")) %>% 
  janitor::clean_names() %>% 
  select(-pups_born_alive) %>% 
  filter(group == "Con7") %>% 
  mutate(
    wt_gain = gd18_weight - gd0_weight,
    group = str_to_lower(group)
    )
```

    ## Rows: 49 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (2): Group, Litter Number
    ## dbl (6): GD0 weight, GD18 weight, GD of Birth, Pups born alive, Pups dead @ ...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
