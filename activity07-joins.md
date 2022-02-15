Activity 7
================
Nathan Vandermeer

## Data and packages

Load the entirety of the `{tidyverse}`. Be sure to avoid printing out
any unnecessary information and give the code chunk a meaningful name.

``` r
library(tidyverse)
## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──
## ✓ ggplot2 3.3.4     ✓ purrr   0.3.4
## ✓ tibble  3.1.2     ✓ dplyr   1.0.7
## ✓ tidyr   1.1.3     ✓ stringr 1.4.0
## ✓ readr   1.4.0     ✓ forcats 0.5.1
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

In this activity we will explore joining information that is contained
in multiple data files. We will also explore ways of visualizing spatial
data.

The late comedian [Mitch
Hedberg](https://en.wikipedia.org/wiki/Mitch_Hedberg) believed that La
Quinta (pronounced la KEEN-ta) was Spanish for “next to Denny’s”.

![Mitch
Hedberg](https://duckduckgrayduck.files.wordpress.com/2014/01/mitch-hedberg.jpg)

In the `data` folder, you have been provided with three `.csv` files.
The `dennys.csv` and `laquinta.csv` files contains addresses for the
locations of each respective company. The `states.csv` file contains the
area (thousand square miles) for each US state and the District of
Columbia. These data were scrapped from
[Denny’s](https://locations.dennys.com/) and [La
Quinta’s](https://www.wyndhamhotels.com/laquinta/locations) location web
pages by Mine Çetinkaya-Rundel.

Read each of the three files using `here::here` in combination with the
appropriate `{readr}` function. Assign each file to a meaningful object
name (e.g., `dennys`,`laquinta`, and `states`), be sure to avoid
printing any unnecessary information. Give your code chunk a meaningful
name.

``` r
dennys <- read.csv(here::here("data",'dennys.csv'))
laquinta <- read.csv(here::here("data",'laquinta.csv'))
states <- read.csv(here::here("data",'states.csv'))
```

### Provide more information

`class`, `str`, `nrow`, `ncol`, and `names` are extremely helpful
functions for quickly getting information about a dataset. Below is a
brief summary of the information that each function provides.

-   `class`: Returns the attribute of the R object
-   `str`: A compact display of the internal structure of an R object.
    This can also be viewed by clicking on the blue circle icon next to
    the R object in the **Environment** pane (upper-right pane).
-   `nrow`: Returns the number of rows in an R object
-   `ncol`: Returns the number of columns in an R object
-   `names`: Get or set the names of an R object (e.g., the column names
    for a dataset)

Use the output from `str` of these datasets to complete the `Type`
column in the partially started data dictionary tables below.

``` r
str(dennys)
```

    ## 'data.frame':    1643 obs. of  6 variables:
    ##  $ address  : chr  "2900 Denali" "3850 Debarr Road" "1929 Airport Way" "230 Connector Dr" ...
    ##  $ city     : chr  "Anchorage" "Anchorage" "Fairbanks" "Auburn" ...
    ##  $ state    : chr  "AK" "AK" "AK" "AL" ...
    ##  $ zip      : int  99503 99508 99701 36849 35207 35294 35056 36301 36043 35816 ...
    ##  $ longitude: num  -149.9 -149.8 -147.8 -85.5 -86.8 ...
    ##  $ latitude : num  61.2 61.2 64.8 32.6 33.6 ...

``` r
str(laquinta)
```

    ## 'data.frame':    909 obs. of  6 variables:
    ##  $ address  : chr  "793 W. Bel Air Avenue" "3018 CatClaw Dr" "3501 West Lake Rd" "184 North Point Way" ...
    ##  $ city     : chr  "\nAberdeen" "\nAbilene" "\nAbilene" "\nAcworth" ...
    ##  $ state    : chr  "MD" "TX" "TX" "GA" ...
    ##  $ zip      : chr  "21001" "79606" "79601" "30102" ...
    ##  $ longitude: num  -76.2 -99.8 -99.7 -84.7 -96.6 ...
    ##  $ latitude : num  39.5 32.4 32.5 34.1 34.8 ...

``` r
str(states)
```

    ## 'data.frame':    51 obs. of  3 variables:
    ##  $ name        : chr  "Alabama" "Alaska" "Arizona" "Arkansas" ...
    ##  $ abbreviation: chr  "AL" "AK" "AZ" "AR" ...
    ##  $ area        : num  52420 665384 113990 53179 163695 ...

#### Denny’s data

| Variable    | Type | Brief description                 |
|-------------|------|-----------------------------------|
| `address`   | chr  | street address of dennys location |
| `city`      | chr  | city of dennys location           |
| `state`     | chr  | state of dennys location          |
| `zip`       | int  | zip code of dennys location       |
| `longitude` | num  | east-west position on Earth       |
| `latitude`  | num  | north-south position on Earth     |

#### La Quinta’s data

| Variable    | Type | Brief description                   |
|-------------|------|-------------------------------------|
| `address`   | chr  | street address of laquinta location |
| `city`      | chr  | city of laquinta location           |
| `state`     | chr  | state of laquinta location          |
| `zip`       | chr  | zip code of laquinta location       |
| `longitude` | num  | east-west position on Earth         |
| `latitude`  | num  | north-south position on Earth       |

#### States data

| Variable       | Type | Brief description             |
|----------------|------|-------------------------------|
| `name`         | chr  | state name                    |
| `abbreviation` | chr  | state abbreviation            |
| `area`         | num  | area in thousand square miles |

![](README-img/noun_pause.png) **Planned Pause Point**: If you have any
questions, contact your instructor. Otherwise feel free to continue on.

## Analysis

### Locations in the US

We will limit our analysis to Denny’s and La Quinta’s locations within
the United States Look at the websites that the data come from (linked
above). Are there any La Quinta’s locations outside of the US? What
about Denny’s?

**Response**: There are 59 distinct states for laquinta, so there are
locations outside the US, but there are only 51 for dennys, so there are
no locations outside the US.

``` r
distinct(select(laquinta,state))
```

    ##    state
    ## 1     MD
    ## 2     TX
    ## 3     GA
    ## 4     OK
    ## 5     AG
    ## 6     NM
    ## 7     LA
    ## 8     CA
    ## 9     AK
    ## 10    MA
    ## 11    WI
    ## 12    NY
    ## 13    WA
    ## 14    MT
    ## 15    OR
    ## 16    AR
    ## 17    MS
    ## 18    AL
    ## 19    ND
    ## 20    MO
    ## 21    ID
    ## 22    FL
    ## 23    NC
    ## 24    KY
    ## 25    QR
    ## 26    OH
    ## 27    WY
    ## 28    CO
    ## 29    CH
    ## 30    UT
    ## 31    IA
    ## 32    PA
    ## 33    IL
    ## 34    SC
    ## 35    VA
    ## 36    TN
    ## 37    NJ
    ## 38    IN
    ## 39    RI
    ## 40    CT
    ## 41    MI
    ## 42    KS
    ## 43    MN
    ## 44    WV
    ## 45    NV
    ## 46    AZ
    ## 47    NE
    ## 48    NL
    ## 49    NH
    ## 50   ANT
    ## 51    ON
    ## 52    ME
    ## 53    VE
    ## 54    PU
    ## 55    SD
    ## 56    SL
    ## 57    VT
    ## 58    FM
    ## 59    BC

``` r
distinct(select(dennys,state))
```

    ##    state
    ## 1     AK
    ## 2     AL
    ## 3     AR
    ## 4     AZ
    ## 5     CA
    ## 6     CO
    ## 7     CT
    ## 8     DC
    ## 9     DE
    ## 10    FL
    ## 11    GA
    ## 12    HI
    ## 13    IA
    ## 14    ID
    ## 15    IL
    ## 16    IN
    ## 17    KS
    ## 18    KY
    ## 19    LA
    ## 20    MA
    ## 21    MD
    ## 22    ME
    ## 23    MI
    ## 24    MN
    ## 25    MO
    ## 26    MS
    ## 27    MT
    ## 28    NC
    ## 29    ND
    ## 30    NE
    ## 31    NH
    ## 32    NJ
    ## 33    NM
    ## 34    NV
    ## 35    NY
    ## 36    OH
    ## 37    OK
    ## 38    OR
    ## 39    PA
    ## 40    RI
    ## 41    SC
    ## 42    SD
    ## 43    TN
    ## 44    TX
    ## 45    UT
    ## 46    VA
    ## 47    VT
    ## 48    WA
    ## 49    WI
    ## 50    WV
    ## 51    WY

If we wanted to do this using the datasets, would we need to ? Don’t
worry about implementing this yet, you only need to brainstorm some
ideas. Include at least one idea as your answer, but you are welcome to
write down a few options too.

**Response**: I did this by looking at distinct values of state

### Preparing to Join

#### Denny’s

Now we will find the Denny’s locations that are outside the US using
code. To do so, *filter* the Denny’s locations for observations where
state is *not in* `states$abbreviation`. Do not assign this to anything;
we only want to see if we need to be aware of non-US cases. If there are
any non-US locations, specify where these are.

``` r
dennys %>% filter(!(state %in% states$abbreviation))
```

    ## [1] address   city      state     zip       longitude latitude 
    ## <0 rows> (or 0-length row.names)

**Response**: The dataset is empty so every dennys is in a US State

Now do this again, but using `anti_join`. To do so, take the Denny’s
locations and anti-join this with the states dataset. Remember to
specify your `by` columns.

``` r
dennys %>% anti_join(states, by=c('state'="abbreviation"))
```

    ## [1] address   city      state     zip       longitude latitude 
    ## <0 rows> (or 0-length row.names)

#### A Brief Aside

Another way to do this would be to create a new variable (called, says,
`country`), then filter on this new variable. Remember that `mutate`
creates new variables. Then `dplyr::case_when` function is a nice way to
do multiple if-else statements. For example, your instructor could see
if there are any Denny’s in any the states that your instructor has
lived in:

``` r
#dennys %>%
#  mutate(bradford_lived = case_when(
#    state %in% c("MI", "NC", "IL", "ME") ~ "Yes",
#    TRUE ~ "No"))
```

`case_when` looks to see if any of the `dennys$state`s are in the vector
of state abbreviations that I have lived (i.e.,
`c("MI", "NC", "IL", "ME")`). If this is a `TRUE` statement, then
`bradford_lived` will be set to `"Yes"`. Otherwise, if the values is not
a missing value (i.e., `NA`), the `TRUE ~ "NO"` line sets
`bradford_lived` to `"No`. If the values are missing, they remain
missing in `bradford_lived`. Without the `TRUE ~ "No"` line, all values
except for “MI”, “NC”, “IL”, and “ME” would be set to missing values.

Note that we could have also used
`!(state %in% c("MI", "NC", "IL", "ME")) ~ "NO"`. However, this would be
slightly inefficient to type as `TRUE ~ "No"` captures this.

To create a new variable called `country` where if the state is in the
US, `country` is set to `"United States"` we would then do:

``` r
#dennys %>% 
#  mutate(country = case_when(
#    state %in% states$abbreviation ~ "United States",
#    TRUE ~ "Other")) %>% 
#  filter(country != "Other")
```

![](README-img/noun_pause.png) **Planned Pause Point**: If you have any
questions, contact your instructor. Otherwise feel free to continue on.

#### La Quinta

Determine if La Quinta has any locations that are outside of the US.

**Response**: There are multiple la quinta locations outside the US

``` r
laquinta %>% anti_join(states, by=c('state'="abbreviation"))
```

    ##                                                                    address
    ## 1                                         Carretera Panamericana Sur KM 12
    ## 2                                         Av. Tulum Mza. 14 S.M. 4 Lote 2 
    ## 3                                                   Ejercito Nacional 8211
    ## 4                                                    Blvd. Aeropuerto 4001
    ## 5  Carrera 38 # 26-13 Avenida las Palmas con Loma de San Julian El Poblado
    ## 6                                                 AV. PINO SUAREZ No. 1001
    ## 7                                   Av. Fidel Velazquez #3000 Col. Central
    ## 8                                                      63 King Street East
    ## 9                                       Calle Las Torres-1 Colonia Reforma
    ## 10                                           Blvd. Audi N. 3 Ciudad Modelo
    ## 11                                            Ave. Zeta del Cochero No 407
    ## 12 Av. Benito Juarez 1230 B (Carretera 57) Col. Valle Dorado Zona Hotelera
    ## 13                                                    Blvd. Fuerza Armadas
    ## 14                                                       8640 Alexandra Rd
    ##                                                city state    zip  longitude
    ## 1                                  \nAguascalientes    AG  20345 -102.27633
    ## 2                                          \nCancun    QR  77500  -86.82462
    ## 3                     Col\nPartido Iglesias\nJuarez    CH  32528 -106.41015
    ## 4          Parque Industrial Interamerican\nApodaca    NL  66600 -100.14050
    ## 5                               \nMedellin Colombia   ANT 050016  -75.56551
    ## 6                            Col. Centro\nMonterrey    NL  64000 -100.32011
    ## 7                                       \nMonterrey    NL  64190 -100.33770
    ## 8                                          \nOshawa    ON L1H1B4  -78.86126
    ## 9                                       \nPoza Rica    VE  93210  -97.43758
    ## 10                                \nSan Jose Chiapa    PU  75010  -97.78146
    ## 11  Col. ReservaTerritorial Atlixcayotl San\nPuebla    PU  72810  -98.22969
    ## 12                                \nSan Luis Potosi    SL  78399 -100.93899
    ## 13          contiguo Mall Las Cascadas\nTegucigalpa    FM  11101  -87.19938
    ## 14                                       \nRichmond    BC V6X1C4 -123.12712
    ##     latitude
    ## 1  21.755585
    ## 2  21.151911
    ## 3  31.697130
    ## 4  25.788750
    ## 5   6.224831
    ## 6  25.666541
    ## 7  25.720653
    ## 8  43.898034
    ## 9  20.564131
    ## 10 19.226297
    ## 11 19.027317
    ## 12 22.136998
    ## 13 14.075929
    ## 14 49.177832

#### Isolating US locations

For the rest of this activity, we will work with the data from the
United States *only*. All Denny’s locations in our file are in the US so
we do not need to worry about updating this object, but you do need to
do some work on the `laquinta` data. Create a new object called
`laquinta_us` that only contains the locations inside the US.

``` r
laquinta_us <- laquinta %>% 
  right_join(states, by=c('state'="abbreviation"))
```

### Fewest locations

Let’s test some of our data summary skills.

Which US state(s) has/ve the fewest Denny’s location?

``` r
dennys2 <- dennys %>% 
  group_by(state) %>% 
  summarise(total=n()) %>% 
  arrange(total)
```

**Response**: Delaware has the fewest dennys locations

Which US state(s) has/ve the fewest La Quinta locations?

``` r
laquinta2 <- laquinta_us %>% 
  group_by(state) %>% 
  summarise(total=n()) %>% 
  arrange(total)
```

**Response**: DC, DE, HI, and ME all have 1 la quinta

Is this surprising to you? Why or why not?

**Response**: It is surprising to see that states with few dennys also
have fewer la quinta

### Locations per thousand square miles

Next we will calculate which states have the most Denny’s and La Quinta
locations per thousand square miles. To do this, we will need to join
each company’s tibble with the information in the `states` tibble.

#### Denny’s

1.  Take the `dennys` tibble and determine how many observations are in
    each state. This should have two columns: `state` and `n`.
2.  *Then*, use `inner_join` to join the previous information to the
    `states` tibble. Note that the variables in the `states` tibble has
    the two letter abbreviations in a column called `abbreviation`.
    Therefore, we will need to specify that the `state` variable from
    the `dennys` tibble should be matched `by` the `abbreviation`
    variable from the `states` tibble.
3.  *Then*, calculate the number of Denny’s locations *per* thousand
    square miles.

``` r
dennys2 %>% 
  inner_join(states, by=c('state'='abbreviation')) %>% 
  mutate(per_1000_sq_mi=total/area) %>% 
  arrange(desc(per_1000_sq_mi))
```

    ## # A tibble: 51 x 5
    ##    state total name                     area per_1000_sq_mi
    ##    <chr> <int> <chr>                   <dbl>          <dbl>
    ##  1 DC        2 District of Columbia     68.3       0.0293  
    ##  2 RI        5 Rhode Island           1545.        0.00324 
    ##  3 CA      403 California           163695.        0.00246 
    ##  4 CT       12 Connecticut            5543.        0.00216 
    ##  5 FL      140 Florida               65758.        0.00213 
    ##  6 MD       26 Maryland              12406.        0.00210 
    ##  7 NJ       10 New Jersey             8723.        0.00115 
    ##  8 NY       56 New York              54555.        0.00103 
    ##  9 IN       37 Indiana               36420.        0.00102 
    ## 10 OH       44 Ohio                  44826.        0.000982
    ## # … with 41 more rows

Which states have the most Denny’s locations per thousand square miles?

**Response**: DC has the most

#### La Quinta

Similarly as we previously did for Denny’s, calculate the number of La
Quinta locations *per* thousand square miles.

``` r
laquinta2 %>% 
  inner_join(states, by=c('state'='abbreviation')) %>% 
  mutate(per_1000_sq_mi=total/area) %>% 
  arrange(desc(per_1000_sq_mi))
```

    ## # A tibble: 51 x 5
    ##    state total name                     area per_1000_sq_mi
    ##    <chr> <int> <chr>                   <dbl>          <dbl>
    ##  1 DC        1 District of Columbia     68.3       0.0146  
    ##  2 RI        2 Rhode Island           1545.        0.00129 
    ##  3 FL       74 Florida               65758.        0.00113 
    ##  4 CT        6 Connecticut            5543.        0.00108 
    ##  5 MD       13 Maryland              12406.        0.00105 
    ##  6 TX      237 Texas                268596.        0.000882
    ##  7 TN       30 Tennessee             42144.        0.000712
    ##  8 GA       41 Georgia               59425.        0.000690
    ##  9 NJ        5 New Jersey             8723.        0.000573
    ## 10 MA        6 Massachusetts         10554.        0.000568
    ## # … with 41 more rows

**Response**: DC also has the most

<img src="README-img/noun_pause.png" alt="pause" width = "20"/>
<b>Planned Pause Point</b>: If you feel that you have a good
understanding of these commands, feel free to start working on your
project. The remainder of this activity will deal with creating map data
visualizations.

### Mapping locations

To be able to map all of the `dennys` and `laquinta` locations, we
should combine them into one dataset.

Both of these tibbles have the same columns in the same order (i.e.,
`address`, `city`, `state`, `zip`, `longitude`, `latitude`, `country`)
so we can simply stack them on top of each other by using `bind_rows`.

Why would it not be wise to use a mutating join (e.g., `inner_join`,
`left/right_join`, `full_join`) for these data? That is, both of the
datasets have the same variable names, would it not make sense to join
them at the same address?

**Response**: They have different types for zip codes

If we were able to use a mutating join, what would this output would
look like? Why would this not be helpful?

**Response**:

You were already shown these top-notch animations from [Garrick
Aden-Blue](https://www.garrickadenbuie.com/project/tidyexplain/). Again,
they are a little old as they still use `spread` and `gather`, but the
`*_join` are still accurate.

Prior to combining these datasets, it would be nice to know which
company each location belongs to.

``` r
pre_join_dennys <- dennys %>% 
  mutate(establishment = "Denny's")

pre_join_laquinta_us <- laquinta_us %>% 
  mutate(establishment = "La Quinta") %>% 
  transform(zip=as.integer(zip))
```

Now, stack these two `pre_join_*` tibbles on top of each other. After
you have verified the stacking worked, assign the resulting object to
`dennys_laquinta`.

``` r
bind_rows(pre_join_dennys, pre_join_laquinta_us)
```

    ##                                  address                  city state   zip
    ## 1                            2900 Denali             Anchorage    AK 99503
    ## 2                       3850 Debarr Road             Anchorage    AK 99508
    ## 3                       1929 Airport Way             Fairbanks    AK 99701
    ## 4                       230 Connector Dr                Auburn    AL 36849
    ## 5               224 Daniel Payne Drive N            Birmingham    AL 35207
    ## 6         900 16th St S, Commons on Gree            Birmingham    AL 35294
    ## 7             5931 Alabama Highway, #157               Cullman    AL 35056
    ## 8                 2190 Ross Clark Circle                Dothan    AL 36301
    ## 9                           900 Tyson Rd     Hope Hull (Tyson)    AL 36043
    ## 10                 4874 University Drive            Huntsville    AL 35816
    ## 11                   2209 Se Walton Blvd           Bentonville    AR 72712
    ## 12                         2589 W 6th St          Fayetteville    AR 72704
    ## 13                       5910 Rogers Ave              Ft Smith    AR 72903
    ## 14                  310 S Shackleford Dr           Little Rock    AR 72211
    ## 15                    43 Bradley Cove Rd          Russellville    AR 72802
    ## 16                     4861 W Sunset Ave            Springdale    AR 72762
    ## 17                     5104 N State Line             Texarkana    AR 71854
    ## 18                8300 State Highway 108             Texarkana    AR 71854
    ## 19                  3000 Service Loop Rd          West Memphis    AR 72301
    ## 20                     4121 W Anthem Way                Anthem    AZ 85086
    ## 21                       10623 E Main St       Apache Junction    AZ 85220
    ## 22                     825 N Ocotillo Rd                Benson    AZ 86502
    ## 23                       441 S Watson Rd               Buckeye    AZ 85326
    ## 24                       1762 Highway 95         Bullhead City    AZ 86442
    ## 25                    1630 W Highway 260            Camp Verde    AZ 86322
    ## 26                  1851 E Florence Blvd           Casa Grande    AZ 85122
    ## 27                  1750 W Chandler Blvd              Chandler    AZ 85224
    ## 28                  7400 W Chandler Blvd              Chandler    AZ 85226
    ## 29                        Indian Route 7                Chinle    AZ 86503
    ## 30                  2211 State Route 89a            Cottonwood    AZ 86326
    ## 31                   401 N Chiricahua Rd               Douglas    AZ 85607
    ## 32                 16189 S Sunshine Blvd                  Eloy    AZ 85131
    ## 33                    2122 S Milton Road             Flagstaff    AZ 86001
    ## 34                     2306 E Lucky Lane             Flagstaff    AZ 86004
    ## 35                4650 N Highway 89 C-32             Flagstaff    AZ 86004
    ## 36           Northern Arizona University             Flagstaff    AZ 86011
    ## 37                     17053 E Shea Blvd        Fountain Hills    AZ 85268
    ## 38                      1368 N Cooper Rd               Gilbert    AZ 85233
    ## 39                      1661 E Warner Rd               Gilbert    AZ 85296
    ## 40                 3971 South Gilbert Rd               Gilbert    AZ 85297
    ## 41                     4303 W Peoria Ave              Glendale    AZ 85302
    ## 42                   9856 W Camelback Rd              Glendale    AZ 85305
    ## 43                 5161 W Thunderbird Rd              Glendale    AZ 85306
    ## 44                  1218 N Litchfield Rd              Goodyear    AZ 85338
    ## 45              18875 S I-19 Frontage Rd          Green Valley    AZ 85614
    ## 46                      2510 Navajo Blvd              Holbrook    AZ 86025
    ## 47                    3255 E Andy Devine               Kingman    AZ 86401
    ## 48                3300 E Andy Devine Ave               Kingman    AZ 86401
    ## 49                   1620 McCulloch Blvd      Lake Havasu City    AZ 86403
    ## 50            Edison Rd & State Road 347              Maricopa    AZ 85139
    ## 51                           1210 E Main                  Mesa    AZ 85203
    ## 52                       1330 S Power Rd                  Mesa    AZ 85206
    ## 53                   1150 S Country Club                  Mesa    AZ 85210
    ## 54                   683 North Grand Ave               Nogales    AZ 85621
    ## 55                    669 Scenic View Dr                  Page    AZ 86040
    ## 56                     312 S Beeline Hwy                Payson    AZ 85541
    ## 57                     8737 Nw Grand Ave                Peoria    AZ 85345
    ## 58                        8131 W Bell Rd                Peoria    AZ 85382
    ## 59                  2801 No Black Canyon               Phoenix    AZ 85009
    ## 60                 5002 North Seventh St               Phoenix    AZ 85014
    ## 61                   3456 W Bethany Home               Phoenix    AZ 85017
    ## 62                      2120 E Cactus Rd               Phoenix    AZ 85022
    ## 63                 2525 W Deer Valley Rd               Phoenix    AZ 85027
    ## 64                       4120 N 51st Ave               Phoenix    AZ 85031
    ## 65                        3205 E Bell Rd               Phoenix    AZ 85032
    ## 66                       1401 N 75th Ave               Phoenix    AZ 85043
    ## 67                      6700 West Latham               Phoenix    AZ 85043
    ## 68           9030 N Black Canyon Highway               Phoenix    AZ 85051
    ## 69                        2717 W Bell Rd               Phoenix    AZ 85053
    ## 70                      7000 E Mayo Blvd               Phoenix    AZ 85054
    ## 71                       3160 W Carefree               Phoenix    AZ 85086
    ## 72                 1230 Gail Gardner Way              Prescott    AZ 86305
    ## 73                         7925 E Hwy 69       Prescott Valley    AZ 86312
    ## 74                   1758 W Hunt Highway        San Tan Valley    AZ 85143
    ## 75                     3315 N Scottsdale            Scottsdale    AZ 85251
    ## 76                 9160 E Indian Bend Rd            Scottsdale    AZ 85256
    ## 77                    7605 E McDowell Rd            Scottsdale    AZ 85260
    ## 78                4471 White Mountain Rd              Show Low    AZ 85901
    ## 79                         2397 Fry Blvd          Sierra Vista    AZ 85635
    ## 80                         392 W Hwy 264           St Michaels    AZ 86511
    ## 81                  14799 West Grand Ave              Surprise    AZ 85374
    ## 82                   650 N Scottsdale Rd                 Tempe    AZ 85281
    ## 83                         825 S 48th St                 Tempe    AZ 85281
    ## 84                       1343 W Broadway                 Tempe    AZ 85282
    ## 85                       4403 S Rural Rd                 Tempe    AZ 85282
    ## 86                    1994 W Baseline Rd                 Tempe    AZ 85283
    ## 87                     2875 W Highway 70              Thatcher    AZ 85552
    ## 88                         2 Legacy Lane             Tuba City    AZ 86045
    ## 89                        5000 Oracle Rd                Tucson    AZ 85704
    ## 90                    1510 W Valencia Rd                Tucson    AZ 85706
    ## 91                    2630 E Valencia Rd                Tucson    AZ 85706
    ## 92                       6484 E Broadway                Tucson    AZ 85710
    ## 93                   3702 E Irvington Rd                Tucson    AZ 85714
    ## 94      University of Arizona - Park Stu                Tucson    AZ 85721
    ## 95                         4920 W Ina Rd                Tucson    AZ 85741
    ## 96                         555 N Freeway                Tucson    AZ 85745
    ## 97                      1010 N Tegner Rd            Wickenburg    AZ 85390
    ## 98                     400 Transcon Lane               Winslow    AZ 86047
    ## 99                       11121 Grand Ave             Youngtown    AZ 85363
    ## 100                       1435 E 16th St                  Yuma    AZ 85365
    ## 101             Marine Corps Air Station                  Yuma    AZ 85365
    ## 102            11255 E South Frontage Rd                  Yuma    AZ 85367
    ## 103                 14240 Us Highway 395              Adelanto    CA 92301
    ## 104                        369 W Main St              Alhambra    CA 91801
    ## 105                   1168 W Katella Ave               Anaheim    CA 92802
    ## 106                   1610 S Harbor Blvd               Anaheim    CA 92802
    ## 107                   2080 S Harbor Blvd               Anaheim    CA 92802
    ## 108                      210 S Euclid St               Anaheim    CA 92802
    ## 109            1420 N State College Blvd               Anaheim    CA 92806
    ## 110                   2005 E Katella Ave               Anaheim    CA 92806
    ## 111                  2006 Somersville Rd               Antioch    CA 94509
    ## 112                   4823 Lone Tree Way               Antioch    CA 94531
    ## 113                         20242 Hwy 18          Apple Valley    CA 92307
    ## 114               18989 Bear Valley Road          Apple Valley    CA 92308
    ## 115                 7 East Huntington Dr               Arcadia    CA 91006
    ## 116                 16907 S Pioneer Blvd               Artesia    CA 90701
    ## 117                  6910 El Camino Real            Atascadero    CA 93422
    ## 118                    1601 Sycamore Ave               Atwater    CA 95301
    ## 119                1800 Auburn Ravine Rd                Auburn    CA 95603
    ## 120                     72415 Baker Blvd                 Baker    CA 92309
    ## 121                      2851 White Lane           Bakersfield    CA 93304
    ## 122                   2684 Mt Vernon Ave           Bakersfield    CA 93306
    ## 123                     2309 Panama Lane           Bakersfield    CA 93307
    ## 124                 8011 E Brundage Lane           Bakersfield    CA 93307
    ## 125                 17047 Zachary Avenue           Bakersfield    CA 93308
    ## 126                 2627 Buck Owens Blvd           Bakersfield    CA 93308
    ## 127              35175 7th Standard Road           Bakersfield    CA 93308
    ## 128                      4440 Gosford Rd           Bakersfield    CA 93311
    ## 129                    8710 Rosedale Hwy           Bakersfield    CA 93312
    ## 130        14550 Baldwin Pk Towne Center          Baldwin Park    CA 91706
    ## 131               6305 Joshua Palmer Way               Banning    CA 92220
    ## 132                       1201 E Main St               Barstow    CA 92311
    ## 133                       2611 Fisher Rd               Barstow    CA 92311
    ## 134                      2830 Lenwood Rd               Barstow    CA 92311
    ## 135                         449 E 4th St              Beaumont    CA 92223
    ## 136                  17230 Lakewood Blvd            Bellflower    CA 90706
    ## 137                       9312 Rosecrans            Bellflower    CA 90706
    ## 138                  41196 Big Bear Blvd              Big Bear    CA 92315
    ## 139                       1014 N Main St                Bishop    CA 93514
    ## 140                      876 W Donlon St                Blythe    CA 92225
    ## 141                    7740 Orangethorpe            Buena Park    CA 90621
    ## 142             2525 North Hollywood Way               Burbank    CA 91505
    ## 143                   1010 W Alameda Ave               Burbank    CA 91506
    ## 144                      20672 Tracy Ave          Buttonwillow    CA 93206
    ## 145                        110 W Cole Rd              Calexico    CA 92231
    ## 146                   1184 Calimesa Blvd              Calimesa    CA 92320
    ## 147                     1659 E Dailey Dr             Camarillo    CA 93010
    ## 148                      3446 Coach Lane          Cameron Park    CA 95682
    ## 149                    2060 S Bascom Ave              Campbell    CA 95008
    ## 150             8330 Topanga Canyon Blvd           Canoga Park    CA 91306
    ## 151            18932 W Soledad Canyon Rd        Canyon Country    CA 91351
    ## 152             1048 Carlsbad Village Dr              Carlsbad    CA 92008
    ## 153                  7433 Fair Oaks Blvd            Carmichael    CA 95608
    ## 154                  600 Carson Plaza Dr                Carson    CA 90746
    ## 155                     31724 Castaic Rd               Castaic    CA 91384
    ## 156                        69050 Hwy 111        Cathedral City    CA 92234
    ## 157                      1507 Herndon Rd                 Ceres    CA 95307
    ## 158                  675 Manzanita Court                 Chico    CA 95926
    ## 159                    12180 Central Ave                 Chino    CA 91710
    ## 160             15459 Fairfield Ranch Rd           Chino Hills    CA 91710
    ## 161                        110 Bonita Rd           Chula Vista    CA 91910
    ## 162                             692 E St           Chula Vista    CA 91910
    ## 163                        2110 Birch Rd           Chula Vista    CA 91915
    ## 164                    6215 Sunrise Blvd        Citrus Heights    CA 95610
    ## 165                       5142 Triggs St      City of Commerce    CA 90022
    ## 166                      7268 E Gage Ave      City of Commerce    CA 90040
    ## 167           820 South Indian Hill Blvd             Claremont    CA 91711
    ## 168                     1151 Herndon Ave                Clovis    CA 93612
    ## 169                    710 W Shaw Avenue                Clovis    CA 93612
    ## 170                    25026 West Dorris              Coalinga    CA 93210
    ## 171                     1190 S Mt Vernon                Colton    CA 92324
    ## 172                 160 West Valley Blvd                Colton    CA 92324
    ## 173                  1313 Willow Pass Rd               Concord    CA 94520
    ## 174                       260 Pittman Rd              Cordelia    CA 94585
    ## 175                    2120 South Avenue               Corning    CA 96021
    ## 176                      165 McKinley St                Corona    CA 92879
    ## 177                     2379 Compton Ave                Corona    CA 92881
    ## 178                        1833 W 6th St                Corona    CA 92882
    ## 179                     3170 Harbor Blvd            Costa Mesa    CA 92626
    ## 180                          1225 5th St         Crescent City    CA 95531
    ## 181                 10700 Jefferson Blvd           Culver City    CA 90230
    ## 182                       #2 Serra Monte             Daly City    CA 94015
    ## 183                     34242 Del Obispo            Dana Point    CA 92629
    ## 184                 2203 via De La Valle               Del Mar    CA 92014
    ## 185                 14390 County Line Rd                Delano    CA 93215
    ## 186                   1250 Stratford Ave                 Dixon    CA 95620
    ## 187                8350 E Firestone Blvd                Downey    CA 90241
    ## 188                   1060 Huntington Dr                Duarte    CA 91010
    ## 189                      2642 Jamacha Rd              El Cajon    CA 92019
    ## 190                       2691 Navajo Rd              El Cajon    CA 92020
    ## 191                       1202 W Main St              El Cajon    CA 92021
    ## 192                   665 N Mollison Ave              El Cajon    CA 92021
    ## 193                  1445 Ocotillo Drive             El Centro    CA 92243
    ## 194                     3403 Dogwood Ave             El Centro    CA 92243
    ## 195                  11344 San Pablo Ave            El Cerrito    CA 94530
    ## 196                       3540 N Peck Rd              El Monte    CA 91731
    ## 197                   9804 E Flair Drive              El Monte    CA 91731
    ## 198                 2555 El Segundo Blvd            El Segundo    CA 90245
    ## 199                  8707 Elk Grove Blvd             Elk Grove    CA 95624
    ## 200                   1776 Powell Street            Emeryville    CA 94608
    ## 201                   135 Encinitas Blvd             Encinitas    CA 92024
    ## 202                    510 W Mission Ave             Escondido    CA 92025
    ## 203                           136 5th St                Eureka    CA 95501
    ## 204                    1360 Holiday Lane             Fairfield    CA 94534
    ## 205                     2980 Travis Blvd             Fairfield    CA 94534
    ## 206                    713 South Main St             Fallbrook    CA 92028
    ## 207                    1011 Riley Street                Folsom    CA 95630
    ## 208                  17009-A Valley Blvd               Fontana    CA 92335
    ## 209                   13479 Baseline Ave               Fontana    CA 92336
    ## 210                26712 Portola Parkway        Foothill Ranch    CA 92610
    ## 211                  16205 Brookhurst St       Fountain Valley    CA 92708
    ## 212                    160 N 10th Street                Fowler    CA 93625
    ## 213                       5280 Mowry Ave               Fremont    CA 94538
    ## 214                   46645 Mission Blvd               Fremont    CA 94539
    ## 215                          141 Abby St                Fresno    CA 93701
    ## 216                3120 N Blackstone Ave                Fresno    CA 93703
    ## 217                      2568 S East Ave                Fresno    CA 93706
    ## 218                    1110 East Shaw St                Fresno    CA 93710
    ## 219                      1746 W Shaw Ave                Fresno    CA 93711
    ## 220                     30 E Herndon Ave                Fresno    CA 93720
    ## 221                 530 South Clovis Ave                Fresno    CA 93727
    ## 222                    1121 N Parkway Dr                Fresno    CA 93728
    ## 223                        901 N Main St              Ft Bragg    CA 95437
    ## 224                   2920 E Nutwood Ave             Fullerton    CA 92831
    ## 225                   1101 S Harbor Blvd             Fullerton    CA 92832
    ## 226                            1055 C St                  Galt    CA 95632
    ## 227                    13302 Harbor Blvd          Garden Grove    CA 92843
    ## 228                  18620 S Western Ave               Gardena    CA 90248
    ## 229                  8425 San Ysidro Ave                Gilroy    CA 95020
    ## 230                    546 W Baseline Rd              Glendora    CA 91740
    ## 231                      12820 St Hwy 33               Gustine    CA 95322
    ## 232                  1150 So Seventh Ave      Hacienda Heights    CA 91745
    ## 233                    1635 Glendale Ave               Hanford    CA 93230
    ## 234                 13201 Hawthorne Blvd             Hawthorne    CA 90250
    ## 235                      14301 Inglewood             Hawthorne    CA 90250
    ## 236                     30163 Industrial               Hayward    CA 94544
    ## 237                   2675 W Florida Ave                 Hemet    CA 92545
    ## 238                        13165 Main St              Hesperia    CA 92345
    ## 239                          6100 Sunset             Hollywood    CA 90028
    ## 240                     18477 Beach Blvd      Huntington Beach    CA 92648
    ## 241                        82120 Hwy 111                 Indio    CA 92201
    ## 242                 14962 Sand Canyon Dr                Irvine    CA 92618
    ## 243                         200 S Hwy 49               Jackson    CA 95642
    ## 244                  27585 Bernard Drive        Kettleman City    CA 93239
    ## 245                        1129 Broadway             King City    CA 93930
    ## 246                           801 Sierra             Kingsburg    CA 93631
    ## 247                    1150 S Beach Blvd              La Habra    CA 90631
    ## 248                       4235 Spring St               La Mesa    CA 91941
    ## 249                    15553 Valley Blvd             La Puente    CA 91744
    ## 250                       31760 Grape St         Lake Elsinore    CA 92532
    ## 251                     23515 El Toro Rd           Lake Forest    CA 92630
    ## 252                  13584 Camino Canada              Lakeside    CA 92040
    ## 253                     2634 E Carson St              Lakewood    CA 90712
    ## 254                    5520 South Street              Lakewood    CA 90713
    ## 255                      11605 Carson St              Lakewood    CA 90715
    ## 256                      1028 West Ave I             Lancaster    CA 93534
    ## 257                         2005 W Ave K             Lancaster    CA 93536
    ## 258                      16851 Harlan Rd               Lathrop    CA 95330
    ## 259                       9046 Loop Rd E                 Lebec    CA 93243
    ## 260                 701 E Kettleman Lane                  Lodi    CA 95240
    ## 261                  15237 Thornton Road                  Lodi    CA 95242
    ## 262                601 N Long Beach Blvd            Long Beach    CA 90802
    ## 263           5570 Pacific Coast Highway            Long Beach    CA 90804
    ## 264                    3333 Atlantic Ave            Long Beach    CA 90807
    ## 265               2860 N Bellflower Blvd            Long Beach    CA 90815
    ## 266                 400 N Vermont Avenue           Los Angeles    CA 90004
    ## 267                    635 S Vermont Ave           Los Angeles    CA 90005
    ## 268                   3750 Wilshire Blvd           Los Angeles    CA 90010
    ## 269                       530 Ramirez St           Los Angeles    CA 90012
    ## 270                 3740 S Crenshaw Blvd           Los Angeles    CA 90016
    ## 271                    888 S Figueroa St           Los Angeles    CA 90017
    ## 272      4760 East Cesar E Chavez Avenue           Los Angeles    CA 90022
    ## 273                     5751 Sunset Blvd           Los Angeles    CA 90028
    ## 274                       5535 W Century           Los Angeles    CA 90045
    ## 275                  8601 S Bellanca Ave           Los Angeles    CA 90045
    ## 276                 11700 Wilmington Ave           Los Angeles    CA 90059
    ## 277                 3060 San Fernando Rd           Los Angeles    CA 90065
    ## 278                    1491 West Pacheco             Los Banos    CA 93635
    ## 279                  2160 E Pacheco Blvd             Los Banos    CA 93635
    ## 280                           I-5 Hwy 46            Lost Hills    CA 93249
    ## 281                11195 Long Beach Blvd               Lynwood    CA 90262
    ## 282                     22717 Ave 18 1/2                Madera    CA 93637
    ## 283                       1135 S Main St               Manteca    CA 95336
    ## 284                 110 Reservation Road                Marina    CA 93933
    ## 285                         630 Tenth St            Marysville    CA 95901
    ## 286                   5900 Atlantic Blvd               Maywood    CA 90270
    ## 287                  1500 Ana Sparks Way         Mckinleyville    CA 95519
    ## 288                       90480 66th Ave                 Mecca    CA 92254
    ## 289                       1128 W 13th St                Merced    CA 95341
    ## 290                         333 S Abbott              Milpitas    CA 95035
    ## 291        6285 Pats Ranch Road, Suite A             Mira Loma    CA 91752
    ## 292                 24445 Alicia Parkway         Mission Viejo    CA 92691
    ## 293                     1525 McHenry Ave               Modesto    CA 95350
    ## 294                 2052 W Orangeburg St               Modesto    CA 95350
    ## 295                       110 McHenry St               Modesto    CA 95354
    ## 296                     16262 Sierra Hwy                Mojave    CA 93501
    ## 297                       2727 via Campo            Montebello    CA 90640
    ## 298                2137 North Fremont St              Monterey    CA 93940
    ## 299                   300 Fremont Street              Monterey    CA 93940
    ## 300              832 New Los Angeles Ave              Moorpark    CA 93021
    ## 301                 24952 Sunnymead Blvd         Moreno Valley    CA 92553
    ## 302                      875 Cochrane Rd           Morgan Hill    CA 95037
    ## 303                    25365 Madison Ave              Murrieta    CA 92562
    ## 304               Highway 79 & Benton Rd              Murrieta    CA 92563
    ## 305                       63690 20th Ave        N Palm Springs    CA 92258
    ## 306                       1000 Imola Ave                  Napa    CA 94559
    ## 307                   1904 Sweetwater Rd         National City    CA 91950
    ## 308                   424 W Mile of Cars         National City    CA 91950
    ## 309                            1400 J St               Needles    CA 92363
    ## 310           2830 Camino Del Rios North          Newbury Park    CA 91320
    ## 311                     681 Newcastle Rd             Newcastle    CA 95658
    ## 312                   25341 Chiquella Dr               Newhall    CA 91381
    ## 313                   11377 Burbank Blvd          No Hollywood    CA 91601
    ## 314                    1360 N Hamner Ave                 Norco    CA 92860
    ## 315                    13136 Sherman Way       North Hollywood    CA 91605
    ## 316                       9001 Tampa Ave            Northridge    CA 91324
    ## 317                  17018 Devonshire St            Northridge    CA 91325
    ## 318                   12616 Pioneer Blvd               Norwalk    CA 90650
    ## 319                       1555 East F St               Oakdale    CA 95361
    ## 320                         40650 Hwy 41              Oakhurst    CA 93644
    ## 321                      601 Hegenberger               Oakland    CA 94621
    ## 322                  2273 El Camino Real             Oceanside    CA 92054
    ## 323                        487 Harbor Dr             Oceanside    CA 92054
    ## 324                     605 College Blvd             Oceanside    CA 92057
    ## 325                     2421 S Archibald               Ontario    CA 91761
    ## 326                        1409 E 4th St               Ontario    CA 91762
    ## 327                       502 N Vineyard               Ontario    CA 91764
    ## 328                   715 N Milliken Ave               Ontario    CA 91764
    ## 329                   1695 E Lincoln Ave                Orange    CA 92865
    ## 330                   3000 W Chapman Ave                Orange    CA 92868
    ## 331                    8841 Greenback Ln            Orangevale    CA 95662
    ## 332                    2470 Oro Dam Blvd              Oroville    CA 95966
    ## 333                        1255 N Oxnard                Oxnard    CA 93030
    ## 334                       800 Garnet Ave         Pacific Beach    CA 92109
    ## 335                1201 N Palm Canyon Dr          Palm Springs    CA 92262
    ## 336               1257 Rancho Vista Blvd              Palmdale    CA 93551
    ## 337                      37050 47th St E              Palmdale    CA 93552
    ## 338                   8500 Van Nuys Blvd         Panorama City    CA 91402
    ## 339                 2627 E Colorado Blvd              Pasadena    CA 91107
    ## 340                         1310 24th St           Paso Robles    CA 93446
    ## 341                      15001 Rogers Rd             Patterson    CA 95363
    ## 342                         570 E 4th St                Perris    CA 92570
    ## 343                   4986 Petaluma Blvd              Petaluma    CA 94952
    ## 344                 8600 E Whittier Blvd           Pico Rivera    CA 90660
    ## 345                   611 Five Cities Dr           Pismo Beach    CA 93449
    ## 346               108 E Orangethorpe Ave             Placentia    CA 92870
    ## 347                      99 Fair Lane Dr           Placerville    CA 95667
    ## 348                612 Contra Costa Blvd         Pleasant Hill    CA 94523
    ## 349                    3012 W Temple Ave                Pomona    CA 91766
    ## 350                     1504 Gillette Rd                Pomona    CA 91768
    ## 351                 3801 W Temple Avenue                Pomona    CA 91768
    ## 352                    2511 N Ventura Rd          Port Hueneme    CA 93041
    ## 353                 1262 W Henderson Ave           Porterville    CA 93257
    ## 354                   360 Montgomery Ave           Porterville    CA 93257
    ## 355                         1946 Main St                Ramona    CA 92065
    ## 356                    2474 Sunrise Blvd        Rancho Cordova    CA 95670
    ## 357                 11899 Foothills Blvd      Rancho Cucamonga    CA 91730
    ## 358                 29019 Western Avenue   Rancho Palos Verdes    CA 90275
    ## 359                     48 Antelope Blvd             Red Bluff    CA 96080
    ## 360                      2608 Hilltop Dr               Redding    CA 96002
    ## 361                     542 E Cypress St               Redding    CA 96002
    ## 362                    9 W Redlands Blvd              Redlands    CA 92373
    ## 363                      1180 Alabama St              Redlands    CA 92374
    ## 364                   1760 Aviation Blvd         Redondo Beach    CA 90278
    ## 365                        1201 Broadway          Redwood City    CA 94063
    ## 366                     7600 Reseda Blvd                Reseda    CA 91335
    ## 367                 1377 W Foothill Blvd                Rialto    CA 92376
    ## 368                  104 China Lake Blvd            Ridgecrest    CA 93555
    ## 369                1501 N Jack Tone Road                 Ripon    CA 95366
    ## 370                  2955 Van Buren Blvd             Riverside    CA 92503
    ## 371                      3530 Madison St             Riverside    CA 92504
    ## 372                  1245 University Ave             Riverside    CA 92507
    ## 373                      4460 Rocklin Rd               Rocklin    CA 95677
    ## 374                      122 Sunrise Ave             Roseville    CA 95661
    ## 375                  5181 Foothills Blvd             Roseville    CA 95747
    ## 376                       1425 S Nogales       Rowland Heights    CA 91748
    ## 377                      6875 Valley Way              Rubidoux    CA 92509
    ## 378                     300 Bercut Drive            Sacramento    CA 95814
    ## 379                     3520 Auburn Blvd            Sacramento    CA 95821
    ## 380                       5460 Florin Rd            Sacramento    CA 95823
    ## 381                        1322 Howe Ave            Sacramento    CA 95825
    ## 382                 7900 College Town Dr            Sacramento    CA 95826
    ## 383                       7401 Elsie Ave            Sacramento    CA 95828
    ## 384                  4441 E Commerce Way            Sacramento    CA 95834
    ## 385                  5120 Interstate Ave            Sacramento    CA 95842
    ## 386                     4324 Salida Blvd                Salida    CA 95368
    ## 387                     1255 De La Torre               Salinas    CA 93905
    ## 388                   2005 N Main Street               Salinas    CA 93906
    ## 389                      975 E Blanco Rd               Salinas    CA 93906
    ## 390                   702 E Highland Ave        San Bernardino    CA 92404
    ## 391                      5975 N Palm Ave        San Bernardino    CA 92407
    ## 392                     529 Avenida Pico          San Clemente    CA 92672
    ## 393                   2445 El Cajon Blvd             San Diego    CA 92104
    ## 394                  4365 University Ave             San Diego    CA 92105
    ## 395                    1601 Rosecrans St             San Diego    CA 92106
    ## 396                  1065 Camino Del Rio             San Diego    CA 92108
    ## 397                       7676 Friars Rd             San Diego    CA 92108
    ## 398                3704 Camino Del Rio W             San Diego    CA 92110
    ## 399                    3920 W Point Loma             San Diego    CA 92110
    ## 400                       5812 Hardy Ave             San Diego    CA 92115
    ## 401            4280 Clairemont Mesa Blvd             San Diego    CA 92117
    ## 402                        6970 Alvarado             San Diego    CA 92120
    ## 403                    6908 Miramar Road             San Diego    CA 92121
    ## 404                 5445 Kearny Villa Rd             San Diego    CA 92123
    ## 405             16686 Bernardo Center Dr             San Diego    CA 92128
    ## 406                  9898 Mira Mesa Blvd             San Diego    CA 92131
    ## 407                         Mcas Miramar             San Diego    CA 92145
    ## 408                    2420 Coronado Ave             San Diego    CA 92154
    ## 409                      548 W Arrow Hwy             San Dimas    CA 91773
    ## 410                   1041 Truman Street          San Fernando    CA 91340
    ## 411                       816 Mission St         San Francisco    CA 94103
    ## 412                         495 Beach St         San Francisco    CA 94133
    ## 413                  1290 North State St           San Jacinto    CA 92583
    ## 414                  1390 South First St              San Jose    CA 95110
    ## 415                        1490 N 1st St              San Jose    CA 95112
    ## 416                   1140 Hillsdale Ave              San Jose    CA 95118
    ## 417                       1001 E Capitol              San Jose    CA 95121
    ## 418                       2401 Lanai Ave              San Jose    CA 95122
    ## 419                 1015 Blossom Hill Rd              San Jose    CA 95123
    ## 420                     910 Saratoga Ave              San Jose    CA 95129
    ## 421                        2077 N 1st St              San Jose    CA 95131
    ## 422                    2484 Berryessa Rd              San Jose    CA 95133
    ## 423                     1803 Marina Blvd           San Leandro    CA 94577
    ## 424                    15015 Freedom Ave           San Leandro    CA 94578
    ## 425                       208 Madonna Rd       San Luis Obispo    CA 93405
    ## 426                731 W San Marcos Blvd            San Marcos    CA 92069
    ## 427                       2920 S Norfolk             San Mateo    CA 94403
    ## 428                       2526 San Pablo             San Pablo    CA 94806
    ## 429                  130 E Calle Primero            San Ysidro    CA 92173
    ## 430                      536 Academy Ave                Sanger    CA 93657
    ## 431                       2314 E 17th St             Santa Ana    CA 92701
    ## 432                2530 S Bristol Street             Santa Ana    CA 92704
    ## 433                       1501 W 17th St             Santa Ana    CA 92706
    ## 434                        3614 State St         Santa Barbara    CA 93105
    ## 435                  1745 El Camino Real           Santa Clara    CA 95050
    ## 436                  3715 El Camino Real           Santa Clara    CA 95050
    ## 437                      16401 Delone St         Santa Clarita    CA 91387
    ## 438                        1515 Ocean St            Santa Cruz    CA 95060
    ## 439                    1019 East Main St           Santa Maria    CA 93454
    ## 440                    1560 Lincoln Blvd          Santa Monica    CA 90401
    ## 441                       327 S Palm Ave           Santa Paula    CA 93060
    ## 442                   1000 W Steele Lane            Santa Rosa    CA 95403
    ## 443                        115 Baker Ave            Santa Rosa    CA 95407
    ## 444                 140 Town Center Pkwy                Santee    CA 92071
    ## 445                 2940 Westminster Ave            Seal Beach    CA 90740
    ## 446                    2763 Highland Ave                 Selma    CA 93662
    ## 447                5525 N Sepulveda Blvd          Sherman Oaks    CA 91411
    ## 448                      2460 N Sycamore           Simi Valley    CA 93065
    ## 449          3064 H De La Rosa Sr Street               Soledad    CA 93960
    ## 450                5811 E Firestone Blvd            South Gate    CA 90280
    ## 451               2870 S Lake Tahoe Blvd      South Lake Tahoe    CA 96150
    ## 452                      10 Airport Blvd   South San Francisco    CA 94080
    ## 453                     12924 Beach Blvd               Stanton    CA 90680
    ## 454                 642 West Charter Way              Stockton    CA 95206
    ## 455                    2670 W March Lane              Stockton    CA 95207
    ## 456                     4747 Pacific Ave              Stockton    CA 95207
    ## 457                     3950 Waterloo Rd              Stockton    CA 95215
    ## 458          5033 S Hwy 99 E Frontage Rd              Stockton    CA 95215
    ## 459                    8022 Vineland Ave            Sun Valley    CA 91352
    ## 460                      814 Ahwanee Ave             Sunnyvale    CA 94085
    ## 461                       311 S Mathilda             Sunnyvale    CA 94086
    ## 462                  12861 Encinitas Ave                Sylmar    CA 91342
    ## 463                  13201 Gladstone Ave                Sylmar    CA 91342
    ## 464                     9000 Magellan St             Tehachapi    CA 93561
    ## 465         28915 Rancho California Road              Temecula    CA 92590
    ## 466                   5603 Rosemead Blvd           Temple City    CA 91780
    ## 467                      72244 Varner Rd        Thousand Palms    CA 92276
    ## 468                 21270 Hawthorne Blvd              Torrance    CA 90503
    ## 469                  3060 Sepulveda Blvd              Torrance    CA 90505
    ## 470                   3718 No Tracy Blvd                 Tracy    CA 95304
    ## 471                 795 E Prosperity Ave                Tulare    CA 93274
    ## 472                      1991 Lander Ave               Turlock    CA 95380
    ## 473                  1571 El Camino Real                Tustin    CA 92780
    ## 474        73669-91 Twentynine Palms Hwy      Twentynine Palms    CA 92277
    ## 475                          105 Pomeroy                 Ukiah    CA 95482
    ## 476                  825 W Foothill Blvd                Upland    CA 91786
    ## 477               1701 E Monte Vista Ave             Vacaville    CA 95688
    ## 478              27541 Wayne Mills Place              Valencia    CA 91355
    ## 479                   250 Fairgrounds Dr               Vallejo    CA 94589
    ## 480                     4355 Sonoma Blvd               Vallejo    CA 94589
    ## 481                    14445 Sherman Way              Van Nuys    CA 91405
    ## 482                 15540 Roscoe Bouleva              Van Nuys    CA 91406
    ## 483                    16575 Sherman Way              Van Nuys    CA 91406
    ## 484                        2148 E Harbor               Ventura    CA 93001
    ## 485               14244 Valley Center Dr           Victorville    CA 92392
    ## 486                    2332 South Mooney               Visalia    CA 93277
    ## 487                       200 S Akers St               Visalia    CA 93291
    ## 488                      540 W Vista Way                 Vista    CA 92083
    ## 489                     1235 Harbor Blvd          W Sacramento    CA 95691
    ## 490                      132 N Grand Ave              W Covina    CA 91791
    ## 491                      1804 Highway 46                 Wasco    CA 93280
    ## 492                          645 N Azusa           West Covina    CA 91791
    ## 493     Joe's Truck Plaza, 4415 Howard R               Westley    CA 95387
    ## 494                    1060 Tiverton Ave              Westwood    CA 90024
    ## 495                  11891 Whittier Blvd              Whittier    CA 90601
    ## 496                  8425 S Pioneer Blvd              Whittier    CA 90606
    ## 497               23857 Clinton Keith Rd              Wildomar    CA 92595
    ## 498                 1440 W Pacific Coast            Wilmington    CA 90744
    ## 499                     21 Bernard Court              Woodland    CA 95695
    ## 500                       1568 E Main St              Woodland    CA 95776
    ## 501                   20137 Ventura Blvd        Woodland Hills    CA 91364
    ## 502                   22027 Burbank Blvd        Woodland Hills    CA 91364
    ## 503               22611 Oak Crest Circle           Yorba Linda    CA 92887
    ## 504                   33540 Yucaipa Blvd               Yucaipa    CA 92399
    ## 505                   56895 29 Palms Hwy          Yucca Valley    CA 92284
    ## 506                      14400 E 6th Ave                Aurora    CO 80011
    ## 507                     16751 E 32nd Ave                Aurora    CO 80011
    ## 508                        1505 S Havana                Aurora    CO 80012
    ## 509                     2905 Baseline Rd               Boulder    CO 80303
    ## 510                 415 S Lincoln Street            Burlington    CO 80807
    ## 511              3205 I-70 Business Loop               Clifton    CO 81520
    ## 512                       315 W Bijou St      Colorado Springs    CO 80905
    ## 513                     1450 Harrison Rd      Colorado Springs    CO 80906
    ## 514                   302 N Academy Blvd      Colorado Springs    CO 80909
    ## 515                  8125 N Academy Blvd      Colorado Springs    CO 80920
    ## 516                       2059 E Main St                Cortez    CO 81321
    ## 517                 1605 Nw Federal Blvd                Denver    CO 80204
    ## 518                 900 West Alameda Ave                Denver    CO 80223
    ## 519                     12090 E 40th Ave                Denver    CO 80239
    ## 520                   666 Camino Del Rio               Durango    CO 81301
    ## 521                    275 W Hampden Ave             Englewood    CO 80110
    ## 522                       420 Centro Way          Fort Collins    CO 80524
    ## 523              6715 Mesa Ridge Parkway              Fountain    CO 80817
    ## 524         3291 Youngfield Service Road                Golden    CO 80401
    ## 525                       710 Horizon Dr        Grand Junction    CO 81501
    ## 526        1164 Sargent Jon Stiles Drive       Highlands Ranch    CO 80129
    ## 527                       565 Union Blvd              Lakewood    CO 80228
    ## 528                    3580 So Wadsworth              Lakewood    CO 80235
    ## 529                          2455 6th St                 Limon    CO 80828
    ## 530                     1515 Venture Way              Montrose    CO 81401
    ## 531                          3600 N Frwy                Pueblo    CO 81003
    ## 532                   350 East 104th Ave              Thornton    CO 80233
    ## 533                      9930 W 49th Ave            Wheatridge    CO 80033
    ## 534                100 Morning Sun Drive         Woodland Park    CO 80863
    ## 535                        61 Newtown Rd               Danbury    CT  6810
    ## 536                           111 Elm St               Enfield    CT  6082
    ## 537                     539 Flatbush Ave              Hartford    CT  6106
    ## 538                 655 S Main St, Rt 17            Middletown    CT  6457
    ## 539                     1467 Whalley Ave             New Haven    CT  6515
    ## 540                    336 S Frontage Rd            New London    CT  6320
    ## 541                         621 Queen St           Southington    CT  6489
    ## 542                    35 Talcotville Rd                Vernon    CT  6066
    ## 543                       939 Wolcott St             Waterbury    CT  6705
    ## 544                       487 Sawmill Rd            West Haven    CT  6516
    ## 545                   28 Flat Rock Place             Westbrook    CT  6498
    ## 546                 1298 Silas Deane Hwy          Wethersfield    CT  6109
    ## 547               1250 Bladensburg Rd Ne            Washington    DC 20002
    ## 548                   4445 Benning Rd Ne            Washington    DC 20019
    ## 549                   80 MacIntosh Plaza                Newark    DE 19713
    ## 550                   255 E Altamonte Dr     Altamonte Springs    FL 32701
    ## 551                        351 E Main St                Apopka    FL 32703
    ## 552                      1035 N Broadway                Bartow    FL 33830
    ## 553                12105 Us Highway 19 N         Bayonet Point    FL 34667
    ## 554                   3249 N Federal Hwy            Boca Raton    FL 33431
    ## 555                 1311 W Palmetto Park            Boca Raton    FL 33486
    ## 556                       610 44th Ave W             Bradenton    FL 34207
    ## 557                  1301 W Brandon Blvd               Brandon    FL 33511
    ## 558                  1306 Del Prado Blvd            Cape Coral    FL 33990
    ## 559                   198 E Semoran Blvd           Casselberry    FL 32707
    ## 560                   26489 Us Hwy 19 No            Clearwater    FL 33761
    ## 561                  25 Town Center Blvd              Clermont    FL 34711
    ## 562                  1245 N Atlantic Ave           Cocoa Beach    FL 32931
    ## 563                       1 Miracle Mile          Coral Gables    FL 33134
    ## 564             1150 South Dixie Highway          Coral Gables    FL 33146
    ## 565                       2380 Nw Hwy 19         Crystal River    FL 34428
    ## 566                        44111 S Us 27             Davenport    FL 33837
    ## 567             5645 South University Dr                 Davie    FL 33328
    ## 568                  2701 N Atlantic Ave         Daytona Beach    FL 32118
    ## 569                  3162 S Atlantic Ave         Daytona Beach    FL 32118
    ## 570                 1206 N Woodland Blvd                Deland    FL 32720
    ## 571                 1206 N Woodland Blvd                Deland    FL 32720
    ## 572                 1910 South McCall Rd             Englewood    FL 34223
    ## 573                     16150 Us Hwy 441                Eustis    FL 32726
    ## 574                    401 Se 1st Avenue          Florida City    FL 33034
    ## 575                      3151 Nw 9th Ave         Ft Lauderdale    FL 33309
    ## 576                   1661 S Federal Hwy         Ft Lauderdale    FL 33316
    ## 577           8031 Summerlin Center Road              Ft Myers    FL 33907
    ## 578                  9340 Marketplace Rd              Ft Myers    FL 33912
    ## 579                   5000 N.federal Hwy         Ft Lauderdale    FL 33308
    ## 580                  100 N Kings Highway             Ft Pierce    FL 34945
    ## 581                  543 N Eglin Parkway       Ft Walton Beach    FL 32547
    ## 582                35649 Us Hwy 27 North           Haines City    FL 33844
    ## 583           1025 W Hallandale Bch Blvd      Hallandale Beach    FL 33009
    ## 584                       1000 W 49th St               Hialeah    FL 33012
    ## 585                  11701 Okeechobee Rd       Hialeah Gardens    FL 33018
    ## 586                  2800 N 28th Terrace             Hollywood    FL 33020
    ## 587                       404 S 60th Ave             Hollywood    FL 33023
    ## 588                    1071 Airport Road          Jacksonville    FL 32218
    ## 589                     13874 Beach Blvd          Jacksonville    FL 32224
    ## 590                  10445 Atlantic Blvd          Jacksonville    FL 32225
    ## 591                   8409 Blanding Blvd          Jacksonville    FL 32244
    ## 592                 8215 Dix Ellis Trail          Jacksonville    FL 32256
    ## 593                   97630 Overseas Hwy             Key Largo    FL 33037
    ## 594                         925 Duval St              Key West    FL 33040
    ## 595                   2509 W Vine Street             Kissimmee    FL 34741
    ## 596               1515 E Osceola Parkway             Kissimmee    FL 34744
    ## 597                  2051 E Irlo Bronson             Kissimmee    FL 34744
    ## 598              4783 W Irlo Bronson Hwy             Kissimmee    FL 34746
    ## 599                  5855 W Irlo Bronson             Kissimmee    FL 34746
    ## 600             7632 W Irlo Bronson  Hwy             Kissimmee    FL 34747
    ## 601                      4541 MacEy Lane            Lake Wales    FL 33853
    ## 602                   4541 Hypoluxo Road            Lake Worth    FL 33463
    ## 603                     3204 N Us Hwy 98              Lakeland    FL 33805
    ## 604                   4325 S Florida Ave              Lakeland    FL 33813
    ## 605                     940 Missouri Ave                 Largo    FL 33770
    ## 606                     2480 Citrus Blvd              Leesburg    FL 34748
    ## 607                   6190 S State Rd 53               Madison    FL 32340
    ## 608                  650 N State Road #7               Margate    FL 33063
    ## 609                     4530 W New Haven             Melbourne    FL 32904
    ## 610                  75 E Merritt Island        Merritt Island    FL 32952
    ## 611                   3600 Biscayne Blvd                 Miami    FL 33137
    ## 612                      8503 Sw 40th St                 Miami    FL 33155
    ## 613                  19313 So Dixie Hgwy                 Miami    FL 33157
    ## 614                      5825 Nw 36th St                 Miami    FL 33166
    ## 615                      7411 Nw 36th St                 Miami    FL 33166
    ## 616                    9545 W Flagler St                 Miami    FL 33174
    ## 617                    3850 Sw 137th Ave                 Miami    FL 33175
    ## 618                   15235 Sw 137th Ave                 Miami    FL 33177
    ## 619                     11350 Nw 41st St                 Miami    FL 33178
    ## 620                12000 N Kendall Drive                 Miami    FL 33186
    ## 621                  7140 Collins Avenue           Miami Beach    FL 33141
    ## 622                 19780 Nw 27th Avenue         Miami Gardens    FL 33056
    ## 623                   18600 N W 67th Ave           Miami Lakes    FL 33015
    ## 624                  32670 Blue Star Hwy                Midway    FL 32343
    ## 625             1450 Miami Gardens Dr Ne               N Miami    FL 33179
    ## 626                  12105 Biscayne Blvd               N Miami    FL 33181
    ## 627                         200 N 3rd St         Neptune Beach    FL 32266
    ## 628                     4442 Us Hwy 19 S       New Port Richey    FL 34652
    ## 629                     1830 State Rd 44      New Smyrna Beach    FL 32168
    ## 630                911 E Commercial Blvd          Oakland Park    FL 33334
    ## 631        3801 West Silver Springs Blvd                 Ocala    FL 34482
    ## 632                  11261 W Colonial Dr                 Ocoee    FL 34761
    ## 633                      1012 Saxon Blvd           Orange City    FL 32763
    ## 634                   440 S Semoran Blvd               Orlando    FL 32807
    ## 635                    8076 S Orange Ave               Orlando    FL 32809
    ## 636                   5825 International               Orlando    FL 32819
    ## 637                   7660 International               Orlando    FL 32819
    ## 638               8243 S John Young Pkwy               Orlando    FL 32819
    ## 639                   8747 International               Orlando    FL 32819
    ## 640                   9880 International               Orlando    FL 32819
    ## 641               11037 International Dr               Orlando    FL 32821
    ## 642                     5725 Tg Lee Blvd               Orlando    FL 32822
    ## 643                  11915 E Colonial Dr               Orlando    FL 32825
    ## 644                   12375 State Rd 535               Orlando    FL 32836
    ## 645                  110 Williamson Blvd          Ormond Beach    FL 32174
    ## 646                   840 Palm Bay Rd Ne              Palm Bay    FL 32905
    ## 647                          7 Kings Hwy            Palm Coast    FL 32137
    ## 648                  3000 S Congress Ave          Palm Springs    FL 33461
    ## 649         2438 Martin Luther King Blvd           Panama City    FL 32405
    ## 650                 1290 N University Dr        Pembroke Pines    FL 33024
    ## 651          10800 Pines Blvd, Suite #15        Pembroke Pines    FL 33026
    ## 652                  4625 Mobile Highway             Pensacola    FL 32506
    ## 653                 7748 N Davis Highway             Pensacola    FL 32514
    ## 654              535 West Nine Mile Road             Pensacola    FL 32534
    ## 655                   2001 S Frontage Rd            Plant City    FL 33566
    ## 656                    1727 N University            Plantation    FL 33322
    ## 657                 12100 W Sunrise Blvd            Plantation    FL 33323
    ## 658                     840 Cypress Blvd             Poinciana    FL 34759
    ## 659                   2671 N Federal Hwy         Pompano Beach    FL 33064
    ## 660                   1340 Tamiami Trail        Port Charlotte    FL 33948
    ## 661                   1641 Dunlawton Ave           Port Orange    FL 32127
    ## 662                         10111 S Us 1         Port St Lucie    FL 34952
    ## 663               4102 W Blue Heron Blvd         Riviera Beach    FL 33404
    ## 664                 300 Civic Center Way      Royal Palm Beach    FL 33411
    ## 665                  29933 State Road 52           San Antonio    FL 33576
    ## 666                      3771 Orlando Dr               Sanford    FL 32773
    ## 667                    3701 Bee Ridge Rd              Sarasota    FL 34233
    ## 668                   4390 Us Highway 27               Sebring    FL 33870
    ## 669           5751 E Silver Springs Blvd        Silver Springs    FL 34488
    ## 670                     1445 Wendy Court           Spring Hill    FL 34607
    ## 671            1300 N Ponce De Leon Blvd          St Augustine    FL 32084
    ## 672                   2455 State Road 16          St Augustine    FL 32084
    ## 673              950 State Road 206 West          St Augustine    FL 32086
    ## 674                   4999 34th St North         St Petersburg    FL 33714
    ## 675                   2331 S Federal Hwy                Stuart    FL 34994
    ## 676            3747 Sun City Center Blvd              Sun City    FL 33573
    ## 677                     2690 N Monroe St           Tallahassee    FL 32303
    ## 678                   875 Traditions Way           Tallahassee    FL 32306
    ## 679                5710 University Drive               Tamarac    FL 33321
    ## 680                2004 N Dale Mabry Hwy                 Tampa    FL 33607
    ## 681                  5603 E Hillsborough                 Tampa    FL 33610
    ## 682                    1700 E Fowler Ave                 Tampa    FL 33612
    ## 683                   1577 Bella Cruz Dr          The Villages    FL 32159
    ## 684                      3500 Chaney Hwy            Titusville    FL 32780
    ## 685                   1763 Tamiami Trail                Venice    FL 34293
    ## 686                    8201 N Wickham Rd                 Viera    FL 32940
    ## 687                 2705 Okeechobee Blvd          W Palm Beach    FL 33409
    ## 688                   894 Cypress Garden          Winter Haven    FL 33880
    ## 689                        2684 Lee Road           Winter Park    FL 32789
    ## 690                   3026 Washington Rd               Augusta    GA 30907
    ## 691                 2990 Us Hwy 17 South             Brunswick    GA 31523
    ## 692                  370 Millennium Blvd             Brunswick    GA 31525
    ## 693                 309 Georgia Hwy 49 N                 Byron    GA 31008
    ## 694                        3239 MacOn Rd              Columbus    GA 31907
    ## 695                      2115 E 16th Ave               Cordele    GA 31015
    ## 696                1701 Browns Bridge Rd           Gainesville    GA 30501
    ## 697         4465 Jimmy Lee Smith Parkway                 Hiram    GA 30141
    ## 698                  1125 Bucksnort Road               Jackson    GA 30233
    ## 699                        1222 Boone St             Kingsland    GA 31548
    ## 700        7001 Lake Park - Bellville Rd             Lake Park    GA 31636
    ## 701                       1020 Tanger Rd          Locust Grove    GA 30248
    ## 702               5534 Jimmy Carter Blvd              Norcross    GA 30093
    ## 703                288 Resaca Beach Blvd                Resaca    GA 30735
    ## 704                    3944 Highway 17 S         Richmond Hill    GA 31324
    ## 705                     6801 Abercorn St              Savannah    GA 31405
    ## 706                       1 Gateway Blvd              Savannah    GA 31419
    ## 707                2160 Scenic Highway N            Snellville    GA 30078
    ## 708                    5368 N Henry Blvd           Stockbridge    GA 30281
    ## 709        2925 Lawrenceville Suwanee Rd               Suwanee    GA 30024
    ## 710                3600 Highway 77 South           Union Point    GA 30669
    ## 711                 1328 St Augustine Rd              Valdosta    GA 31602
    ## 712                        205 Lewers St              Honolulu    HI 96815
    ## 713                          430 Kele St         Kahului, Maui    HI 96732
    ## 714                     75-1027 Henry St           Kailua-Kona    HI 96740
    ## 715     45-480 Kaneohe Bay Drive, Ste B1               Kaneohe    HI 96744
    ## 716        4470 Kapolei Parkway, Ste 800               Kapolei    HI 96707
    ## 717                    94-305 Kunia Road               Waipahu    HI 96797
    ## 718                      4175 Highway 21              Brooklyn    IA 52211
    ## 719                   11820 Hickman Road                 Clive    IA 50325
    ## 720                         8200 Nw Blvd             Davenport    IA 52806
    ## 721                     2580 Airport Way                 Boise    ID 83705
    ## 722                   611 N Overland Ave                Burley    ID 83318
    ## 723                   3512 Franklin Road              Caldwell    ID 83605
    ## 724                 4310 Yellowstone Ave              Chubbuck    ID 83202
    ## 725                        2300 N 4th St        Coeur D' Alene    ID 83814
    ## 726                     950 Lindsey Blvd           Idaho Falls    ID 83401
    ## 727               3155 E Fairview Avenue              Meridian    ID 83642
    ## 728      Living Learning Community, Bldg                Moscow    ID 83844
    ## 729                   607 Northside Blvd                 Nampa    ID 83687
    ## 730               1670 Schneidmiller Ave            Post Falls    ID 83854
    ## 731                   291 Pole Line Road            Twin Falls    ID 83301
    ## 732                  140 Racehorse Drive               Alorton    IL 62205
    ## 733                    17 W Algonquin Rd     Arlington Heights    IL 60005
    ## 734                      4330 Fox Valley                Aurora    IL 60504
    ## 735                   1130 S.illinois St            Belleville    IL 62220
    ## 736                      701 Eldorado Rd           Bloomington    IL 61704
    ## 737                    126 Frontage Road           Bolingbrook    IL 60440
    ## 738                     1303 Armour Road           Bourbonnais    IL 60914
    ## 739                 1380 Torrence Avenue          Calumet City    IL 60409
    ## 740                      1915 W Sycamore            Carbondale    IL 62901
    ## 741                   363 S Schmale Road          Carol Stream    IL 60188
    ## 742                    200 Springhill Dr       Carpentersville    IL 60110
    ## 743               702 W Town Center Blvd             Champaign    IL 61822
    ## 744                      522 Ramada Blvd          Collinsville    IL 62234
    ## 745                     1307 N Keller Dr             Effingham    IL 62401
    ## 746           1701 West Evergreen Avenue             Effingham    IL 62401
    ## 747                   2447 Mannheim Road         Franklin Park    IL 60131
    ## 748                         815 Hwy 24 W                Gilman    IL 60938
    ## 749                       27 Junction Dr           Glen Carbon    IL 62034
    ## 750                       6429 Grand Ave                Gurnee    IL 60031
    ## 751                         1086 Lake St          Hanover Park    IL 60133
    ## 752                   7627 W 95th Street         Hickory Hills    IL 60457
    ## 753                   2063 Skokie Valley         Highland Park    IL 60035
    ## 754              1175 North Roselle Road       Hoffman Estates    IL 60169
    ## 755                  13240 S Illinois 47               Huntley    IL 60142
    ## 756                 2531 Plainfield Road                Joliet    IL 60435
    ## 757                       343 Civic Road               Lasalle    IL 61301
    ## 758                   1407 W Hudson Blvd            Litchfield    IL 62056
    ## 759                   364 Richmond Ave E               Mattoon    IL 61938
    ## 760               8349 West North Avenue          Melrose Park    IL 60160
    ## 761              19099 S Old Lagrange Rd                Mokena    IL 60448
    ## 762                     2601 52nd Avenue                Moline    IL 61265
    ## 763                        4610 Broadway             Mt Vernon    IL 62864
    ## 764                     412 S Lincolnway              N Aurora    IL 60542
    ## 765                    115 Radio City Dr               N Pekin    IL 61554
    ## 766                          1615 N Main                Normal    IL 61761
    ## 767                 4609 N Harlem Avenue              Norridge    IL 60706
    ## 768                         737 W Hwy 50             O' Fallon    IL 62269
    ## 769                    9217 S Cicero Ave              Oak Lawn    IL 60453
    ## 770              711 North Harlem Avenue              Oak Park    IL 60302
    ## 771                 17 W 660 22nd Street      Oakbrook Terrace    IL 60181
    ## 772              #20 Orland Square Drive           Orland Park    IL 60462
    ## 773                    975 E Dundee Road              Palatine    IL 60074
    ## 774             1310 E Chain of Rocks Rd         Pontoon Beach    IL 62040
    ## 775                4111 Timberlake Drive         Pontoon Beach    IL 62040
    ## 776                      7375 E State St              Rockford    IL 61108
    ## 777                       1812 W Main St                 Salem    IL 62881
    ## 778                   4824 No River Road         Schiller Park    IL 60176
    ## 779                 16049 Willowbrook Rd          South Beloit    IL 61080
    ## 780                 2905 Adlai Stevenson           Springfield    IL 62703
    ## 781                 2599 W Wabash Avenue           Springfield    IL 62704
    ## 782                    1104 Tuscola Blvd               Tuscola    IL 61953
    ## 783                   4 Interstate Drive              Vandalia    IL 62471
    ## 784                 890 Milwaukee Avenue          Vernon Hills    IL 60061
    ## 785                   959 N Ilinois Rt 3              Waterloo    IL 62298
    ## 786               3890 N Point Boulevard              Waukegan    IL 60085
    ## 787                   7737 S Kingery Hwy           Willowbrook    IL 60527
    ## 788                       1035 James Ave               Bedford    IN 47421
    ## 789                     2160 N Walnut St           Bloomington    IN 47401
    ## 790          943 E Lewis & Clark Parkway           Clarksville    IN 47129
    ## 791                   812 N Conner Court                  Dale    IN 47523
    ## 792                  15876 W Commerce Rd             Daleville    IN 47334
    ## 793                     19501 Elphers Rd            Evansville    IN 47711
    ## 794                        3901 Hwy 41 N            Evansville    IN 47711
    ## 795                       5212 Weston Rd            Evansville    IN 47713
    ## 796                 351 N Green River Rd            Evansville    IN 47715
    ## 797              8695 Jack Carnes Way, N           French Lick    IN 47432
    ## 798                        3150 Grant St                  Gary    IN 46408
    ## 799                     1253 S Park Blvd             Greenwood    IN 46143
    ## 800                      844 East 1250 S             Haubstadt    IN 47639
    ## 801               3231 East 181st Street                Hebron    IN 46341
    ## 802                1720 West Thompson Rd          Indianapolis    IN 46217
    ## 803                         2490 Post Rd          Indianapolis    IN 46219
    ## 804                    4795 Kentucky Ave          Indianapolis    IN 46221
    ## 805               6241 Crawfordsville Rd          Indianapolis    IN 46224
    ## 806                  3512 S Keystone Ave          Indianapolis    IN 46227
    ## 807                     8901 Us 31 South          Indianapolis    IN 46227
    ## 808                       6288 E 82nd St          Indianapolis    IN 46250
    ## 809                   8808 N Michigan Rd          Indianapolis    IN 46268
    ## 810                       3850 Newton St                Jasper    IN 47546
    ## 811                   4260 State Rd 26 E             Lafayette    IN 47905
    ## 812                   1401 Ripley Street          Lake Station    IN 46405
    ## 813                      1524 W South St               Lebanon    IN 46052
    ## 814                        720 E 81st St          Merrillville    IN 46410
    ## 815                   5800 S Franklin St         Michigan City    IN 46360
    ## 816             1989 South State Road 57          Oakland City    IN 47660
    ## 817                       6171 Melton Dr               Portage    IN 46368
    ## 818           175 South Honeyrun Parkway            Scottsburg    IN 47170
    ## 819                         102 Lee Blvd           Shelbyville    IN 46176
    ## 820                    5300 S State Rt 3             Spiceland    IN 47385
    ## 821                   3442 Us Highway 41           Terre Haute    IN 47802
    ## 822                         233 S 3rd St           Terre Haute    IN 47807
    ## 823                        2728 N 6th St             Vincennes    IN 47591
    ## 824                  4982 North 350 East             Whiteland    IN 46184
    ## 825                204 Tuttle Creek Blvd             Manhattan    KS 66502
    ## 826                       9001 W 63rd St               Merriam    KS 66203
    ## 827                        10480 Metcalf         Overland Park    KS 66212
    ## 828                   1500 Wannamaker Rd                Topeka    KS 66604
    ## 829                  3210 Sw Topeka Blvd                Topeka    KS 66611
    ## 830           8030 E Kellogg Drive North               Wichita    KS 67208
    ## 831                       5700 W Kellogg               Wichita    KS 67209
    ## 832                      4024 E Harry St               Wichita    KS 67218
    ## 833                    1748 Us Hwy 231 S            Beaver Dam    KY 42320
    ## 834                15236 State Route 180          Catlettsburg    KY 41129
    ## 835                  2008 N Mulberry Ave         Elizabethtown    KY 42701
    ## 836              4380 South Nashville Rd              Franklin    KY 42134
    ## 837                     1956 Us Hwy 41 N             Henderson    KY 42420
    ## 838                1949 Nicholasville Rd             Lexington    KY 40503
    ## 839                     1880 Newton Pike             Lexington    KY 40511
    ## 840                  434 Eastern Parkway            Louisville    KY 40217
    ## 841                        60 Ruby Drive          Madisonville    KY 42431
    ## 842                     1610 Richmond St             Mt Vernon    KY 40456
    ## 843         18750 Herndon / Oak Grove Rd             Oak Grove    KY 42262
    ## 844                4545 Frederica Street             Owensboro    KY 42301
    ## 845                    200 S Lakeview Dr        Shepherdsville    KY 40165
    ## 846                 4030 Dutchman's Lane           St Matthews    KY 40207
    ## 847                        1670 Waddy Rd                 Waddy    KY 40076
    ## 848                  13019 Walton Verona                Walton    KY 41094
    ## 849                 1832 Old Minden Road          Bossier City    LA 71111
    ## 850                  9510 Greenwood Road             Greenwood    LA 71033
    ## 851                        5910 Veterans              Metairie    LA 70003
    ## 852                    9080 Mansfield Rd            Shreveport    LA 71118
    ## 853                      467 Memorial Dr        Chicopee Falls    MA  1020
    ## 854                      152 Endicott St               Danvers    MA  1923
    ## 855           243 William S Canning Blvd            Fall River    MA  2721
    ## 856                  2173 Northampton St               Holyoke    MA  1040
    ## 857                     160 Winthrop Ave              Lawrence    MA  1843
    ## 858                     38 Commercial Rd            Leominster    MA  1453
    ## 859                       1284 Boston Rd           Springfield    MA  1119
    ## 860                       494 Lincoln St             Worcester    MA  1605
    ## 861                         2095 West St             Annapolis    MD 21401
    ## 862                      1995 E Joppa Rd             Baltimore    MD 21234
    ## 863                     8025 Belair Road             Baltimore    MD 21236
    ## 864                 781 Sunburst Highway             Cambridge    MD 21613
    ## 865                   8424 Baltimore Ave          College Park    MD 20740
    ## 866                1412 Merrit Boulevard               Dundalk    MD 21222
    ## 867                   8493 Ocean Gateway                Easton    MD 21601
    ## 868                     1803 Edgewood Rd              Edgewood    MD 21040
    ## 869                        6400 Ridge Rd            Eldersburg    MD 21784
    ## 870       Fort Meade/meade Main Exchange            Fort Meade    MD 20755
    ## 871                 1207 West Patrick St             Frederick    MD 21701
    ## 872                  100 East Cedar Lane             Fruitland    MD 21826
    ## 873                     6302 Ritchie Hwy           Glen Burnie    MD 21061
    ## 874                        1630 Dual Hwy            Hagerstown    MD 21740
    ## 875                       1365 Mellon Rd               Hanover    MD 21076
    ## 876                  314 Washington Blvd                Laurel    MD 20707
    ## 877                    1239 National Hwy                Lavale    MD 21502
    ## 878         19290 Montgomery Village Ave    Montgomery Village    MD 20886
    ## 879                       1 Center Drive            North East    MD 21901
    ## 880                  1000 Starlite Plaza               Oakland    MD 21550
    ## 881                    11201 Coastal Hwy            Ocean City    MD 21842
    ## 882                      41 Heather Lane            Perryville    MD 21903
    ## 883                     405 Punkin Court             Salisbury    MD 21804
    ## 884                    15061 Georgia Ave         Silver Spring    MD 20853
    ## 885                   2975 Crain Highway               Waldorf    MD 20601
    ## 886                        400 Englar Rd           Westminster    MD 21157
    ## 887                         211 Court St                Auburn    ME  4210
    ## 888                  123 Civic Center Dr               Augusta    ME  4330
    ## 889                       120 Haskell Rd                Bangor    ME  4401
    ## 890                           75 High St             Ellsworth    ME  4605
    ## 891                1091-1107 Congress St              Portland    ME  4102
    ## 892                    1220 Brighton Ave              Portland    ME  4102
    ## 893               1075 Commercial Street              Rockport    ME  4856
    ## 894                    3310 Washtenaw Rd             Ann Arbor    MI 48104
    ## 895                    4785 Beckley Road          Battle Creek    MI 49015
    ## 896                2033 Rawsonville Road            Belleville    MI 48111
    ## 897                2701 East Grand River          East Lansing    MI 48823
    ## 898                     4010 24th Avenue            Ft Gratiot    MI 48059
    ## 899            7800 West Grand River Ave           Grand Ledge    MI 48837
    ## 900                   3127 Plainfield Ne          Grand Rapids    MI 49505
    ## 901                 631 East 24th Street               Holland    MI 49423
    ## 902                    2560 Airport Road               Jackson    MI 49202
    ## 903                     3817 Cork Street             Kalamazoo    MI 49001
    ## 904                 3900 28th Street, Se              Kentwood    MI 49512
    ## 905               7330 W Saginaw Highway               Lansing    MI 48917
    ## 906                  804 Us Highway 27 N              Marshall    MI 49068
    ## 907                 1224 N Dixie Highway                Monroe    MI 48162
    ## 908                    3520 Green Street              Muskegon    MI 49444
    ## 909                        27750 Novi Rd                  Novi    MI 48377
    ## 910                    31831 Gratiot Ave             Roseville    MI 48066
    ## 911                   2435 Tittabawassee               Saginaw    MI 48604
    ## 912                      44945 Woodridge      Sterling Heights    MI 48313
    ## 913                    20565 Eureka Road                Taylor    MI 48180
    ## 914                       878 Munson Ave         Traverse City    MI 49686
    ## 915                      715 44th St S W               Wyoming    MI 49509
    ## 916                     7805 150th St Nw          Apple Valley    MN 55124
    ## 917                8850 University Drive                Blaine    MN 55434
    ## 918                        815 E 78th St           Bloomington    MN 55420
    ## 919                 4209 W American Blvd           Bloomington    MN 55437
    ## 920                  6405 James Circle N       Brooklyn Center    MN 55430
    ## 921                  12950 Aldrich Ave S            Burnsville    MN 55337
    ## 922                  3565 Northdale Blvd           Coon Rapids    MN 55448
    ## 923                  3050 White Bear Ave             Maplewood    MN 55109
    ## 924                    255 N Century Ave             Maplewood    MN 55119
    ## 925                       2700 E Lake St           Minneapolis    MN 55406
    ## 926                      38681 Tanger Dr              N Branch    MN 55056
    ## 927                   9020 Quaday Ave Ne                Otsego    MN 55330
    ## 928                   286 17th Avenue Nw             Rochester    MN 55901
    ## 929                      1226 S Broadway             Rochester    MN 55904
    ## 930                      13450 Rogers Dr                Rogers    MN 55374
    ## 931                      1203 Drury Lane                Arnold    MO 63010
    ## 932                         1105 N 7 Hwy          Blue Springs    MO 64015
    ## 933                        2335 W Hwy 76               Branson    MO 65616
    ## 934                          161 West Dr        Cape Girardeau    MO 63701
    ## 935                    1100 Knipp Street              Columbia    MO 65203
    ## 936                        1717 W 5th St                Eureka    MO 63025
    ## 937                 1096 S.highway Drive                Fenton    MO 63026
    ## 938                        2925 N Hwy 67            Florissant    MO 63033
    ## 939                     5789 Campus Pkwy             Hazelwood    MO 63042
    ## 940                      2500 Sm 291 Hwy          Independence    MO 64055
    ## 941                        3939 S Noland          Independence    MO 64108
    ## 942                 3602 Range Line Road                Joplin    MO 64801
    ## 943                         11570 Hwy Ff                Joplin    MO 64804
    ## 944                   1600 Broadway Blvd           Kansas City    MO 64108
    ## 945                      6887 E Front St           Kansas City    MO 64120
    ## 946               3832 Blue Ridge Cutoff           Kansas City    MO 64133
    ## 947                         3294 Gold Rd          Kingdom City    MO 65262
    ## 948              1300 Lake St Louis Blvd         Lake St Louis    MO 63367
    ## 949                 1320 S Jefferson Ave               Lebanon    MO 65536
    ## 950                   12319 Dorsett Road      Maryland Heights    MO 63043
    ## 951                     703 State Hwy 80              Matthews    MO 63867
    ## 952                        2117 Taney St         N Kansas City    MO 64116
    ## 953                      3001 Lusk Drive                Neosho    MO 64850
    ## 954                   1140 Technology Dr              O'fallon    MO 63368
    ## 955              21921 S East Outer Road              Peculiar    MO 64078
    ## 956                       8810 E 350 Hwy               Raytown    MO 64133
    ## 957               1419 Martin Springs Rd                 Rolla    MO 65401
    ## 958                      2401 W Broadway               Sedalia    MO 65301
    ## 959                  4760 S Campbell Ave           Springfield    MO 65807
    ## 960                    1423 S 5th Street            St Charles    MO 63301
    ## 961                  4015 Frederick Blvd             St Joseph    MO 64506
    ## 962                   11266 Midland Blvd              St Louis    MO 63114
    ## 963                 6441 S Lindberg Blvd              St Louis    MO 63123
    ## 964                      10575 Watson Rd              St Louis    MO 63127
    ## 965                   1515 S Hampton Ave              St Louis    MO 63139
    ## 966                        3939 Veterans             St Peters    MO 63376
    ## 967                   231 St Robert Blvd             St Robert    MO 65584
    ## 968                 825 North Loop Drive              Sullivan    MO 63080
    ## 969                       1 Merlin Drive                  Troy    MO 63379
    ## 970                    #1 Camp Branch Rd             Warrenton    MO 63383
    ## 971              429 E Veterans Memorial             Warrenton    MO 63383
    ## 972                       102 Fore Drive               Wayland    MO 63472
    ## 973                 1839 Highway 1 South            Greenville    MS 38701
    ## 974                        9351 Canal Rd              Gulfport    MS 39503
    ## 975             7541 N Washington Street         Ocean Springs    MS 39564
    ## 976                      685 Hwy 80 East                 Pearl    MS 39208
    ## 977                     500 Highway 12 E            Starkville    MS 39759
    ## 978                        501 N 27th St              Billings    MT 59101
    ## 979                    2010 Overland Ave              Billings    MT 59102
    ## 980                  3715 31st Street Sw           Great Falls    MT 59404
    ## 981                       2922 Brooks St              Missoula    MT 59801
    ## 982              1 Regent Park Boulevard             Asheville    NC 28806
    ## 983                           7135 Nc #4            Battleboro    NC 27809
    ## 984                  581 South Highway 9        Black Mountain    NC 28711
    ## 985                       4541 Sunset Rd             Charlotte    NC 28216
    ## 986                        516 Tyvola Rd             Charlotte    NC 28217
    ## 987      University of North Carolina At             Charlotte    NC 28223
    ## 988              8031 Concord Mills Blvd               Concord    NC 28027
    ## 989               7021 Highway 751, #901                Durham    NC 27707
    ## 990                      5505 Raeford Rd          Fayetteville    NC 28304
    ## 991                    808 S Memorial Dr            Greenville    NC 27834
    ## 992                1043 Jimmie Kerr Road             Haw River    NC 27258
    ## 993                       1524 Dabney Dr             Henderson    NC 27536
    ## 994                    1550 Four Seasons        Hendersonville    NC 28792
    ## 995                   2207 N Marine Blvd          Jacksonville    NC 28546
    ## 996            1800 Princeton-Kenly Road                 Kenly    NC 27542
    ## 997                        975 S Main St          Kernersville    NC 27284
    ## 998                   101 Wintergreen Dr             Lumberton    NC 28358
    ## 999                 378 West Plaza Drive           Mooresville    NC 28117
    ## 1000              1209 Burkemount Avenue             Morganton    NC 28655
    ## 1001                4380 Fayetteville Rd               Raleigh    NC 27603
    ## 1002                 3215 Wake Forest Rd               Raleigh    NC 27609
    ## 1003                          55 Jr Road                 Selma    NC 27576
    ## 1004                   2025 E Dixon Blvd                Shelby    NC 28150
    ## 1005                    2020 Highway 172          Sneads Ferry    NC 28460
    ## 1006             1493 Us Hwy 74-A Bypass              Spindale    NC 28160
    ## 1007                    103 Sedgehill Dr           Thomasville    NC 27360
    ## 1008                 1201 S College Road            Wilmington    NC 28403
    ## 1009           2806 Raleigh Rd Parkway W                Wilson    NC 27893
    ## 1010                        405 S 7th St              Bismarck    ND 58504
    ## 1011                    4437 13th Ave Sw                 Fargo    ND 58103
    ## 1012                  3770 32nd Avenue S           Grand Forks    ND 58201
    ## 1013                     2701 S Broadway                 Minot    ND 58701
    ## 1014                    3333 Ramada Road          Grand Island    NE 68801
    ## 1015            15010 South State Hwy 31                Gretna    NE 68028
    ## 1016                  3400 Newberry Road          North Platte    NE 69101
    ## 1017                  201 Chuck Wagon Rd              Ogallala    NE 69153
    ## 1018                      3509 S 84th St                 Omaha    NE 68124
    ## 1019                   34 Gusabel Avenue                Nashua    NH  3063
    ## 1020                92 Cluff Crossing Rd                 Salem    NH  3079
    ## 1021                 261 Plainfield Road             W Lebanon    NH  3784
    ## 1022                 1286 St Georges Ave                Avenel    NJ  7001
    ## 1023                      221 Us Hwy 130            Bordentown    NJ  8505
    ## 1024              326 Slapes Corner Road        Carney's Point    NJ  8069
    ## 1025         1410 Blackwood Clementon Rd             Clementon    NJ  8021
    ## 1026                         752 Hwy #18        East Brunswick    NJ  8816
    ## 1027              242 E White Horse Pike              Galloway    NJ  8205
    ## 1028                       335 Tilton Rd            Northfield    NJ  8225
    ## 1029                       5550 Route 42          Turnersville    NJ  8012
    ## 1030                   1001 W Landis Ave              Vineland    NJ  8360
    ## 1031                    3513 S Delsea Dr              Vineland    NJ  8360
    ## 1032                   930 S White Sands            Alamogordo    NM 88310
    ## 1033                   9911 Avalon Rd Nw           Albuquerque    NM 87105
    ## 1034          1620 Towne Center Lane S.e           Albuquerque    NM 87106
    ## 1035              5201 San Antonio Drive           Albuquerque    NM 87109
    ## 1036              2400 San Mateo Blvd Ne           Albuquerque    NM 87110
    ## 1037                11261 Menaul Blvd Ne           Albuquerque    NM 87112
    ## 1038                  1602 Coors Road Nw           Albuquerque    NM 87121
    ## 1039                          254 Us 550            Bernalillo    NM 87004
    ## 1040                     810 W Pierce St              Carlsbad    NM 88220
    ## 1041                    3501 N Prince St                Clovis    NM 88101
    ## 1042                      120 N Platinum                Deming    NM 88030
    ## 1043                   60 State Road 344              Edgewood    NM 87013
    ## 1044                       600 Scott Ave            Farmington    NM 87401
    ## 1045                       3810 E Hwy 66                Gallup    NM 87301
    ## 1046                        836 N Us 491                Gallup    NM 87301
    ## 1047                      1700 Sidney Dr                Grants    NM 87020
    ## 1048            5114 N Lovington Highway                 Hobbs    NM 88240
    ## 1049                    1 Giant Crossing             Jamestown    NM 87347
    ## 1050                       740 S Main St            Las Cruces    NM 88001
    ## 1051       3901 Bataan Memorial Hwy West            Las Cruces    NM 88012
    ## 1052                   11 Old Highway 70             Lordsburg    NM 88045
    ## 1053              1875 Emilio Lopez Loop             Los Lunas    NM 87031
    ## 1054                      430 Clayton Rd                 Raton    NM 87740
    ## 1055                      2200 N Main St               Roswell    NM 88220
    ## 1056                      26137 W Hwy 70         Ruidoso Downs    NM 88346
    ## 1057                   3004 Cerrillos Rd              Santa Fe    NM 87507
    ## 1058                      2255 N Date St Truth or Consequences    NM 87901
    ## 1059                  2021 S Mountain Rd             Tucumcari    NM 88401
    ## 1060                    900 Hwy 95 North                Beatty    NV 89003
    ## 1061                    2299 N Carson St           Carson City    NV 89706
    ## 1062               2405 Moutain City Hwy                  Elko    NV 89801
    ## 1063                   480 Truck Inn Way               Fernley    NV 89408
    ## 1064              1201 W Warm Springs Rd             Henderson    NV 89014
    ## 1065                           1 Main St                  Jean    NV 89019
    ## 1066                     5585 Simmons St             Las Vegas    NV 89031
    ## 1067                  450 Fremont Street             Las Vegas    NV 89101
    ## 1068              6300 W Charleston #110             Las Vegas    NV 89102
    ## 1069             3330 W Tropicana Avenue             Las Vegas    NV 89103
    ## 1070                    5045 W Tropicana             Las Vegas    NV 89103
    ## 1071               1826 S Las Vegas Blvd             Las Vegas    NV 89104
    ## 1072               3001 S Las Vegas Blvd             Las Vegas    NV 89109
    ## 1073                3081 S Maryland Park             Las Vegas    NV 89109
    ## 1074               3397 S Las Vegas Blvd             Las Vegas    NV 89109
    ## 1075               3771 S Las Vegas Blvd             Las Vegas    NV 89109
    ## 1076                   310 N Nellis Blvd             Las Vegas    NV 89110
    ## 1077                   8000 W Sahara Ave             Las Vegas    NV 89117
    ## 1078                2380 E Tropicana Ave             Las Vegas    NV 89119
    ## 1079               7200 S Las Vegas Blvd             Las Vegas    NV 89119
    ## 1080                5318 Boulder Highway             Las Vegas    NV 89122
    ## 1081                  9320 S Eastern Ave             Las Vegas    NV 89123
    ## 1082               7341 W Lake Mead Blvd             Las Vegas    NV 89128
    ## 1083               7071 W Craig Rd, #101             Las Vegas    NV 89129
    ## 1084                9310 W Tropicana Ave             Las Vegas    NV 89147
    ## 1085               Nellis Air Force Base             Las Vegas    NV 89191
    ## 1086                    2700 S Casino Dr              Laughlin    NV 89029
    ## 1087                       3230 Losee Rd           N Las Vegas    NV 89030
    ## 1088                     4280 W Craig Rd           N Las Vegas    NV 89031
    ## 1089                2400 North Rancho Dr           N Las Vegas    NV 89130
    ## 1090                240 S Nevada Hwy 160               Pahrump    NV 89048
    ## 1091              31900 S Las Vegas Blvd                 Primm    NV 89019
    ## 1092                   1100 E Plumb Lane                  Reno    NV 89502
    ## 1093                     680 N Wells Ave                  Reno    NV 89512
    ## 1094                      205 Nugget Ave                Sparks    NV 89431
    ## 1095                       114 Wolf Road                Albany    NY 12205
    ## 1096                     3920 Maple Road               Amherst    NY 14226
    ## 1097                       176 Grant Ave                Auburn    NY 13021
    ## 1098                       364 W Main St               Batavia    NY 14020
    ## 1099                 1250 Upper Front St            Binghamton    NY 13901
    ## 1100                805 Pennsylvania Ave              Brooklyn    NY 11207
    ## 1101                   2215 Delaware Ave               Buffalo    NY 14216
    ## 1102                        4445 Main St               Buffalo    NY 14226
    ## 1103                  5300 W Genessee St              Camillus    NY 13031
    ## 1104                    160 Eastern Blvd           Canandaigua    NY 14424
    ## 1105                371 Old Country Road           Carle Place    NY 11514
    ## 1106                 255 Centereach Mall            Centereach    NY 11720
    ## 1107                     4610 Genesee St           Cheektowaga    NY 14225
    ## 1108                 7873 Brewerton Road                Cicero    NY 13212
    ## 1109                 8484 Allegheny Road                 Corfu    NY 14036
    ## 1110                          1 River St              Cortland    NY 13045
    ## 1111                         126 Troy Rd           E Greenbush    NY 12061
    ## 1112               4 Perinton Hills Mall              Fairport    NY 14450
    ## 1113              963 Hempstead Turnpike       Franklin Square    NY 11010
    ## 1114                   10390 Bennet Road              Fredonia    NY 14063
    ## 1115                 4240 Lakeville Road               Geneseo    NY 14454
    ## 1116                  813 Canandaigua Rd                Geneva    NY 14456
    ## 1117                        5092 Camp Rd               Hamburg    NY 14075
    ## 1118                       701 Mohawk St              Herkimer    NY 13350
    ## 1119                      950 Chemung St            Horseheads    NY 14845
    ## 1120                       323 Elmira Rd                Ithaca    NY 14850
    ## 1121                  8710 Northern Blvd       Jackson Heights    NY 11372
    ## 1122                     4757 Transit Rd             Lancaster    NY 14043
    ## 1123             697 Troy-Schenectady Rd                Latham    NY 12110
    ## 1124                   5699 Transit Road              Lockport    NY 14094
    ## 1125                  411 Route 211 East            Middletown    NY 10940
    ## 1126                   201 Lawrence Road            N Syracuse    NY 13212
    ## 1127                8548 Seneca Turnpike          New Hartford    NY 13413
    ## 1128                       150 Nassau St              New York    NY 10038
    ## 1129                         1283 Rt 300              Newburgh    NY 12550
    ## 1130                         443 Main St         Niagara Falls    NY 14301
    ## 1131                  8020 Niagara Falls         Niagara Falls    NY 14304
    ## 1132                  1143 Deer Park Ave         North Babylon    NY 11703
    ## 1133                1078 Glenwood Avenue                Oneida    NY 13421
    ## 1134                   4979 State Hwy 23               Oneonta    NY 13820
    ## 1135             3165 South Western Blvd          Orchard Park    NY 14127
    ## 1136                 118 Victory Highway          Painted Post    NY 14870
    ## 1137                       248 Quaker Rd            Queensbury    NY 12804
    ## 1138                  911 Jefferson Road             Rochester    NY 14623
    ## 1139                   2890 W Ridge Road             Rochester    NY 14626
    ## 1140                      200 S James St                  Rome    NY 13440
    ## 1141                     468 Louden Road      Saratoga Springs    NY 12866
    ## 1142                     60 Nott Terrace           Schenectady    NY 12308
    ## 1143                  6591 Thompson Road              Syracuse    NY 13206
    ## 1144               103 Elwood Davis Road              Syracuse    NY 13212
    ## 1145                      3414 Erie Blvd              Syracuse    NY 13214
    ## 1146                    180 N Genesee St                 Utica    NY 13501
    ## 1147            4024 Vestal Parkway East                Vestal    NY 13850
    ## 1148               7503 Main St - Fisher                Victor    NY 14564
    ## 1149                     1881 Ridge Road              W Seneca    NY 14224
    ## 1150                     1142 Arsenal St             Watertown    NY 13601
    ## 1151                       1681 Home Ave                 Akron    OH 44310
    ## 1152                 2943 S Arlington Rd                 Akron    OH 44312
    ## 1153                900 N Leavitt Avenue               Amherst    OH 44001
    ## 1154                  738 State Rt 250 E               Ashland    OH 44805
    ## 1155                    2349 Center Road            Austinburg    OH 44010
    ## 1156                   4927 Mahoning Ave            Austintown    OH 44515
    ## 1157                       420 E Main St             Beaverdam    OH 45808
    ## 1158                 7735 State Rd Rt 37             Berkshire    OH 43074
    ## 1159            154 Boardman-Canfield Rd          Boardman Twp    OH 44512
    ## 1160              2031 Southgate Parkway             Cambridge    OH 43725
    ## 1161                     4646 Tuscarawas                Canton    OH 44708
    ## 1162     Case Western Reserve University             Cleveland    OH 44106
    ## 1163                6850 Brook Park Road             Cleveland    OH 44129
    ## 1164                 4331 W 150th Street             Cleveland    OH 44135
    ## 1165           3025 Olentangy River Road              Columbus    OH 43202
    ## 1166              1136 South Main Street                Dayton    OH 45409
    ## 1167             Wright State University                Dayton    OH 45435
    ## 1168                    640 Tillotson St                Elyria    OH 44035
    ## 1169               1051 Interstate Court               Findlay    OH 45840
    ## 1170                   1750 Cedar Street               Fremont    OH 43420
    ## 1171              6207 Wilson Mills Road      Highland Heights    OH 44143
    ## 1172                     2226 North Main               Hubbard    OH 44425
    ## 1173                    6100 Rockside Rd          Independence    OH 44131
    ## 1174                 9935 State Route 41        Jeffersonville    OH 43128
    ## 1175       720 N Lexington-Springmill Rd             Mansfield    OH 44906
    ## 1176                  1050 Mt Vernon Ave                Marion    OH 43302
    ## 1177                    3105 Medina Road       Medina Township    OH 44256
    ## 1178                  7700 Mentor Avenue                Mentor    OH 44060
    ## 1179                     10480 Baltimore           Millersport    OH 43046
    ## 1180               1256 W High Extension      New Philadelphia    OH 44663
    ## 1181               3960 Everhard Road Nw          North Canton    OH 44709
    ## 1182                   25912 Lorain Road         North Olmsted    OH 44070
    ## 1183                         8111 Day Dr                 Parma    OH 44129
    ## 1184                      26415 Warns Rd            Perrysburg    OH 43551
    ## 1185                      1122 Buck Road              Rossford    OH 43460
    ## 1186                     3321 Milan Road              Sandusky    OH 44870
    ## 1187                 68296 Red Roof Lane        St Clairsville    OH 43950
    ## 1188                  9449 State Road 14           Streetsboro    OH 44241
    ## 1189                 315 W Market Street                Tiffin    OH 44883
    ## 1190                  6920 W Central Ave                Toledo    OH 43615
    ## 1191                 34725 Euclid Avenue            Willoughby    OH 44094
    ## 1192                    4020 Belmont Ave            Youngstown    OH 44505
    ## 1193         Youngstown State University            Youngstown    OH 44555
    ## 1194                       10 Airport Rd            Zanesville    OH 43701
    ## 1195                     2705 W Broadway               Ardmore    OK 73401
    ## 1196                    1255 W Gentry St              Checotah    OK 74426
    ## 1197                       1100 E 2nd St                Edmond    OK 73034
    ## 1198              2703 S Country Club Rd               El Reno    OK 73036
    ## 1199                     600 S 69 Bypass             Mcalester    OK 74501
    ## 1200               635 South 32nd Street              Muskogee    OK 74401
    ## 1201               315 S Meridian Avenue         Oklahoma City    OK 73108
    ## 1202                     3030 South I-35         Oklahoma City    OK 73129
    ## 1203                   3130 Douglas Blvd         Oklahoma City    OK 73150
    ## 1204                 1617 Sw 74th Street         Oklahoma City    OK 73159
    ## 1205               385 Mid America Drive                 Pryor    OK 74361
    ## 1206                2400 South 4th Route                 Sayre    OK 73662
    ## 1207                     4903 N Harrison               Shawnee    OK 74801
    ## 1208              45 North Sheridan Road                 Tulsa    OK 74115
    ## 1209              121 North 129 East Ave                 Tulsa    OK 74116
    ## 1210                   3435 Se Spicer Dr                Albany    OR 97321
    ## 1211                     1369 Se 1st Ave                 Canby    OR 97013
    ## 1212                    15815 Se 82nd Dr             Clackamas    OR 97015
    ## 1213                    3652 Glenwood Dr                Eugene    OR 97403
    ## 1214                  115 Ne Morgan Lane           Grants Pass    OR 97526
    ## 1215                   12101 Se 82nd Ave          Happy Valley    OR 97086
    ## 1216                      2265 Hwy 395 S             Hermiston    OR 97838
    ## 1217                       2947 S 6th St         Klamath Falls    OR 97603
    ## 1218                     2604 Island Ave             La Grande    OR 97850
    ## 1219                    2260 Biddle Road               Medford    OR 97504
    ## 1220                    800 John Long Rd               Oakland    OR 97462
    ## 1221                  76 E Goodfellow St               Ontario    OR 97914
    ## 1222                     610 Tutuilla Rd             Pendleton    OR 97801
    ## 1223                     10428 S E Stark              Portland    OR 97216
    ## 1224               11950 N Center Street              Portland    OR 97217
    ## 1225            8787 Sw Scholls Ferry Rd              Portland    OR 97223
    ## 1226                   425 Ne Hassalo St              Portland    OR 97232
    ## 1227                       350 W Harvard              Roseburg    OR 97470
    ## 1228                     3155 Ryan Dr Se                 Salem    OR 97301
    ## 1229                   3680 Market St Ne                 Salem    OR 97301
    ## 1230                       987 Kruse Way           Springfield    OR 97477
    ## 1231                      1710 W 6th Ave            The Dalles    OR 97058
    ## 1232              2230 Main Avenue North             Tillamook    OR 97141
    ## 1233                    2919 Newberg Hwy              Woodburn    OR 97071
    ## 1234                1871 Catasauqua Road             Allentown    PA 18109
    ## 1235                  220 Park Hills Plz               Altoona    PA 16602
    ## 1236             4276 Business Route 220               Bedford    PA 15522
    ## 1237                    855 Rostraver Rd          Belle Vernon    PA 15012
    ## 1238                    131 Papermill Rd            Bloomsburg    PA 17815
    ## 1239                   16414 Lincoln Hwy            Breezewood    PA 15533
    ## 1240                  246 Allegheny Blvd            Brookville    PA 15825
    ## 1241                1501 Harrisburg Pike              Carlisle    PA 17015
    ## 1242                   1071 Wayne Avenue          Chambersburg    PA 17201
    ## 1243                 5321 Baltimore Pike       Clifton Heights    PA 19018
    ## 1244                 1346 Old Freedom Rd    Cranberry Township    PA 16066
    ## 1245               47 Industrial Highway             Essington    PA 19029
    ## 1246                      200 West Drive            Greensburg    PA 15601
    ## 1247                            84 Rt 93              Hazleton    PA 18202
    ## 1248                     2079 E State St             Hermitage    PA 16148
    ## 1249                    9174 Lincoln Hwy                 Irwin    PA 15642
    ## 1250                     3156 Elton Road             Johnstown    PA 15904
    ## 1251                   2204 Columbia Ave             Lancaster    PA 17603
    ## 1252            640 East Lincoln Highway             Langhorne    PA 19047
    ## 1253                       3870 Route 30               Latrobe    PA 15650
    ## 1254                  5505 Carlisle Pike         Mechanicsburg    PA 17050
    ## 1255           5609 Nittany Valley Drive             Mill Hall    PA 17751
    ## 1256           3980 William Penn Highway           Monroeville    PA 15146
    ## 1257                1829 Lincoln Highway          N Versailles    PA 15137
    ## 1258                 209 Tarentum Bridge        New Kensington    PA 15068
    ## 1259                    1623 Oliver Road           New Milford    PA 18834
    ## 1260                   2180 Greentree Rd            Pittsburgh    PA 15220
    ## 1261                   271 Clairton Blvd            Pittsburgh    PA 15236
    ## 1262                    7319 McKnight Rd            Pittsburgh    PA 15237
    ## 1263                    2701 Freeport Rd            Pittsburgh    PA 15238
    ## 1264                449 Pennsylvania 315              Pittston    PA 18640
    ## 1265         410 Scranton Carbondale Hwy              Scranton    PA 18508
    ## 1266             973 N Susquehanna Trail           Selinsgrove    PA 17870
    ## 1267                 122 Fitz Henry Road              Smithton    PA 15479
    ## 1268                  1860 N Atherton St         State College    PA 16801
    ## 1269            2200 Lebanon Church Road             W Mifflin    PA 15122
    ## 1270                  1395 W Chestnut St            Washington    PA 15301
    ## 1271                       488 Kidder St          Wilkes-Barre    PA 18702
    ## 1272                       1716 E 3rd St          Williamsport    PA 17701
    ## 1273                     1199 Louck Road                  York    PA 17404
    ## 1274      785 Centre of New England Blvd              Coventry    RI  2816
    ## 1275                  1177 Reservoir Ave              Cranston    RI  2920
    ## 1276                   1450 Hartford Ave              Johnston    RI  2919
    ## 1277             44 Dowling Village Blvd          N Smithfield    RI  2896
    ## 1278                 444 Quaker Lane, #5               Warwick    RI  2886
    ## 1279              3401 Clemson Boulevard              Anderson    SC 29621
    ## 1280                  1011 N Mountain St            Blacksburg    SC 29702
    ## 1281               115 Sloan Garden Road       Boiling Springs    SC 29316
    ## 1282                   5901 Fairfield Dr              Columbia    SC 29203
    ## 1283                   342 Harbison Blvd              Columbia    SC 29212
    ## 1284                   2521 Wade Hampton            Greenville    SC 29615
    ## 1285                     800 S Kings Hwy          Myrtle Beach    SC 29577
    ## 1286                    124 Loyola Drive          Myrtle Beach    SC 29588
    ## 1287            2280 Ashley Phosphate Rd          N Charleston    SC 29406
    ## 1288                730 Highway 17 South        N Myrtle Beach    SC 29582
    ## 1289             5270 International Blvd      North Charleston    SC 29418
    ## 1290                  2435 Mt Holly Road             Rock Hill    SC 29730
    ## 1291                 2306 Reidville Road           Spartanburg    SC 29301
    ## 1292                     113 Motel Drive             St George    SC 29477
    ## 1293            1200 Us Highway 17 North        Surfside Beach    SC 29575
    ## 1294                    2514 Sunset Blvd         West Columbia    SC 29169
    ## 1295                 3536 Point South Dr              Yemassee    SC 29945
    ## 1296               2206 North Lacross Dr            Rapid City    SD 57701
    ## 1297                      4001 E 10th St           Sioux Falls    SD 57103
    ## 1298                   5201 Granite Lane           Sioux Falls    SD 57107
    ## 1299              1011 Paul Huff Highway             Cleveland    TN 37312
    ## 1300                   1420 Hwy 96 North              Fairview    TN 37062
    ## 1301                  28 San Pebble Road               Jackson    TN 38305
    ## 1302                         800 Watt Rd             Knoxville    TN 37932
    ## 1303                  7065 Winchester Rd               Memphis    TN 38125
    ## 1304                        2480 Parkway          Pigeon Forge    TN 37863
    ## 1305                        3716 Parkway          Pigeon Forge    TN 37863
    ## 1306                       120 East I-20               Abilene    TX 79601
    ## 1307                 3314 S Clack Street               Abilene    TX 79601
    ## 1308            209 Central Expressway N                 Allen    TX 75013
    ## 1309                      1710 I-40 East              Amarillo    TX 79103
    ## 1310                   2116 S Georgia St              Amarillo    TX 79109
    ## 1311              9601 I-40 East Exit 76              Amarillo    TX 79111
    ## 1312             3001 Mountain Pass Blvd               Anthony    TX 79821
    ## 1313                   839 N Watson Road             Arlington    TX 76011
    ## 1314                 4820 W Sublett Road             Arlington    TX 76017
    ## 1315                       4928 S Cooper             Arlington    TX 76017
    ## 1316                         1601 Ih35 N                Austin    TX 78702
    ## 1317                2320 S Interregional                Austin    TX 78704
    ## 1318          7619 E Ben White Boulevard                Austin    TX 78741
    ## 1319                     7100 North I-35                Austin    TX 78752
    ## 1320                   1876 East Freeway               Baytown    TX 77521
    ## 1321                     7207 Garth Road               Baytown    TX 77521
    ## 1322                       1710 E 3rd St            Big Spring    TX 79720
    ## 1323                    435 W Bandera St                Boerne    TX 78006
    ## 1324             2951 State Hwy 36 South               Brenham    TX 77833
    ## 1325                        204 S Waller            Brookshire    TX 77423
    ## 1326                   1875 N Expressway           Brownsville    TX 78520
    ## 1327                6131 Paredes Line Rd           Brownsville    TX 78526
    ## 1328        2890 Harvey Mitchell Parkway                 Bryan    TX 77807
    ## 1329                  868 Ne Asbury Blvd              Burleson    TX 76028
    ## 1330                      17400 N Hwy 20                Canton    TX 75103
    ## 1331                        1058 Us 59 S              Carthage    TX 75633
    ## 1332         16851 Interstate Highway 20                 Cisco    TX 76437
    ## 1333                       607 Texas Ave       College Station    TX 77840
    ## 1334                   2243 Stoneside Rd                Conroe    TX 77303
    ## 1335                    8000 S I-35 East               Corinth    TX 76205
    ## 1336                  5165 Interstate 37        Corpus Christi    TX 78408
    ## 1337                         2931 Hwy 77        Corpus Christi    TX 78410
    ## 1338              4918 S Padre Island Dr        Corpus Christi    TX 78411
    ## 1339                  2801 South Hwy 287             Corsicana    TX 75110
    ## 1340                   26050 Highway 290               Cypress    TX 77429
    ## 1341                4400 N Central Expwy                Dallas    TX 75206
    ## 1342                1415 Medical Dist Dr                Dallas    TX 75207
    ## 1343                2030 Market Ctr Blvd                Dallas    TX 75207
    ## 1344              1641 N Westmoreland Rd                Dallas    TX 75211
    ## 1345         8233 East R.l Thornton Frwy                Dallas    TX 75228
    ## 1346               2425 Walnut Hill Lane                Dallas    TX 75229
    ## 1347               10433 N Central Expwy                Dallas    TX 75231
    ## 1348                7120 S Cockrell Hill                Dallas    TX 75236
    ## 1349               7425 Bonnie View Road                Dallas    TX 75241
    ## 1350                 13689 N Central Exp                Dallas    TX 75243
    ## 1351                    9009 Skillman Rd                Dallas    TX 75243
    ## 1352                 4567 Frankford Road                Dallas    TX 75287
    ## 1353                4007 N Interstate 35                Denton    TX 76207
    ## 1354          2301 West University Drive              Edinburg    TX 78539
    ## 1355                 1305 E Monte Cristo              Edinburg    TX 78542
    ## 1356                      6144 Gateway E               El Paso    TX 79905
    ## 1357                     510 Zaragosa Rd               El Paso    TX 79907
    ## 1358                   4690 Woodrow Bean               El Paso    TX 79924
    ## 1359                        9567 Dyer St               El Paso    TX 79924
    ## 1360                    6650 Montana Ave               El Paso    TX 79925
    ## 1361             1301 North Horizon Blvd               El Paso    TX 79927
    ## 1362                     11045 Gateway W               El Paso    TX 79935
    ## 1363                   11315 Montwood Dr               El Paso    TX 79936
    ## 1364    1613 Pleasanton Road, Suite B-10    El Paso - Ft Bliss    TX 79906
    ## 1365                      301 North I-45                 Ennis    TX 75119
    ## 1366                  1015 Spur 350 West                Euless    TX 76039
    ## 1367                    3228 Se Loop 820           Forest Hill    TX 76140
    ## 1368    Hood Main Store/clear Creek Main             Fort Hood    TX 76544
    ## 1369                      19212 Gulf Fwy           Friendswood    TX 77546
    ## 1370            Dallas Parkway & Main St                Frisco    TX 75033
    ## 1371                  4400 South Freeway              Ft Worth    TX 76115
    ## 1372                6737 Camp Bowie Blvd              Ft Worth    TX 76116
    ## 1373                     4601 S Hulen Rd              Ft Worth    TX 76132
    ## 1374                     6480 North Frwy              Ft Worth    TX 76137
    ## 1375                   1410 Seawall Blvd             Galveston    TX 77550
    ## 1376                12733 Interstate 635               Garland    TX 75041
    ## 1377                 457 W Interstate 30               Garland    TX 75043
    ## 1378                 408 Westchase Drive         Grand Prairie    TX 75052
    ## 1379                   1015 West Main St       Gun Barrel City    TX 75156
    ## 1380             4703 S Expressway 77/83             Harlingen    TX 78550
    ## 1381                      100 Cottonwood             Hempstead    TX 77445
    ## 1382                 100 Us Highway 79 S             Henderson    TX 75652
    ## 1383                   2316 Southmore Rd               Houston    TX 77004
    ## 1384                 7300 Washington Ave               Houston    TX 77007
    ## 1385                   2120 No Loop West               Houston    TX 77018
    ## 1386                       3332 S Loop W               Houston    TX 77025
    ## 1387                     11901 E Freeway               Houston    TX 77029
    ## 1388     1104 N Sam Houston Parkway East               Houston    TX 77032
    ## 1389                  12501 Gulf Freeway               Houston    TX 77034
    ## 1390                   9810 Gulf Freeway               Houston    TX 77034
    ## 1391                 11320 North Freeway               Houston    TX 77037
    ## 1392                    12862 Nw Freeway               Houston    TX 77040
    ## 1393                    9766-B Katy Frwy               Houston    TX 77055
    ## 1394                  2735 Bay Area Blvd               Houston    TX 77058
    ## 1395                    12697 Gessner Rd               Houston    TX 77064
    ## 1396                     13031 Fm 1960 W               Houston    TX 77065
    ## 1397      6711 Sam Houston Parkway South               Houston    TX 77072
    ## 1398                        8199 Sw Frwy               Houston    TX 77074
    ## 1399                 12405 Westheimer Rd               Houston    TX 77077
    ## 1400                      925 N Wilcrest               Houston    TX 77079
    ## 1401                   6060 Hillcroft St               Houston    TX 77081
    ## 1402                  8401 Westheimer Rd               Houston    TX 77083
    ## 1403                        4999 Hwy 6 N               Houston    TX 77084
    ## 1404                   6969 Gulf Freeway               Houston    TX 77087
    ## 1405            7707 Steubner Airline Rd               Houston    TX 77088
    ## 1406                 15919 North Freeway               Houston    TX 77090
    ## 1407                   1760 West Fm 1960               Houston    TX 77090
    ## 1408                       11099 Nw Frwy               Houston    TX 77092
    ## 1409                        19450 Hwy 59                Humble    TX 77338
    ## 1410                         6920 Fm1960                Humble    TX 77346
    ## 1411        9655 N Sam Houston Parkway E                Humble    TX 77396
    ## 1412                      3018 W 11th St            Huntsville    TX 77340
    ## 1413                   4115 West Airport                Irving    TX 75062
    ## 1414                    8120 Esters Blvd                Irving    TX 75063
    ## 1415                   1702 S Jackson St          Jacksonville    TX 75766
    ## 1416                         11710 Ih 35               Jarrell    TX 76537
    ## 1417                       1579 Fry Road                  Katy    TX 77449
    ## 1418                    2405 Tex Mati Dr                  Katy    TX 77494
    ## 1419                     6920 S Fry Road                  Katy    TX 77494
    ## 1420            2200 S Washington Street               Kaufman    TX 75142
    ## 1421          209 Sidney Barker St South             Kerrville    TX 78028
    ## 1422       4315 Texas State Hwy 42 North               Kilgore    TX 75662
    ## 1423                 1108 S Fort Hood Rd               Killeen    TX 76541
    ## 1424                   22671 Eastex Frwy              Kingwood    TX 77339
    ## 1425                       928 Hwy 146 S              La Porte    TX 77571
    ## 1426                 401-443 This Way St          Lake Jackson    TX 77566
    ## 1427             3543 Jim Wright Freeway            Lake Worth    TX 76135
    ## 1428                   3600 Santa Ursula                Laredo    TX 78041
    ## 1429                1011 Beltway Parkway                Laredo    TX 78045
    ## 1430               2940 South Highway 45           League City    TX 77573
    ## 1431             2267 S Stemmons Freeway            Lewisville    TX 75067
    ## 1432                        12816 Ih35 N              Live Oak    TX 78233
    ## 1433                   3126 S Eastman Rd              Longview    TX 75602
    ## 1434                        607 Avenue Q               Lubbock    TX 79401
    ## 1435                       4718 Slide Rd               Lubbock    TX 79414
    ## 1436                        6111 Fm 1488              Magnolia    TX 77354
    ## 1437            Highway 360 & E Broad St             Mansfield    TX 76063
    ## 1438                     1110 So 10th St               Mcallen    TX 78501
    ## 1439                    401 E Nolana Ave               Mcallen    TX 78504
    ## 1440           1615 N Central Expressway              Mckinney    TX 75070
    ## 1441                11511 W Airport Blvd         Meadows Place    TX 77477
    ## 1442                          114 Hwy 80              Mesquite    TX 75149
    ## 1443                4004 Emporium Circle              Mesquite    TX 75150
    ## 1444                     3701 W Wall Ave               Midland    TX 79703
    ## 1445                  116 South Shary Rd               Mission    TX 78572
    ## 1446                        6131 Hwy 6 S         Missouri City    TX 77459
    ## 1447                        298 W Fm 544                Murphy    TX 75094
    ## 1448                    8025 Glenview Dr      N Richland Hills    TX 76180
    ## 1449               2615 N Stallings Dr N           Nacogdoches    TX 75964
    ## 1450                   I-30 & Highway 82            New Boston    TX 75570
    ## 1451                    1348 North Ih 35         New Braunfels    TX 78130
    ## 1452                        23412 Sh 242             New Caney    TX 77357
    ## 1453                       105 W 42nd St                Odessa    TX 79764
    ## 1454                      7112 I-10 West                Orange    TX 77632
    ## 1455              2350 West Oak St, #100             Palestine    TX 75801
    ## 1456                    3040 Ne Loop 286                 Paris    TX 75460
    ## 1457                    4125 Spencer Hwy              Pasadena    TX 77504
    ## 1458                   909 West Pasadena              Pasadena    TX 77506
    ## 1459                 11221 Broadway Blvd              Pearland    TX 77581
    ## 1460                     100 E Pinehurst                 Pecos    TX 79772
    ## 1461                     1830 No Central                 Plano    TX 75074
    ## 1462                   1305 Preston Road                 Plano    TX 75093
    ## 1463        408 N I-35 East Service Road               Red Oak    TX 75154
    ## 1464                       4756 E Hwy 83       Rio Grande City    TX 78582
    ## 1465                   Hwy 114 & Hwy 170               Roanoke    TX 76262
    ## 1466              670 East Interstate 30              Rockwall    TX 75032
    ## 1467                    27960 Sw Freeway             Rosenburg    TX 77471
    ## 1468                    2700 North Ih-35            Round Rock    TX 78681
    ## 1469                      201 W Hwy I-30            Royse City    TX 75189
    ## 1470                      427 W Avenue I            San Angelo    TX 76901
    ## 1471              4510 Fredericksburg Rd           San Antonio    TX 78201
    ## 1472                   903 E Commerce St           San Antonio    TX 78205
    ## 1473                      6801 Blanco Rd           San Antonio    TX 78216
    ## 1474                        6000 N Panam           San Antonio    TX 78218
    ## 1475                    2210 Loop 410 Se           San Antonio    TX 78220
    ## 1476                7122 Interstate 35-S           San Antonio    TX 78224
    ## 1477                6859 Highway 90 West           San Antonio    TX 78227
    ## 1478                      9550 I-10 West           San Antonio    TX 78230
    ## 1479                 13635 San Pedro Ave           San Antonio    TX 78232
    ## 1480                      6854 Ingram Rd           San Antonio    TX 78238
    ## 1481                    7138 Nw Loop 410           San Antonio    TX 78238
    ## 1482                1815 North Foster Rd           San Antonio    TX 78244
    ## 1483                           6703 Fm78           San Antonio    TX 78244
    ## 1484                       17455 Ih 35 N               Schertz    TX 78154
    ## 1485                   400 N Highway 175            Seagoville    TX 75159
    ## 1486                     1100 Padre Blvd    South Padre Island    TX 78597
    ## 1487                        6504 Fm 2920                Spring    TX 77379
    ## 1488                     7720 Louetta Rd                Spring    TX 77379
    ## 1489                        20015 I-45 N                Spring    TX 77388
    ## 1490                  1422 State Hwy 6 S            Sugar Land    TX 77478
    ## 1491                         1102 Fm 148               Terrell    TX 75160
    ## 1492                      1201 Hwy 146 N            Texas City    TX 77590
    ## 1493                       14010 Fm 2920               Tomball    TX 77375
    ## 1494                        101 N Fm 707                   Tye    TX 79563
    ## 1495                    3244 Gentry Pkwy                 Tyler    TX 75702
    ## 1496                     7601 Navarro St              Victoria    TX 77904
    ## 1497                          709 N I-35                  Waco    TX 76705
    ## 1498                 2409 South New Road                  Waco    TX 76711
    ## 1499                     1416 W Expwy 83               Weslaco    TX 78596
    ## 1500                    10367 Highway 59               Wharton    TX 77488
    ## 1501                 1206 Central Frwy N         Wichita Falls    TX 76301
    ## 1502                      4301 Kemp Blvd         Wichita Falls    TX 76308
    ## 1503                      201 N Hwy I-45                Wilmer    TX 75172
    ## 1504                     46000 I-10 East                Winnie    TX 77665
    ## 1505                    28669 I-45 North             Woodlands    TX 77381
    ## 1506                     255 N 1100 West            Cedar City    UT 84720
    ## 1507           1605 East Saddleback Blvd            Lake Point    UT 84074
    ## 1508                         991 N 400 W                Layton    UT 84041
    ## 1509                   101 N 1200th East                  Lehi    UT 84043
    ## 1510                      1045 N Main St                 Logan    UT 84341
    ## 1511                        401 W 7200 S               Midvale    UT 84047
    ## 1512                       7051 S 1300 E               Midvale    UT 84047
    ## 1513                       989 N Hwy 191                  Moab    UT 84532
    ## 1514                        420 W 4500 S                Murray    UT 84123
    ## 1515                      1597 S Main St                 Nephi    UT 84648
    ## 1516                1250 Washington Blvd                 Ogden    UT 84404
    ## 1517                      485 N State St                  Orem    UT 84057
    ## 1518                        1680 N 200 W                 Provo    UT 84601
    ## 1519                5805 S Harrison Blvd               S Ogden    UT 84403
    ## 1520                     1525 S State St                Salina    UT 84654
    ## 1521                         250 W 500 S        Salt Lake City    UT 84101
    ## 1522                        2025 S 900 W        Salt Lake City    UT 84104
    ## 1523                  1701 West N Temple        Salt Lake City    UT 84116
    ## 1524               11575 South 4000 West          South Jordan    UT 84095
    ## 1525                      592 Kirby Lane          Spanish Fork    UT 84660
    ## 1526                       1460 N 1750 W           Springville    UT 84663
    ## 1527                      1215 S Main St             St George    UT 84770
    ## 1528                  155 N 1000 East St             St George    UT 84770
    ## 1529                       925 N Main St                Tooele    UT 84074
    ## 1530                      2341 W Main St             Tremonton    UT 84337
    ## 1531                   2097 W Highway 40                Vernal    UT 84078
    ## 1532                      1172 W 2100 St            West Haven    UT 84401
    ## 1533                   7214 Richmond Hwy            Alexandria    VA 22306
    ## 1534                     1401 Tintern St            Chesapeake    VA 23320
    ## 1535             12441 Redwater Creek Rd               Chester    VA 23831
    ## 1536                 1530 Rest Church Rd           Clear Brook    VA 22624
    ## 1537                       801 South Ave      Colonial Heights    VA 23834
    ## 1538                      2861 Dale Blvd             Dale City    VA 22193
    ## 1539                 16102 Theme Parkway               Doswell    VA 23047
    ## 1540                       10473 Lee Hwy               Fairfax    VA 22030
    ## 1541                5325 Jefferson Davis        Fredericksburg    VA 22408
    ## 1542               139 Factory Outlet Dr           Ft Chiswell    VA 24360
    ## 1543                 1040 W Mercury Blvd               Hampton    VA 23666
    ## 1544                   5111 Oaklawn Blvd              Hopewell    VA 23860
    ## 1545                      8201 Sudley Rd              Manassas    VA 20109
    ## 1546                 250 Conicville Blvd            Mt Jackson    VA 22842
    ## 1547                 6598 W Broad Street              Richmond    VA 23230
    ## 1548            11161 Research Plaza Way              Richmond    VA 23236
    ## 1549                     3429 Orange Ave               Roanoke    VA 24012
    ## 1550              24279 Roger Clark Blvd           Ruther Glen    VA 22546
    ## 1551                      2039 W Main St                 Salem    VA 24153
    ## 1552                 10404 Blue Star Hwy           Stony Creek    VA 23882
    ## 1553                       119 Hite Lane             Strasburg    VA 22657
    ## 1554                 3337 Virginia Beach        Virginia Beach    VA 23452
    ## 1555                      198 Newtown Rd        Virginia Beach    VA 23462
    ## 1556              7323 Comfort Inn Drive             Warrenton    VA 20187
    ## 1557                       409 Bypass Rd          Williamsburg    VA 23185
    ## 1558                    1601 Martinsburg            Winchester    VA 22603
    ## 1559                13775 Jeff Davis Hwy            Woodbridge    VA 22191
    ## 1560                   3249 Chapman Road            Wytheville    VA 24382
    ## 1561           361 S Main St, Us Rt 7 So               Rutland    VT  5701
    ## 1562                    730 Shelburne Rd      South Burlington    VT  5403
    ## 1563                      418 W Heron St              Aberdeen    WA 98520
    ## 1564                2202 State Rt 530 Ne             Arlington    WA 98223
    ## 1565                    521 Auburn Way S                Auburn    WA 98002
    ## 1566                   2233 148th Ave Ne              Bellevue    WA 98007
    ## 1567                    161 Telegraph Rd            Bellingham    WA 98226
    ## 1568             20805 State Route 410 E           Bonney Lake    WA 98391
    ## 1569                22833 Bothell Hwy Se               Bothell    WA 98021
    ## 1570                     5004 Kitsap Way             Bremerton    WA 98312
    ## 1571                  1052a Harrison Ave             Centralia    WA 98531
    ## 1572                  118 Interstate Ave              Chehalis    WA 98532
    ## 1573                    8431 244th St Sw               Edmonds    WA 98026
    ## 1574                    2903 Pacific Ave               Everett    WA 98201
    ## 1575                       132 128 St Sw               Everett    WA 98204
    ## 1576                  8417 Evergreen Way               Everett    WA 98208
    ## 1577                     2132 S 320th St           Federal Way    WA 98003
    ## 1578                        34726 S 16th           Federal Way    WA 98003
    ## 1579                     5720 Barrett Rd              Ferndale    WA 98248
    ## 1580                  5110 Pacific Hwy E                  Fife    WA 98424
    ## 1581                        110 Kelso Dr                 Kelso    WA 98626
    ## 1582                    2801 W Kennewick             Kennewick    WA 99336
    ## 1583              1246 North Central Ave                  Kent    WA 98032
    ## 1584                      12106 Ne 124th              Kirkland    WA 98034
    ## 1585               108 College Street Se                 Lacey    WA 98516
    ## 1586                    4109 196th Place              Lynnwood    WA 98036
    ## 1587                    18824 State Rt 2                Monroe    WA 98272
    ## 1588                1590 E Yonezawa Blvd            Moses Lake    WA 98837
    ## 1589                100 East College Way             Mt Vernon    WA 98273
    ## 1590                  626 S Hill Park Dr              Puyallup    WA 98373
    ## 1591                16515 Meridian Ave E              Puyallup    WA 98375
    ## 1592               4750 Ne Lk Washington                Renton    WA 98056
    ## 1593         1301a George Washington Way              Richland    WA 99354
    ## 1594                   17206 Pacific Hwy                Seatac    WA 98188
    ## 1595             18623 Pacific Hwy South                Seatac    WA 98188
    ## 1596                   2762 4th Avenue S               Seattle    WA 98134
    ## 1597               14821 First Ave South               Seattle    WA 98168
    ## 1598         E 301 Wallace-Kneeland Blvd               Shelton    WA 98584
    ## 1599              20420 Mountain Highway              Spanaway    WA 98387
    ## 1600                       North 6 Pines               Spokane    WA 99206
    ## 1601                  3525 N Division St               Spokane    WA 99207
    ## 1602                   2022 N Argonne Rd               Spokane    WA 99212
    ## 1603                  3711 S Geiger Blvd               Spokane    WA 99224
    ## 1604                        5924 6th Ave                Tacoma    WA 98406
    ## 1605             10802 Pacific Ave South                Tacoma    WA 98444
    ## 1606                       8614 S Hosmer                Tacoma    WA 98444
    ## 1607               11755 Pacific Highway                Tacoma    WA 98499
    ## 1608                    6112 100th St Sw                Tacoma    WA 98499
    ## 1609                      2603 Rudkin Rd             Union Gap    WA 98903
    ## 1610                1337 N Wenatchee Ave             Wenatchee    WA 98801
    ## 1611                     1012 N 40th Ave                Yakima    WA 98908
    ## 1612            3470 West College Avenue              Appleton    WI 54914
    ## 1613                          780 Hwy 54     Black River Falls    WI 54615
    ## 1614                2894 S Oneida Street             Green Bay    WI 54303
    ## 1615                     4695 S 108th St            Greenfield    WI 53228
    ## 1616                   1000 Gateway Blvd                Hudson    WI 54016
    ## 1617                     2020 Milton Ave            Janesville    WI 53545
    ## 1618         1090 Wisconsin Dells Pkwy S           Lake Delton    WI 53940
    ## 1619                   1798 Thierer Road               Madison    WI 53704
    ## 1620                   433 S Gammon Road               Madison    WI 53719
    ## 1621                  611 Gateway Avenue               Mauston    WI 53948
    ## 1622                  1827 N Broadway St             Menomonie    WI 54751
    ## 1623                4925 S Howell Avenue             Milwaukee    WI 53207
    ## 1624                  3801 S 27th Street             Milwaukee    WI 53221
    ## 1625                   7822 W Capitol Dr             Milwaukee    WI 53222
    ## 1626              8001 W Brown Deer Road             Milwaukee    WI 53223
    ## 1627                     1201 E Broadway                Monona    WI 53716
    ## 1628              9650 South 20th Street             Oak Creek    WI 53154
    ## 1629          2801 N Grandview Boulevard              Pewaukee    WI 53072
    ## 1630              5501 Washington Avenue                Racine    WI 53406
    ## 1631                  1500 County Hwy Xx            Rothschild    WI 54474
    ## 1632                    310 E McCoy Blvd                 Tomah    WI 54660
    ## 1633                  2300 W St Paul Ave              Waukesha    WI 53188
    ## 1634             11155 West North Avenue             Wauwatosa    WI 53226
    ## 1635                         531 Hwy 128                Wilson    WI 54027
    ## 1636                   600 S Frontage Rd       Wisconsin Dells    WI 53965
    ## 1637                        489 Emily Dr            Clarksburg    WV 26301
    ## 1638                        Rr3 Box 3188                Keyser    WV 26726
    ## 1639                   100 Hornbeck Road            Morgantown    WV 26508
    ## 1640               4220 Hospitality Lane                Casper    WY 82609
    ## 1641                   2250 Etchepare Dr              Cheyenne    WY 82007
    ## 1642                     #1 Flying J Way               Rawlins    WY 82301
    ## 1643                   650 Stagecoach Dr          Rock Springs    WY 82901
    ## 1644               793 W. Bel Air Avenue            \nAberdeen    MD 21001
    ## 1645                     3018 CatClaw Dr             \nAbilene    TX 79606
    ## 1646                   3501 West Lake Rd             \nAbilene    TX 79601
    ## 1647                 184 North Point Way             \nAcworth    GA 30102
    ## 1648          2828 East Arlington Street                 \nAda    OK 74820
    ## 1649                 14925 Landmark Blvd             \nAddison    TX 75254
    ## 1650                909 East Frontage Rd               \nAlamo    TX 78516
    ## 1651            2116 Yale Blvd Southeast         \nAlbuquerque    NM 87106
    ## 1652     7439 Pan American Fwy Northeast         \nAlbuquerque    NM 87109
    ## 1653          2011 Menaul Blvd Northeast         \nAlbuquerque    NM 87107
    ## 1654       5241 San Antonio Dr Northeast         \nAlbuquerque    NM 87109
    ## 1655             6101 Iliff Rd Northwest         \nAlbuquerque    NM 87121
    ## 1656                6116 West Calhoun Dr          \nAlexandria    LA 71303
    ## 1657                   2400 East Main St               \nAlice    TX 78332
    ## 1658       1220 North Central Expressway               \nAllen    TX 75013
    ## 1659                        1165 Hwy 67W            \nAlvarado    TX 76009
    ## 1660                   880 South Loop 35               \nAlvin    TX 77511
    ## 1661             1708 Interstate 40 East            \nAmarillo    TX 79103
    ## 1662             9305 East Interstate 40            \nAmarillo    TX 79118
    ## 1663                   2108 S Coulter St            \nAmarillo    TX 79106
    ## 1664                  1752 Clementine St             \nAnaheim    CA 92802
    ## 1665                  3501 Minnesota Dr.           \nAnchorage    AK 99503
    ## 1666                        131 River Rd             \nAndover    MA  1810
    ## 1667                   1012 NE 1st Place             \nAndrews    TX 79714
    ## 1668               2400 West Mulberry St            \nAngleton    TX 77515
    ## 1669               3800 West College Ave            \nAppleton    WI 54914
    ## 1670                      1502 Woerz Way             \nArdmore    OK 73401
    ## 1671                 825 North Watson Rd           \nArlington    TX 76011
    ## 1672                 94 Business Park Dr              \nArmonk    NY 10504
    ## 1673               2207 West Main Street             \nArtesia    NM 88210
    ## 1674                 6909 Atascocita Rd.              \nHumble    TX 77346
    ## 1675                   1200 Virginia Ave             \nAtlanta    GA 30344
    ## 1676             4820 Massachusetts Blvd             \nAtlanta    GA 30337
    ## 1677                 1350 North Point Dr          \nAlpharetta    GA 30022
    ## 1678                  1184 Dogwood Dr SE             \nConyers    GA 30012
    ## 1679                  1000 Linnenkohl Dr        \nDouglasville    GA 30134
    ## 1680             2370 Stephens Center Dr              \nDuluth    GA 30096
    ## 1681         2535 Chantilly Dr Northeast             \nAtlanta    GA 30324
    ## 1682       2415 Paces Ferry Rd Southeast             \nAtlanta    GA 30339
    ## 1683          6260 Peachtree Dunwoody Rd             \nAtlanta    GA 30328
    ## 1684           575 Old Holcomb Bridge Rd             \nRoswell    GA 30076
    ## 1685                    600 Bullsboro Dr              \nNewnan    GA 30265
    ## 1686                   3581 Cameron Pkwy         \nStockbridge    GA 30281
    ## 1687                  446 Southbridge St              \nAuburn    MA  1501
    ## 1688           225 6th Street South East              \nAuburn    WA 98002
    ## 1689                  3020 Washington Rd             \nAugusta    GA 30907
    ## 1690            7625 East Ben White Blvd              \nAustin    TX 78741
    ## 1691                    300 East 11th St              \nAustin    TX 78701
    ## 1692              10701 Lakeline Mall Dr              \nAustin    TX 78717
    ## 1693           1010 East Whitestone Blvd          \nCedar Park    TX 78613
    ## 1694             11901 North Mopac Expwy              \nAustin    TX 78759
    ## 1695                     7622 I-35 North              \nAustin    TX 78752
    ## 1696               1603 East Oltorf Blvd              \nAustin    TX 78741
    ## 1697                     2004 North I-35          \nRound Rock    TX 78681
    ## 1698                     4200 I-35 South              \nAustin    TX 78745
    ## 1699              4424 South Mopac Expwy              \nAustin    TX 78735
    ## 1700            5812 N Interstate Hwy 35              \nAustin    TX 78751
    ## 1701              8858 Spectrum Park Way         \nBakersfield    CA 93308
    ## 1702                   3232 Riverside Dr         \nBakersfield    CA 93308
    ## 1703                1734 West Nursery Rd           \nLinthicum    MD 21090
    ## 1704            200 West Saratoga Street           \nBaltimore    MD 21201
    ## 1705                   4 Philadelphia Ct           \nBaltimore    MD 21237
    ## 1706                    6323 Ritchie Hwy         \nGlen Burnie    MD 21061
    ## 1707                      8200 Park Road             \nBatavia    NY 14020
    ## 1708                       1617 Oneal Ln         \nBaton Rouge    LA 70816
    ## 1709               2720 N Westport Drive          \nPort Allen    LA 70767
    ## 1710                     10555 Reiger Rd         \nBaton Rouge    LA 70809
    ## 1711          2333 South Acadian Thruway         \nBaton Rouge    LA 70808
    ## 1712                         5300 7th St            \nBay City    TX 77414
    ## 1713                      5820 Walden Rd            \nBeaumont    TX 77707
    ## 1714                       2062 HWY 59 E            \nBeeville    TX 78102
    ## 1715                6445 Jackrabbit Lane            \nBelgrade    MT 59714
    ## 1716               1063 W. Bakerview Rd.          \nBellingham    WA 98226
    ## 1717                   229 West Loop 121              \nBelton    TX 76513
    ## 1718                  61200 South Hwy 97                \nBend    OR 97702
    ## 1719               1001 Southeast Walton         \nBentonville    AR 72712
    ## 1720                  920 University Ave            \nBerkeley    CA 94710
    ## 1721                     1102 IH 20 West          \nBig Spring    TX 79720
    ## 1722                5720 S. Frontage Rd.            \nBillings    MT 59101
    ## 1723                   957 Cedar Lake Rd              \nBiloxi    MS 39532
    ## 1724                     581 Harry L. Dr        \nJohnson City    NY 13790
    ## 1725              513 Cahaba Park Circle          \nBirmingham    AL 35242
    ## 1726                  60 State Farm Pkwy            \nHomewood    AL 35209
    ## 1727            120 Riverchase Pkwy East          \nBirmingham    AL 35244
    ## 1728              2240 North 12th Street            \nBismarck    ND 58501
    ## 1729        3402 North West Jefferson St        \nBlue Springs    MO 64015
    ## 1730                  2613 S. Vista Ave.               \nBoise    ID 83705
    ## 1731              7965 W. Emerald Street               \nBoise    ID 83704
    ## 1732              28600 Trails Edge Blvd      \nBonita Springs    FL 34134
    ## 1733               165 Hwy 105 Extension               \nBoone    NC 28607
    ## 1734                    309 Preston Blvd        \nBossier City    LA 71111
    ## 1735                      23 Cummings St          \nSomerville    MA  2145
    ## 1736                    14221 Highway 90              \nBoutte    LA 70039
    ## 1737                1953 Mel Browning St       \nBowling Green    KY 42104
    ## 1738                       620 Nikles Dr             \nBozeman    MT 59715
    ## 1739                        215 Dande Rd             \nBrandon    MS 39042
    ## 1740                2950 Wood Ridge Blvd             \nBrenham    TX 77833
    ## 1741                       108 Texas Ave         \nBridge City    TX 77611
    ## 1742                        2000 10th St          \nBridgeport    TX 76426
    ## 1743                     451 W Albany St        \nBroken Arrow    OK 74012
    ## 1744                  1229 Atlantic Ave.            \nBrooklyn    NY 11216
    ## 1745                         533 3rd Ave            \nBrooklyn    NY 11215
    ## 1746                     1412 Pitkin Ave            \nBrooklyn    NY 11233
    ## 1747                         721 FM 1489          \nBrookshire    TX 77423
    ## 1748                104 Sweetland Avenue           \nBroussard    LA 70518
    ## 1749               5051 North Expressway         \nBrownsville    TX 78520
    ## 1750               103 Market Place Blvd           \nBrownwood    TX 76801
    ## 1751               165 Warren Mason Blvd           \nBrunswick    GA 31520
    ## 1752                   3 Centerpointe Dr            \nLa Palma    CA 90623
    ## 1753                     6619 Transit Rd       \nWilliamsville    NY 14221
    ## 1754               225 East Alsbury Blvd            \nBurleson    TX 76028
    ## 1755                1 Holiday Park Drive               \nButte    MT 59701
    ## 1756                      901 Specht Ave            \nCaldwell    ID 83605
    ## 1757               150 Cracker Barrel Dr             \nCalhoun    GA 30701
    ## 1758              152 Soldiers Colony Rd              \nCanton    MS 39046
    ## 1759     5335 Broadmoor Circle Northwest              \nCanton    OH 44709
    ## 1760                55 Hampton Park Blvd     \nCapitol Heights    MD 20743
    ## 1761         4020 National Parks Highway            \nCarlsbad    NM 88220
    ## 1762                       400 West F St              \nCasper    WY 82601
    ## 1763                        884 Park St.         \nCastle Rock    CO 80109
    ## 1764              1377 South Main Street          \nCedar City    UT 84720
    ## 1765                    1419 N.US Hwy 67          \nCedar Hill    TX 75104
    ## 1766                  1220 Park Place NE        \nCedar Rapids    IA 52402
    ## 1767                      1777 LaRue Dr.       \nCentral Point    OR 97502
    ## 1768                       199 Walker Rd        \nChambersburg    PA 17201
    ## 1769                      1900 Center Dr           \nChampaign    IL 61820
    ## 1770                 11 Ashley Pointe Dr          \nCharleston    SC 29407
    ## 1771                       3127 Sloan Dr           \nCharlotte    NC 28208
    ## 1772                 4900 South Tryon St           \nCharlotte    NC 28217
    ## 1773                     1803 Emmet St N     \nCharlottesville    VA 22901
    ## 1774                   6650 Ringgold Rd.          \nEast Ridge    TN 37412
    ## 1775                 7017 Shallowford Rd         \nChattanooga    TN 37421
    ## 1776                311 Browns Ferry Rd.         \nChattanooga    TN 37419
    ## 1777              5000 New Country Drive              \nHixson    TN 37343
    ## 1778                 7051 McCutcheon Rd.         \nChattanooga    TN 37421
    ## 1779                2410 West Lincolnway            \nCheyenne    WY 82009
    ## 1780                  One South Franklin             \nChicago    IL 60606
    ## 1781                  5688 Northridge Dr              \nGurnee    IL 60031
    ## 1782           4900A S. Lake Shore Drive             \nChicago    IL 60615
    ## 1783              2000 South Lakeside Dr         \nBannockburn    IL 60015
    ## 1784                 1900 East Oakton St   \nElk Grove Village    IL 60007
    ## 1785              1 South 666 Midwest Rd    \nOakbrook Terrace    IL 60181
    ## 1786                  7255 West 183rd St         \nTinley Park    IL 60477
    ## 1787                         855 79th St         \nWillowbrook    IL 60527
    ## 1788                       350 Meijer Dr            \nFlorence    KY 41042
    ## 1789              12150 Springfield Pike          \nSpringdale    OH 45246
    ## 1790                      9918 Escort Dr               \nMason    OH 45040
    ## 1791                     11029 Dowlin Dr          \nCincinnati    OH 45241
    ## 1792               774 S Lynn Riggs Blvd           \nClaremore    OK 74017
    ## 1793                   251 Holiday Drive         \nClarksville    TN 37040
    ## 1794              520 West Bay Area Blvd             \nWebster    TX 77598
    ## 1795                    3301 Ulmerton Rd          \nClearwater    FL 33762
    ## 1796               21338 US Hwy 19 North          \nClearwater    FL 33765
    ## 1797                      5000 Lake Blvd          \nClearwater    FL 33760
    ## 1798             107 East Kilpatrick Ave            \nCleburne    TX 76031
    ## 1799                    4222 West 150 St           \nCleveland    OH 44135
    ## 1800             25105 Country Club Blvd       \nNorth Olmsted    OH 44070
    ## 1801                      6161 Quarry Ln        \nIndependence    OH 44131
    ## 1802                268 East Highland Rd           \nMacedonia    OH 44056
    ## 1803               130 Interstate Dr. NW           \nCleveland    TN 37312
    ## 1804                   1004 Hwy 59 South           \nCleveland    TX 77327
    ## 1805                        1749 Route 9        \nClifton Park    NY 12065
    ## 1806                      265 Rte 3 East             \nClifton    NJ  7014
    ## 1807                   2715 Chapman Road             \nClinton    OK 73601
    ## 1808                4521 North Prince St              \nClovis    NM 88101
    ## 1809             1126 South Hwy 332 West               \nClute    TX 77531
    ## 1810                        1 Hendry Ave         \nCocoa Beach    FL 32931
    ## 1811             1275 North Atlantic Ave         \nCocoa Beach    FL 32931
    ## 1812                  333 W Ironwood Dr.       \nCoeur d Alene    ID 83814
    ## 1813                    1838 Graham Road     \nCollege Station    TX 77845
    ## 1814                       607 Texas Ave     \nCollege Station    TX 77840
    ## 1815                     6 Gateway Drive        \nCollinsville    IL 62234
    ## 1816                       880 East I-20       \nColorado City    TX 79512
    ## 1817                      4385 Sinton Rd    \nColorado Springs    CO 80907
    ## 1818                 8155 N Academy Blvd    \nColorado Springs    CO 80920
    ## 1819                      2750 Geyser Dr    \nColorado Springs    CO 80906
    ## 1820                  7300 Crestmount Rd              \nJessup    MD 20794
    ## 1821                   1538 Horseshoe Dr            \nColumbia    SC 29223
    ## 1822               7333 Garners Ferry Rd            \nColumbia    SC 29209
    ## 1823     2500 Interstate 70 Dr Southwest            \nColumbia    MO 65203
    ## 1824                       2447 Brice Rd        \nReynoldsburg    OH 43068
    ## 1825                6145 Parkcenter Cir               \nDublin    OH 43017
    ## 1826                     101 Carrie Lane            \nColumbus    IN 47201
    ## 1827                    1711 Rollins Way            \nColumbus    GA 31904
    ## 1828               3201 Macon Rd Suite A            \nColumbus    GA 31906
    ## 1829                2919 Warm Springs Rd            \nColumbus    GA 31909
    ## 1830                      5510 Trabue Rd            \nColumbus    OH 43228
    ## 1831                   2427 Hwy 71 South            \nColumbus    TX 78934
    ## 1832                  4006 Sprayberry Ln              \nConroe    TX 77303
    ## 1833                   2350 Sanders Road              \nConway    AR 72032
    ## 1834            1131 South Jefferson Ave          \nCookeville    TN 38501
    ## 1835                  3701 University Dr       \nCoral Springs    FL 33065
    ## 1836           546 South Padre Island Dr      \nCorpus Christi    TX 78405
    ## 1837      14430 South Padre Island Drive      \nCorpus Christi    TX 78418
    ## 1838                     5155 I-37 North      \nCorpus Christi    TX 78408
    ## 1839                10446 IH37 Access Rd      \nCorpus Christi    TX 78410
    ## 1840               201 Buddy Ganem Drive            \nPortland    TX 78374
    ## 1841          6225 South Padre Island Dr      \nCorpus Christi    TX 78412
    ## 1842                       2020 Regal Dr           \nCorsicana    TX 75109
    ## 1843                  677 N. Baylor Ave.             \nCotulla    TX 78014
    ## 1844                    4 Universal Blvd            \nCoventry    RI  2816
    ## 1845                  9159 Access Rd NW            \nCovington    GA 30014
    ## 1846               2054 St. Joseph Drive             \nCullman    AL 35058
    ## 1847                       27130 Hwy 290             \nCypress    TX 77429
    ## 1848                  801 Liberal Street             \nDalhart    TX 79022
    ## 1849                4001 Scots Legacy Dr           \nArlington    TX 76015
    ## 1850      13175 North Central Expressway              \nDallas    TX 75243
    ## 1851       4850 West John Carpenter Frwy              \nIrving    TX 75063
    ## 1852                  302 S. Houston St.              \nDallas    TX 75202
    ## 1853                380 E Palace Parkway       \nGrand Prairie    TX 75050
    ## 1854                      2131 West I-20       \nGrand Prairie    TX 75052
    ## 1855                 1000 Dowdy Ferry Rd            \nHutchins    TX 75141
    ## 1856               2421 Walnut Hill Lane              \nDallas    TX 75229
    ## 1857           4225 North MacArthur Blvd              \nIrving    TX 75038
    ## 1858         8300 John W. Carpenter Fwy.              \nDallas    TX 75247
    ## 1859                  118 East US Hwy 80            \nMesquite    TX 75149
    ## 1860      10001 North Central Expressway              \nDallas    TX 75231
    ## 1861                1310 Marketplace Dr.             \nGarland    TX 75041
    ## 1862                4800 West Plano Pkwy               \nPlano    TX 75093
    ## 1863       4440 North Central Expressway              \nDallas    TX 75206
    ## 1864                      715 College Dr              \nDalton    GA 30720
    ## 1865                      116 Newtown Rd             \nDanbury    CT  6810
    ## 1866               3330 East Kimberly Rd           \nDavenport    IA 52807
    ## 1867               1771 Research Park Dr               \nDavis    CA 95618
    ## 1868                        19 Weller Dr           \nTipp City    OH 45371
    ## 1869 2725 W. International Speedway Blvd       \nDaytona Beach    FL 32114
    ## 1870           918 Beltline Rd Southwest             \nDecatur    AL 35601
    ## 1871                  1405 South Hwy 287             \nDecatur    TX 76234
    ## 1872                      1400 East Blvd           \nDeer Park    TX 77536
    ## 1873             351 West Hillsboro Blvd     \nDeerfield Beach    FL 33441
    ## 1874              100 Southwest 12th Ave     \nDeerfield Beach    FL 33442
    ## 1875                  2005 Veterans Blvd             \nDel Rio    TX 78840
    ## 1876                   4300 East Pine St              \nDeming    NM 88030
    ## 1877             801 US Highway 75 North             \nDenison    TX 75020
    ## 1878                     4465 North I-35              \nDenton    TX 76207
    ## 1879                       6801 Tower Rd              \nDenver    CO 80249
    ## 1880               1011 South Abilene St              \nAurora    CO 80012
    ## 1881                  902 West Dillon Rd          \nLouisville    CO 80027
    ## 1882                  3500 Park Ave West              \nDenver    CO 80216
    ## 1883            1975 South Colorado Blvd              \nDenver    CO 80222
    ## 1884               9009 East Arapahoe Rd   \nGreenwood Village    CO 80112
    ## 1885                  4460 Peoria Street              \nDenver    CO 80239
    ## 1886          3301 Youngfield Service Rd              \nGolden    CO 80401
    ## 1887                  345 West 120th Ave         \nWestminster    CO 80234
    ## 1888               7190 West Hampden Ave            \nLakewood    CO 80227
    ## 1889               7077 South Clinton St   \nGreenwood Village    CO 80112
    ## 1890                    8701 Turnpike Dr         \nWestminster    CO 80030
    ## 1891             1390 Northwest 118th St               \nClive    IA 50325
    ## 1892                 641 North I-35 East              \nDeSoto    TX 75115
    ## 1893                       41211 Ford Rd              \nCanton    MI 48187
    ## 1894                      30847 Flynn Dr             \nRomulus    MI 48174
    ## 1895                      12888 Reeck Rd           \nSouthgate    MI 48195
    ## 1896                      45311 Park Ave               \nUtica    MI 48315
    ## 1897               4105 West Airport Fwy              \nIrving    TX 75062
    ## 1898                        1809 Hwy 121             \nBedford    TX 76021
    ## 1899                     431 Airport Fwy              \nEuless    TX 76040
    ## 1900                552 12th Street West           \nDickinson    ND 58601
    ## 1901           2400 West Wyatt Earp Blvd          \nDodge City    KS 67801
    ## 1902              3593 Ross Clark Circle              \nDothan    AL 36303
    ## 1903                    6275 Dublin Blvd              \nDublin    CA 94568
    ## 1904              101 Travel Center Blvd              \nDublin    GA 31021
    ## 1905               1805 Maple Grove Road              \nDuluth    MN 55811
    ## 1906                1912 South Dumas Ave               \nDumas    TX 79029
    ## 1907              125 Mercury Village Dr             \nDurango    CO 81303
    ## 1908                   417 Criswell Blvd              \nDurant    OK 74701
    ## 1909        4414 Durham Chapel Hill Blvd              \nDurham    NC 27707
    ## 1910                    1910 Westpark Dr              \nDurham    NC 27713
    ## 1911                     2525 E. Main St          \nEagle Pass    TX 78852
    ## 1912                         10150 IH 20            \nEastland    TX 76448
    ## 1913              2112B Emmorton Park Rd            \nEdgewood    MD 21040
    ## 1914                      200 Meline Dr.              \nEdmond    OK 73034
    ## 1915          1103 Avenue of Mid America           \nEffingham    IL 62401
    ## 1916               2303 Junction City Rd           \nEl Dorado    AR 71730
    ## 1917                 6140 Gateway Blvd E             \nEl Paso    TX 79905
    ## 1918                     7620 North Mesa             \nEl Paso    TX 79912
    ## 1919                   9125 Gateway West             \nEl Paso    TX 79925
    ## 1920                   7944 Gateway East             \nEl Paso    TX 79915
    ## 1921             11033 Gateway Blvd West             \nEl Paso    TX 79935
    ## 1922                  7550 Remcon Circle             \nEl Paso    TX 79912
    ## 1923                     210 Commerce Dr       \nElizabethtown    KY 42701
    ## 1924                       2611 E Hwy 66            \nElk City    OK 73644
    ## 1925          101 Crossing Shopping Mall             \nElkview    WV 25071
    ## 1926               540 Saw Mill River Rd            \nElmsford    NY 10523
    ## 1927               1591 Great Basin Blvd                 \nEly    NV 89301
    ## 1928               2930 Eaglecrest Drive             \nEmporia    KS 66801
    ## 1929          4914 W. Owen K Garriott Rd                \nEnid    OK 73703
    ## 1930              110 South Sonoma Trail               \nEnnis    TX 75119
    ## 1931                      7820 Perry Hwy                \nErie    PA 16509
    ## 1932                   155 Day Island Rd              \nEugene    OR 97401
    ## 1933               8015 East Division St          \nEvansville    IN 47715
    ## 1934                  12619 4th Ave West             \nEverett    WA 98204
    ## 1935                        4920 Dale Rd           \nFairbanks    AK 99709
    ## 1936               2540 University Blvd.            \nFairborn    OH 45324
    ## 1937                    316 Pittman Road           \nFairfield    CA 94534
    ## 1938                   38 Two Bridges Rd           \nFairfield    NJ  7004
    ## 1939               1010 West Commerce St           \nFairfield    TX 75840
    ## 1940           4317 Rockaway Beach Blvd.        \nFar Rockaway    NY 11692
    ## 1941                  2355 46th St South               \nFargo    ND 58104
    ## 1942                       675 Scott Ave          \nFarmington    NM 87401
    ## 1943                   720 E. Millsap Rd        \nFayetteville    AR 72703
    ## 1944                  1001 Veterans Blvd              \nFestus    MO 63028
    ## 1945             2015 South Beulah\nBlvd           \nFlagstaff    AZ 86001
    ## 1946              2123 West Lucas Street            \nFlorence    SC 29501
    ## 1947                    1910 10th Street         \nFloresville    TX 78114
    ## 1948                 400 Russell Parkway             \nForsyth    GA 31029
    ## 1949               3709 East Mulberry St        \nFort Collins    CO 80524
    ## 1950               5070 North State Rd 7     \nFort Lauderdale    FL 33319
    ## 1951                   9521 Market Pl Rd          \nFort Myers    FL 33912
    ## 1952            4850 South Cleveland Ave          \nFort Myers    FL 33907
    ## 1953                      6700 Boston St          \nFort Smith    AR 72903
    ## 1954                  1537 North Hwy 285       \nFort Stockton    TX 79735
    ## 1955             3 Miracle Strip Pkwy SW   \nFort Walton Beach    FL 32548
    ## 1956                  8250 Anderson Blvd          \nFort Worth    TX 76120
    ## 1957                      5800 Quebec St          \nFort Worth    TX 76135
    ## 1958                   653 N.E. Loop 820               \nHurst    TX 76053
    ## 1959                      4700 North Fwy          \nFort Worth    TX 76137
    ## 1960                4900 Bryant Irvin Rd          \nFort Worth    TX 76132
    ## 1961                 7310 Calmont Avenue          \nFort Worth    TX 76116
    ## 1962                  190 N. 10th Street              \nFowler    CA 93625
    ## 1963                   2150 W Holiday Rd           \nFrankfort    IN 46041
    ## 1964                   1465 East Main St      \nFredericksburg    TX 78624
    ## 1965                  46200 Landing Pkwy             \nFremont    CA 94538
    ## 1966             5077 North Cornelia Ave              \nFresno    CA 93722
    ## 1967                    330 East Fir Ave              \nFresno    CA 93720
    ## 1968                         2926 Tulare              \nFresno    CA 93721
    ## 1969                       570 Raptor Rd              \nFruita    CO 81521
    ## 1970                 2620 North 26th Ave           \nHollywood    FL 33020
    ## 1971           999 West Cypress Creek Rd     \nFort Lauderdale    FL 33309
    ## 1972              5727 North Federal Hwy     \nFort Lauderdale    FL 33308
    ## 1973                      8101 Peters Rd          \nPlantation    FL 33324
    ## 1974           3800 West Commercial Blvd     \nFort Lauderdale    FL 33309
    ## 1975                  20091 Summerlin Rd          \nFort Myers    FL 33908
    ## 1976                2655 Crossroads Pkwy         \nFort Pierce    FL 34945
    ## 1977                 2902 East Dupont Rd          \nFort Wayne    IN 46825
    ## 1978             3346 Forest Hill Circle           \nFt. Worth    TX 76140
    ## 1979                     1207 Boots Blvd          \nFultondale    AL 35068
    ## 1980          920 Northwest 69th Terrace         \nGainesville    FL 32605
    ## 1981                     4201 North I-35         \nGainesville    TX 76240
    ## 1982                    3880 East Hwy 66              \nGallup    NM 87301
    ## 1983                   1402 Seawall Blvd           \nGalveston    TX 77550
    ## 1984                     821 Stewart Ave         \nGarden City    NY 11530
    ## 1985                         375 E. I-30             \nGarland    TX 75043
    ## 1986                   450 E Boxelder Rd            \nGillette    WY 82718
    ## 1987              101 West BO Gibbs Blvd           \nGlen Rose    TX 76043
    ## 1988               1717 N Merrill Avenue            \nGlendive    MT 59330
    ## 1989                   1631 Water Street            \nGonzales    TX 78629
    ## 1990                     2816 S. Cabelas            \nGonzales    LA 70737
    ## 1991          120 South Cartwright Court      \nGoodlettsville    TN 37072
    ## 1992                 880 Harbor Lakes Dr            \nGranbury    TX 76048
    ## 1993              4051 Garden View Drive         \nGrand Forks    ND 58201
    ## 1994                2761 Crossroads Blvd      \nGrand Junction    CO 81506
    ## 1995             243 Northeast Morgan Ln         \nGrants Pass    OR 97526
    ## 1996                  600 River Dr South         \nGreat Falls    MT 59405
    ## 1997                      1201 Lanada Rd          \nGreensboro    NC 27407
    ## 1998             65 West Orchard Park Dr          \nGreenville    SC 29615
    ## 1999                 1281 South Park Dr.           \nGreenwood    IN 46143
    ## 2000                     3962 Jackpot Rd          \nGrove City    OH 43123
    ## 2001                   210 Heritage Pkwy     \nGun Barrel City    TX 75156
    ## 2002                    406 Heather Road             \nGuthrie    OK 73044
    ## 2003               42126 Veterans Avenue             \nHammond    LA 70403
    ## 2004                 990 Eisenhower Blvd          \nHarrisburg    PA 17111
    ## 2005                 265 N. Hershey Road          \nHarrisburg    PA 17112
    ## 2006             64 Ella Grasso Turnpike       \nWindsor Locks    CT  6096
    ## 2007                      109 Lundy Lane         \nHattiesburg    MS 39401
    ## 2008               701 Washington Street              \nHelena    MT 59601
    ## 2009                  9041 Brighton Road           \nHenderson    CO 80640
    ## 2010                   12000 Mariposa Rd            \nHesperia    CA 92345
    ## 2011            1607 Fairgrove Church Rd             \nConover    NC 28613
    ## 2012                 1513 Old Brandon Rd           \nHillsboro    TX 76645
    ## 2013              1740 E. Oglethorpe Hwy          \nHinesville    GA 31313
    ## 2014            3312 North Lovington Hwy               \nHobbs    NM 88240
    ## 2015                    291 Financial Dr           \nHollister    MO 65672
    ## 2016                345 Griffin Bell Dr.        \nHopkinsville    KY 42240
    ## 2017                    721 Southwest Dr           \nHorn Lake    MS 38637
    ## 2018                 4253 Central Avenue         \nHot Springs    AR 71913
    ## 2019             189 Synergy Center Blvd               \nHouma    LA 70360
    ## 2020                      5215 I-10 East             \nBaytown    TX 77521
    ## 2021          15510 John F. Kennedy Blvd             \nHouston    TX 77032
    ## 2022                   18201 Kenswick Dr              \nHumble    TX 77338
    ## 2023    5520 East Sam Houston Pkwy North             \nHouston    TX 77015
    ## 2024              4424 Westway Park Blvd             \nHouston    TX 77041
    ## 2025                  13290 FM 1960 West             \nHouston    TX 77065
    ## 2026                     930 Normandy St             \nHouston    TX 77015
    ## 2027               2451 Shadow View Lane             \nHouston    TX 77077
    ## 2028                1625 West Loop South             \nHouston    TX 77027
    ## 2029                  4015 Southwest Fwy             \nHouston    TX 77027
    ## 2030                   8776 Airport Blvd             \nHouston    TX 77061
    ## 2031                         415 FM 1960             \nHouston    TX 77073
    ## 2032                  1105 Hwy 146 South            \nLa Porte    TX 77571
    ## 2033                      3636 NASA Rd 1            \nSeabrook    TX 77586
    ## 2034                    24868 I-45 North              \nSpring    TX 77386
    ## 2035                 11130 Northwest Fwy             \nHouston    TX 77092
    ## 2036        9034 West Sam Houston Pkwy N             \nHouston    TX 77064
    ## 2037                  6790 Southwest Fwy             \nHouston    TX 77074
    ## 2038                 12727 Southwest Fwy            \nStafford    TX 77477
    ## 2039                      15225 Katy Fwy             \nHouston    TX 77094
    ## 2040                     10850 Harwin Dr             \nHouston    TX 77072
    ## 2041                   8383 FM 1960 Rd W             \nHouston    TX 77070
    ## 2042                  105 Westchester Rd             \nMadison    AL 35758
    ## 2043        4870 University Dr Northwest          \nHuntsville    AL 35816
    ## 2044                      124 I-45 North          \nHuntsville    TX 77340
    ## 2045    3416 Martin Luther King Jr. Blvd            \nLongview    TX 75602
    ## 2046                2501 South 25th East         \nIdaho Falls    ID 83406
    ## 2047                   2650 Executive Dr        \nIndianapolis    IN 46241
    ## 2048              5316 West Southern Ave        \nIndianapolis    IN 46241
    ## 2049               2251 Manchester Drive          \nPlainfield    IN 46168
    ## 2050            401 E. Washington Street        \nIndianapolis    IN 46204
    ## 2051                        2349 Post Dr        \nIndianapolis    IN 46219
    ## 2052                   3880 West 92nd St        \nIndianapolis    IN 46268
    ## 2053                     5120 Victory Dr        \nIndianapolis    IN 46203
    ## 2054                 3945 W Imperial Hwy           \nInglewood    CA 90303
    ## 2055                204 West Frontage Rd                \nIowa    LA 70647
    ## 2056               14972 Sand Canyon Ave              \nIrvine    CA 92618
    ## 2057                          10 Aero Rd             \nBohemia    NY 11716
    ## 2058                501 South Pearson Rd               \nPearl    MS 39208
    ## 2059                 593 E. Beasley Road             \nJackson    MS 39206
    ## 2060                2370 N. Highland Ave             \nJackson    TN 38305
    ## 2061               4686 Lenoir Ave South        \nJacksonville    FL 32216
    ## 2062                     3199 Hartley Rd        \nJacksonville    FL 32257
    ## 2063               1902 South Jackson St        \nJacksonville    TX 75766
    ## 2064                 1515 South Coast Dr          \nCosta Mesa    CA 92626
    ## 2065             3320 South Rangeline Rd              \nJoplin    MO 64804
    ## 2066                          110 BMT Dr          \nJourdanton    TX 78026
    ## 2067                   34 Fishermans Way             \nJupiter    FL 33477
    ## 2068                    255 Montclair Dr           \nKalispell    MT 59901
    ## 2069             6901 North West 83rd St         \nKansas City    MO 64152
    ## 2070                      9461 Lenexa Dr              \nLenexa    KS 66215
    ## 2071                       2214 Taney St      \nN. Kansas City    MO 64116
    ## 2072              1025 S. State Hwy. 123         \nKarnes City    TX 78118
    ## 2073                      22455 Katy Fwy                \nKaty    TX 77450
    ## 2074                         108 3rd Ave             \nKearney    NE 68845
    ## 2075          560 Greers Chapel Drive NW            \nKennesaw    GA 30144
    ## 2076               2600 South Quillan Pl           \nKennewick    WA 99338
    ## 2077                1940 Sidney Baker St           \nKerrville    TX 78028
    ## 2078             1112 South Fort Hood St             \nKilleen    TX 76541
    ## 2079                      3419 Hotel Way             \nKingman    AZ 86409
    ## 2080                    104 May Creek Dr           \nKingsland    GA 31548
    ## 2081                  10150 Airport Pkwy           \nKingsport    TN 37663
    ## 2082                       2151 S Hwy 77          \nKingsville    TX 78363
    ## 2083                  22790 Highway 59 N            \nKingwood    TX 77339
    ## 2084                       126 Cusick Rd               \nAlcoa    TN 37701
    ## 2085                     1317 Kirby Road           \nKnoxville    TN 37909
    ## 2086                      7534 Conner Rd              \nPowell    TN 37849
    ## 2087                 7210 Saddle Rack St           \nKnoxville    TN 37914
    ## 2088                      511 Albany Dr.              \nKokomo    IN 46901
    ## 2089                   18869 IH-35 North                \nKyle    TX 78640
    ## 2090                       101 E. 500 N.         \nLa Verkin\n    UT 84745
    ## 2091   2100 Northeast Evangeline Thruway           \nLafayette    LA 70501
    ## 2092                    312 Meijer Drive           \nLafayette    IN 47905
    ## 2093                   111 Hoffman Drive            \nLaGrange    GA 30241
    ## 2094             1201 West Prien Lake Rd        \nLake Charles    LA 70601
    ## 2095                    1008 Sampson St.            \nWestlake    LA 70669
    ## 2096                      70 Kaibab Road                \nPage    AZ 86040
    ## 2097               4315 Lakeland Park Dr            \nLakeland    FL 33809
    ## 2098     1024 Lakeland Park Center Drive            \nLakeland    FL 33809
    ## 2099                   25 Eastbrook Road               \nRonks    PA 17572
    ## 2100               7220 Bob Bullock Loop              \nLaredo    TX 78041
    ## 2101               3610 Santa Ursula Ave              \nLaredo    TX 78041
    ## 2102              790 Avenida de Mesilla          \nLas Cruces    NM 88005
    ## 2103                     1500 Hickory Dr          \nLas Cruces    NM 88005
    ## 2104                    3970 Paradise Rd           \nLas Vegas    NV 89169
    ## 2105                      6560 Surrey St           \nLas Vegas    NV 89119
    ## 2106              4288 North Nellis Blvd           \nLas Vegas    NV 89115
    ## 2107                  9570 W. Sahara Ave           \nLas Vegas    NV 89117
    ## 2108              7101 Cascade Valley Ct           \nLas Vegas    NV 89128
    ## 2109            4975 S. Valley View Blvd           \nLas Vegas    NV 89118
    ## 2110                   833 New Loudon Rd              \nLatham    NY 12110
    ## 2111             1408 North West 40th ST              \nLawton    OK 73505
    ## 2112              5249 West Century Blvd         \nLos Angeles    CA 90045
    ## 2113                    140 Dixie Avenue             \nLebanon    TN 37090
    ## 2114                  1308 Entrance Road           \nLeesville    LA 71446
    ## 2115                22769 Three Notch Rd          \nCalifornia    MD 20619
    ## 2116                    100 Canebrake Dr           \nLexington    KY 40509
    ## 2117                    1920 Stanton Way           \nLexington    KY 40511
    ## 2118                  4433 North 27th St             \nLincoln    NE 68521
    ## 2119            204 West Centennial Blvd             \nLindale    TX 75771
    ## 2120                  617 South Broadway         \nLittle Rock    AR 72201
    ## 2121                      4311 Warden Rd   \nNorth Little Rock    AR 72116
    ## 2122                   9 Crossings Court         \nLittle Rock    AR 72205
    ## 2123                408 West Commerce Dr              \nBryant    AR 72022
    ## 2124                  7700 Southfront Rd           \nLivermore    CA 94551
    ## 2125               402 Hwy 59 Loop South          \nLivingston    TX 77351
    ## 2126              4832 Bill Gardner Pkwy        \nLocust Grove    GA 30248
    ## 2127                     853 S Hwy 89/91               \nLogan    UT 84321
    ## 2128               908 East Hawkins Pkwy            \nLongview    TX 75605
    ## 2129                   20 W Pacheco Blvd           \nLos Banos    CA 93635
    ## 2130                      12400 Hwy 72 N              \nLoudon    TN 37774
    ## 2131                   4125 Preston Hwy.          \nLouisville    KY 40213
    ## 2132                    1501 Alliant Ave          \nLouisville    KY 40299
    ## 2133                    1450 Cascade Ave            \nLoveland    CO 80538
    ## 2134                           601 Ave Q             \nLubbock    TX 79401
    ## 2135                      5006 Auburn St             \nLubbock    TX 79416
    ## 2136                     6504 I-27 South             \nLubbock    TX 79412
    ## 2137               4115 Marsha Sharp Fwy             \nLubbock    TX 79407
    ## 2138                      2119 S. 1st St              \nLufkin    TX 75901
    ## 2139                        203 E HWY 90              \nLuling    TX 78648
    ## 2140                       104 N. LHS Dr           \nLumberton    TX 77657
    ## 2141         3320 Candlers Mountain Road           \nLynchburg    VA 24502
    ## 2142            4300 Alderwood Mall Blvd            \nLynnwood    WA 98036
    ## 2143                    4615 Chambers Rd               \nMacon    GA 31206
    ## 2144                    3944 River Pl Dr               \nMacon    GA 31210
    ## 2145                5217 East Terrace Dr             \nMadison    WI 53718
    ## 2146                       6950 Nova Way            \nManassas    VA 20109
    ## 2147                     21 Front Street          \nManchester    NH  3102
    ## 2148                     17 West 32nd St            \nNew York    NY 10001
    ## 2149                     120 Stander Ave           \nMansfield    OH 44903
    ## 2150                1503 Breckenridge Rd           \nMansfield    TX 76063
    ## 2151                      1524 Colony Rd               \nRipon    CA 95366
    ## 2152                   501 Hwy 2147 West        \nMarble Falls    TX 78654
    ## 2153            6015 East End Blvd South            \nMarshall    TX 75672
    ## 2154                        12909 I H 37              \nMathis    TX 78368
    ## 2155      1137 S. George Nigh Expressway           \nMcAlester    OK 74501
    ## 2156                      801 S. Ware Rd             \nMcAllen    TX 78501
    ## 2157              1120 South 10th Street             \nMcAllen    TX 78501
    ## 2158                       100 Mill Road           \nMcDonough    GA 30253
    ## 2159                   6501 Henneman Way            \nMcKinney    TX 75070
    ## 2160                350 Bent Creek Blvd.       \nMechanicsburg    PA 17050
    ## 2161          4510 West New Haven Avenue           \nMelbourne    FL 32904
    ## 2162           7200 George T. Edwards Dr           \nMelbourne    FL 32940
    ## 2163                  2979 Millbranch Rd             \nMemphis    TN 38116
    ## 2164                     6069 Macon Cove             \nMemphis    TN 38134
    ## 2165                   1236 Primacy Pkwy             \nMemphis    TN 38119
    ## 2166               2839 New Brunswick Rd             \nMemphis    TN 38133
    ## 2167                    310 Union Avenue             \nMemphis    TN 38103
    ## 2168             7007 East Expressway 83            \nMercedes    TX 78570
    ## 2169                  800 S Allen Street            \nMeridian    ID 83642
    ## 2170                     1400 Roebuck Dr            \nMeridian    MS 39301
    ## 2171                   8210 Louisiana St        \nMerrillville    IN 46410
    ## 2172 6530 East Superstition Springs Blvd                \nMesa    AZ 85206
    ## 2173                    3501 NW 42nd Ave               \nMiami    FL 33142
    ## 2174             7401 North West 36th St               \nMiami    FL 33166
    ## 2175             8730 North West 27th St               \nMiami    FL 33172
    ## 2176                10821 Caribbean Blvd          \nCutler Bay    FL 33189
    ## 2177            7925 North West 154th St         \nMiami Lakes    FL 33016
    ## 2178            2606 North Loop 250 West             \nMidland    TX 79707
    ## 2179                   4130 West Wall St             \nMidland    TX 79703
    ## 2180                        24 Beaver St             \nMilford    MA  1757
    ## 2181              1839 N Columbia Street       \nMilledgeville    GA 31061
    ## 2182                  7141 South 13th St           \nOak Creek    WI 53154
    ## 2183       5423 North Port Washington Rd            \nGlendale    WI 53217
    ## 2184                    2801 Hillside Dr           \nDelafield    WI 53018
    ## 2185       5110 North Port Washington Rd            \nGlendale    WI 53217
    ## 2186            15300 West Rock Ridge Rd          \nNew Berlin    WI 53151
    ## 2187             20391 West Bluemound Rd          \nBrookfield    WI 53045
    ## 2188             7815 Nicollet Ave South         \nBloomington    MN 55420
    ## 2189             5151 American Blvd West         \nBloomington    MN 55437
    ## 2190                  10420 Wayzata Blvd          \nMinnetonka    MN 55305
    ## 2191               7011 Northland Circle       \nBrooklyn Park    MN 55428
    ## 2192              1605 35th Avenue S. W.               \nMinot    ND 58701
    ## 2193                       805 Travis St             \nMission    TX 78572
    ## 2194               5059 North Reserve St            \nMissoula    MT 59808
    ## 2195                   815 South Main St                \nMoab    UT 84532
    ## 2196                     8946 Sawwood St              \nDaphne    AL 36527
    ## 2197                        6104\nHwy 43             \nSatsuma    AL 36572
    ## 2198                       5170 Motel Ct              \nMobile    AL 36619
    ## 2199      816 West I-65 Service Rd South              \nMobile    AL 36609
    ## 2200                        4909 Sisk Rd              \nSalida    CA 95368
    ## 2201                        5450 27th St              \nMoline    IL 61265
    ## 2202                      1314 West I-20            \nMonahans    TX 79756
    ## 2203                23090 Sunnymead Blvd       \nMoreno Valley    CA 92553
    ## 2204                     2018 Allison St         \nMorgan City    LA 70380
    ## 2205                    17043 Condit Rd.         \nMorgan Hill    CA 95037
    ## 2206                  5000 Gateway Drive          \nMorgantown    WV 26501
    ## 2207                    185 Warbonnet Dr              \nMoscow    ID 83843
    ## 2208                   7001 Hwy 63 North          \nMoss Point    MS 39563
    ## 2209                    5000 Clover Road        \nMount Laurel    NJ  8054
    ## 2210                      1620 Rotan Ave        \nMt. Pleasant    TX 75455
    ## 2211              1701 North 32nd Street            \nMuskogee    OK 74401
    ## 2212                4709 North Kings Hwy        \nMyrtle Beach    SC 29577
    ## 2213                 1561 21st Ave North        \nMyrtle Beach    SC 29577
    ## 2214                  1555 5th Ave South              \nNaples    FL 34102
    ## 2215                   185 Bedzel Circle              \nNaples    FL 34104
    ## 2216                     2345 Atrium Way           \nNashville    TN 37214
    ## 2217                   531 Donelson Pike           \nNashville    TN 37214
    ## 2218            4207 Franklin Commons Ct            \nFranklin    TN 37067
    ## 2219                       4311 Sidco Dr           \nNashville    TN 37204
    ## 2220                     12441 Carson St    \nHawaiian Gardens    CA 90716
    ## 2221                    365 Hwy 46 South       \nNew Braunfels    TX 78130
    ## 2222                         22025 US 59           \nNew Caney    TX 77357
    ## 2223                    130 Limekiln Rd.      \nNew Cumberland    PA 17070
    ## 2224                      400 Sargent Dr           \nNew Haven    CT  6511
    ## 2225                  611A Queen City Dr          \nNew Iberia    LA 70560
    ## 2226                  2610 Williams Blvd              \nKenner    LA 70062
    ## 2227                3100 I-10 Service Rd            \nMetairie    LA 70001
    ## 2228                         301 Camp St         \nNew Orleans    LA 70130
    ## 2229            794 East I-10 Service Rd             \nSlidell    LA 70461
    ## 2230         5900 Veterans Memorial Blvd            \nMetairie    LA 70003
    ## 2231                       50 Terry Pkwy              \nGretna    LA 70056
    ## 2232                 31 West 71st Street            \nNew York    NY 10023
    ## 2233                 304 Belle Hill Road              \nElkton    MD 21921
    ## 2234                45 Southeast 32nd St             \nNewport    OR 97365
    ## 2235             6225 Niagara Falls Blvd       \nNiagara Falls    NY 14304
    ## 2236             1387 North Military Hwy             \nNorfolk    VA 23502
    ## 2237                      192 Newtown Rd      \nVirginia Beach    VA 23462
    ## 2238                  1601B Hwy 17 North  \nNorth Myrtle Beach    SC 29582
    ## 2239                2600 Eagles Wings Pl        \nNorth Platte    NE 69101
    ## 2240            6020 West Hospitality Rd              \nTucson    AZ 85743
    ## 2241                    136 Regency Park             \nOFallon    IL 62269
    ## 2242                 8465 Enterprise Way             \nOakland    CA 94621
    ## 2243                20777 Hesperian Blvd             \nHayward    CA 94541
    ## 2244                    3530 SW 36th Ave               \nOcala    FL 34474
    ## 2245                     106 32nd Street          \nOcean City    MD 21842
    ## 2246              816 North Atlantic Ave       \nDaytona Beach    FL 32118
    ## 2247                     4118 Faudree Rd              \nOdessa    TX 79765
    ## 2248                   4122 Faudree Road              \nOdessa    TX 79765
    ## 2249                 5001 East Bus. I-20              \nOdessa    TX 79761
    ## 2250               3003 West Memorial Rd       \nOklahoma City    OK 73134
    ## 2251                808 S. Meridian Ave.       \nOklahoma City    OK 73108
    ## 2252                5653 Tinker Diagonal        \nMidwest City    OK 73110
    ## 2253                   2140 Riverwalk Dr               \nMoore    OK 73160
    ## 2254                   930 Ed Noble Pkwy              \nNorman    OK 73072
    ## 2255           4829 Northwest Expressway       \nOklahoma City    OK 73132
    ## 2256        11500 West - I 40 Service Rd               \nYukon    OK 73099
    ## 2257                 20570 West 151st St              \nOlathe    KS 66061
    ## 2258      4704 Park Center Ave Northeast               \nLacey    WA 98516
    ## 2259                          1201 Ave H         \nCarter Lake    IA 51510
    ## 2260                3330 North 104th Ave               \nOmaha    NE 68134
    ## 2261                          10760 M St               \nOmaha    NE 68127
    ## 2262             3555 Inland Empire Blvd             \nOntario    CA 91764
    ## 2263                  2721 Hotel Terrace           \nSanta Ana    CA 92705
    ## 2264                   2220 Hwy 62 South              \nOrange    TX 77630
    ## 2265            521 West University Pkwy                \nOrem    UT 84058
    ## 2266                 1100 West 780 North                \nOrem    UT 84057
    ## 2267              7160 North Frontage Rd             \nOrlando    FL 32812
    ## 2268                   7931 Daetwyler Dr             \nOrlando    FL 32812
    ## 2269                 8504 Universal Blvd             \nOrlando    FL 32819
    ## 2270               5825 International Dr             \nOrlando    FL 32819
    ## 2271                 1060 Greenwood Blvd           \nLake Mary    FL 32746
    ## 2272                   2051 Consulate Dr             \nOrlando    FL 32837
    ## 2273                 11805 Research Pkwy             \nOrlando    FL 32826
    ## 2274                     5621 Major Blvd             \nOrlando    FL 32819
    ## 2275                      1886 Rath Lane             \nOshkosh    WI 54902
    ## 2276                      10610 Marty St       \nOverland Park    KS 66212
    ## 2277                  8949 N Garnett Rd.              \nOwasso    OK 74055
    ## 2278                  100 Colonial Drive              \nOxford    AL 36203
    ## 2279        3960 Coleman Crossing Circle             \nPaducah    KY 42001
    ## 2280                 3000 South Loop 256           \nPalestine    TX 75801
    ## 2281                     500 W Harvester               \nPampa    TX 79065
    ## 2282   17710 West Panama City Beach Pkwy   \nPanama City Beach    FL 32413
    ## 2283             7115 Coastal Palms Blvd   \nPanama City Beach    FL 32408
    ## 2284                   1030 East 23rd St         \nPanama City    FL 32405
    ## 2285             3205 Northeast Loop 286               \nParis    TX 75460
    ## 2286               2205 Pasadena Freeway            \nPasadena    TX 77506
    ## 2287    3490 East Sam Houston Pkwy South            \nPasadena    TX 77505
    ## 2288                 2615 Buena Vista Dr         \nPaso Robles    CA 93446
    ## 2289                    9002 Broadway St            \nPearland    TX 77584
    ## 2290                      170 Medical Dr            \nPearsall    TX 78061
    ## 2291   101 South Frontage Road I-20 West               \nPecos    TX 79772
    ## 2292                7750 North Davis Hwy           \nPensacola    FL 32514
    ## 2293                     4389 Venture Dr                \nPeru    IL 61354
    ## 2294                    4607 N Cage Blvd               \nPharr    TX 78577
    ## 2295                   4603 N. Cage Blvd               \nPharr    TX 78577
    ## 2296               53 Industrial Highway           \nEssington    PA 19029
    ## 2297                 15241 South 50th St             \nPhoenix    AZ 85044
    ## 2298               4929 West McDowell Rd             \nPhoenix    AZ 85035
    ## 2299                  902 West Grove Ave                \nMesa    AZ 85210
    ## 2300                  2510 W Greenway Rd             \nPhoenix    AZ 85023
    ## 2301                 8888 East Shea Blvd          \nScottsdale    AZ 85260
    ## 2302                   911 South 48th St               \nTempe    AZ 85281
    ## 2303         2725 North Black Canyon Hwy             \nPhoenix    AZ 85009
    ## 2304                16321 North 83rd Ave              \nPeoria    AZ 85382
    ## 2305                        219 Emert St        \nPigeon Forge    TN 37863
    ## 2306          125 Community Center Drive        \nPigeon Forge    TN 37863
    ## 2307                 2410 S Broadway St.           \nPittsburg    KS 66762
    ## 2308                8507 University Blvd       \nMoon Township    PA 15108
    ## 2309                         18 Pratt Rd          \nPlainfield    CT  6374
    ## 2310         6624 Communications Parkway               \nPlano    TX 75024
    ## 2311               7901 Southwest 6th St          \nPlantation    FL 33324
    ## 2312                       16 Plaza Blvd         \nPlattsburgh    NY 12901
    ## 2313                      7540 118th Ave    \nPleasant Prairie    WI 53158
    ## 2314                     1440 Bench Road           \nPocatello    ID 83201
    ## 2315                 3200 W. Temple Ave.              \nPomona    CA 91768
    ## 2316                      3413 N 14th St          \nPonca City    OK 74601
    ## 2317                  14 Regency Parkway       \nPontoon Beach    IL 62040
    ## 2318                  7540 Memorial Blvd         \nPort Arthur    TX 77642
    ## 2319                       812 Kings Hwy      \nPort Charlotte    FL 33980
    ## 2320                    910 Hwy 35 North         \nPort Lavaca    TX 77979
    ## 2321                      1791 Dunlawton         \nPort Orange    FL 32129
    ## 2322           11207 Northeast Holman St            \nPortland    OR 97220
    ## 2323                    4319 NW Yeon Ave            \nPortland    OR 97210
    ## 2324                        340 Park Ave            \nPortland    ME  4102
    ## 2325  261 Interstate Commercial Parkloop          \nPrattville    AL 36066
    ## 2326               4499 E State Route 69            \nPrescott    AZ 86301
    ## 2327             4801 North Elizabeth St              \nPueblo    CO 81008
    ## 2328                   37-18 Queens Blvd    \nLong Island City    NY 11101
    ## 2329                      1450 Tyler Ave             \nRadford    VA 24141
    ## 2330                191 Crescent Commons                \nCary    NC 27518
    ## 2331                 2211 Summit Park Ln             \nRaleigh    NC 27612
    ## 2332             1001 Aerial Center Pkwy         \nMorrisville    NC 27560
    ## 2333                 1001 Hospitality Ct         \nMorrisville    NC 27560
    ## 2334                   11131 Folsom Blvd      \nRancho Cordova    CA 95670
    ## 2335              1416 North Elk Vale Rd          \nRapid City    SD 57701
    ## 2336             128 North Expressway 77        \nRaymondville    TX 78580
    ## 2337                     2180 Hilltop Dr             \nRedding    CA 96002
    ## 2338                      4001 Market St                \nReno    NV 89502
    ## 2339                    1301 Huguenot Rd          \nMidlothian    VA 23113
    ## 2340          16280 International Street             \nDoswell    VA 23047
    ## 2341                       9040 Pams Ave            \nRichmond    VA 23237
    ## 2342                   1751 Lexington Rd            \nRichmond    KY 40475
    ## 2343                       600 Wapiti Ct               \nRifle    CO 81650
    ## 2344                     140 Sheraton Dr               \nSalem    VA 24153
    ## 2345                 4353 Canal Place SE           \nRochester    MN 55904
    ## 2346                     7401 Walton St.            \nRockford    IL 61108
    ## 2347                   2921 Highway 35 N            \nRockport    TX 78382
    ## 2348              689 East Interstate 30            \nRockwall    TX 75087
    ## 2349                       15 Chateau Dr                \nRome    GA 30161
    ## 2350              28332 Southwest Fwy 59           \nRosenberg    TX 77471
    ## 2351                    200 East 19th St             \nRoswell    NM 88201
    ## 2352                       150 Parker Dr              \nAustin    TX 78728
    ## 2353                 26147 US Highway 70       \nRuidoso Downs    NM 88346
    ## 2354                 111 East Harrell Dr        \nRussellville    AR 72802
    ## 2355                      200 Jibboom St          \nSacramento    CA 95811
    ## 2356                    4604 Madison Ave          \nSacramento    CA 95841
    ## 2357                      8 Keewaydin Dr               \nSalem    NH  3079
    ## 2358         890 Hawthorne Ave Southeast               \nSalem    OR 97301
    ## 2359              201 East Diamond Drive              \nSalina    KS 67401
    ## 2360            300 South Salisbury Blvd           \nSalisbury    MD 21801
    ## 2361            4905 West Wiley Post Way      \nSalt Lake City    UT 84116
    ## 2362                1965 North 1200 West              \nLayton    UT 84041
    ## 2363               7231 South Catalpa St             \nMidvale    UT 84047
    ## 2364                3540 South 2200 West      \nSalt Lake City    UT 84119
    ## 2365                       2307 Loop 306          \nSan Angelo    TX 76904
    ## 2366                       850 Halm Blvd         \nSan Antonio    TX 78216
    ## 2367                      3180 Goliad Rd         \nSan Antonio    TX 78223
    ## 2368            100 W. Cesar Chavez Blvd         \nSan Antonio    TX 78204
    ## 2369                     6111 IH-10 East         \nSan Antonio    TX 78219
    ## 2370                     6410 I-35 North         \nSan Antonio    TX 78218
    ## 2371                    12822 I-35 North         \nSan Antonio    TX 78233
    ## 2372            6511 West Military Drive         \nSan Antonio    TX 78227
    ## 2373                     900 Dolorosa St         \nSan Antonio    TX 78207
    ## 2374              4431 Horizon Hill Blvd         \nSan Antonio    TX 78229
    ## 2375                  1017 N Loop 1604 E         \nSan Antonio    TX 78232
    ## 2376          11155 West Loop 1604 North         \nSan Antonio    TX 78254
    ## 2377                         303 Blum St         \nSan Antonio    TX 78205
    ## 2378             7134 Northwest Loop 410         \nSan Antonio    TX 78238
    ## 2379             7202 South Pan Am Expwy         \nSan Antonio    TX 78224
    ## 2380                    25042 IH-10 West         \nSan Antonio    TX 78257
    ## 2381                      5922 I-10 West         \nSan Antonio    TX 78201
    ## 2382             205 East Hospitality Ln      \nSan Bernardino    CA 92408
    ## 2383                    760 Macadamia Dr            \nCarlsbad    CA 92011
    ## 2384                       150 Bonita Rd         \nChula Vista    CA 91910
    ## 2385                 10185 Paseo Montril           \nSan Diego    CA 92129
    ## 2386                 4610 De Soto Street           \nSan Diego    CA 92109
    ## 2387                 937 North Coast Hwy           \nOceanside    CA 92054
    ## 2388                       2380 Moore St           \nSan Diego    CA 92110
    ## 2389            641 Camino Del Rio South           \nSan Diego    CA 92108
    ## 2390                    630 Sycamore Ave               \nVista    CA 92083
    ## 2391                     20 Airport Blvd \nSouth San Francisco    CA 94080
    ## 2392                 1390 El Camino Real            \nMillbrae    CA 94030
    ## 2393                   2585 Seaboard Ave            \nSan Jose    CA 95131
    ## 2394              111 Center Point Court          \nSan Marcos    TX 78666
    ## 2395                     1619 I-35 North          \nSan Marcos    TX 78666
    ## 2396                        415 Cedar St           \nSandpoint    ID 83864
    ## 2397                       3304 Milan Rd            \nSandusky    OH 44870
    ## 2398                   1601 State Street       \nSanta Barbara    CA 93101
    ## 2399                    25201 The Old Rd     \nStevenson Ranch    CA 91381
    ## 2400                   4298 Cerrillos Rd            \nSanta Fe    NM 87507
    ## 2401                2277 Historic Rte 66          \nSanta Rosa    NM 88435
    ## 2402                 5931 Commercial Way            \nSarasota    FL 34232
    ## 2403            1803 North Tamiami Trail            \nSarasota    FL 34234
    ## 2404                         414 Gray St              \nPooler    GA 31322
    ## 2405                6 Gateway Blvd South            \nSavannah    GA 31419
    ## 2406                    6805 Abercorn St            \nSavannah    GA 31405
    ## 2407                    8484 Abercorn St            \nSavannah    GA 31406
    ## 2408                  17650 Four Oaks Ln             \nSchertz    TX 78154
    ## 2409                 504 North Poplar St              \nSearcy    AR 72143
    ## 2410         10530 Northeast Northup Way            \nKirkland    WA 98033
    ## 2411                        2224 8th Ave             \nSeattle    WA 98121
    ## 2412                 2824 South 188th St             \nSeattle    WA 98188
    ## 2413                    4115 US 27 South             \nSebring    FL 33870
    ## 2414                    350 Lighting Way            \nSecaucus    NJ  7094
    ## 2415                   1501 Hwy 46 North              \nSeguin    TX 78155
    ## 2416             2428 Winfield Dunn Pkwy         \nSevierville    TN 37764
    ## 2417                  5401 Enterprise Ct             \nShawnee    OK 74804
    ## 2418             2932 Kohler Memorial Dr           \nSheboygan    WI 53081
    ## 2419                   2912 Hwy 75 North             \nSherman    TX 75090
    ## 2420               6700 Financial Circle          \nShreveport    LA 71129
    ## 2421                 560 Silverthorne Ln        \nSilverthorne    CO 80498
    ## 2422                 4521 W. 41st Street         \nSioux Falls    SD 57106
    ## 2423                    126 Holiday Blvd             \nSlidell    LA 70460
    ## 2424                  2537 Highwood Blvd              \nSmyrna    TN 37167
    ## 2425                   2971 Main St West          \nSnellville    GA 30078
    ## 2426               23040 Lincolnway West          \nSouth Bend    IN 46628
    ## 2427                   1285 Williston Rd    \nSouth Burlington    VT  5403
    ## 2428                     7000 Padre Blvd  \nSouth Padre Island    TX 78597
    ## 2429                 9601 N Newport Hwy.             \nSpokane    WA 99218
    ## 2430              3808 North Sullivan Rd      \nSpokane Valley    WA 99216
    ## 2431                  1300 South 48th St          \nSpringdale    AR 72762
    ## 2432        2445 N. Airport Plaza Avenue         \nSpringfield    MO 65803
    ## 2433                     1121 Lejune Dr.         \nSpringfield    IL 62703
    ## 2434                 2535 South Campbell         \nSpringfield    MO 65807
    ## 2435                 100 Congress Street         \nSpringfield    MA  1104
    ## 2436                      813 Fairfax Rd        \nSaint Albans    VT  5478
    ## 2437                250 Outlet Mall Blvd     \nSaint Augustine    FL 32084
    ## 2438                  91 East 2680 South        \nSaint George    UT 84790
    ## 2439                       318 Taylor Rd           \nHazelwood    MO 63042
    ## 2440               13615 Riverport Drive    \nMaryland Heights    MO 63043
    ## 2441                   11805 Lackland Rd         \nSaint Louis    MO 63146
    ## 2442               6638 4th Street North      \nSt. Petersburg    FL 33702
    ## 2443                     135 Harvard Ave            \nStamford    CT  6902
    ## 2444                 982 Highway 12 East          \nStarkville    MS 39759
    ## 2445                      3155 Ingles Ln   \nSteamboat Springs    CO 80487
    ## 2446                   105 Christy Plaza        \nStephenville    TX 76401
    ## 2447                        4917 Main St       \nStevens Point    WI 54481
    ## 2448                   5285 West 6th Ave          \nStillwater    OK 74074
    ## 2449                  2710 West March Ln            \nStockton    CA 95219
    ## 2450                      349 Liberty St           \nPawcatuck    CT  6379
    ## 2451                        478 Main St.          \nSturbridge    MA  1518
    ## 2452                       1344 Eaton Dr     \nSulphur Springs    TX 75482
    ## 2453      106 Merchants Walk Shopping CE        \nSummersville    WV 26651
    ## 2454              13651 Northwest 2nd St             \nSunrise    FL 33325
    ## 2455              13600 Northwest 2nd St             \nSunrise    FL 33325
    ## 2456               308 SE Georgia Ave.\n          \nSweetwater    TX 79556
    ## 2457          500 North West Georgia Ave          \nSweetwater    TX 79556
    ## 2458                   1425 East 27th St              \nTacoma    WA 98421
    ## 2459                   2905 N. Monroe St         \nTallahassee    FL 32303
    ## 2460                   4730 W. Spruce St               \nTampa    FL 33607
    ## 2461                   7500 Hwy 19 North       \nPinellas Park    FL 33781
    ## 2462              310 Grand Regency Blvd             \nBrandon    FL 33510
    ## 2463             602 South Falkenburg Rd               \nTampa    FL 33619
    ## 2464                3826 West Waters Ave               \nTampa    FL 33614
    ## 2465               4811 US Hwy 301 North               \nTampa    FL 33610
    ## 2466                  9202 North 30th St               \nTampa    FL 33612
    ## 2467              17301 Dona Michelle Dr               \nTampa    FL 33647
    ## 2468                4620 West Gandy Blvd               \nTampa    FL 33611
    ## 2469                      500 Steuber Rd           \nTehachapi    CA 93561
    ## 2470                 27330 Jefferson Ave            \nTemecula    CA 92590
    ## 2471                1604 West Barton Ave              \nTemple    TX 76504
    ## 2472                 451 E Margaret Ave.         \nTerre Haute    IN 47802
    ## 2473                   1614 Hwy 34 South             \nTerrell    TX 75160
    ## 2474                  3750 Market Street          \nThe Colony    TX 75056
    ## 2475                     1320 Newbury Rd       \nThousand Oaks    CA 91320
    ## 2476                1154 Professional Dr          \nPerrysburg    OH 43551
    ## 2477            14000 Medical Complex Dr             \nTomball    TX 77375
    ## 2478                      2833 Toupal Dr            \nTrinidad    CO 81082
    ## 2479              7001 South Tucson Blvd              \nTucson    AZ 85706
    ## 2480                  6404 East Broadway              \nTucson    AZ 85710
    ## 2481                 102 N. Alvernon Way              \nTucson    AZ 85711
    ## 2482                 2516 South Adams St           \nTucumcari    NM 88401
    ## 2483                   1500 N. Cherry Ct              \nTulare    CA 93274
    ## 2484              23 North 67th East Ave               \nTulsa    OK 74115
    ## 2485                 2009 S. Cherokee St             \nCatoosa    OK 74015
    ## 2486                 6030 East Skelly Dr               \nTulsa    OK 74135
    ## 2487               4600 Capitol Blvd. SE            \nTumwater    WA 98501
    ## 2488               1013 North Gloster St              \nTupelo    MS 38804
    ## 2489            4122 McFarland Blvd East          \nTuscaloosa    AL 35405
    ## 2490                  539 Pole Line Road          \nTwin Falls    ID 83301
    ## 2491                  2552 S SE Loop 323               \nTyler    TX 75701
    ## 2492        1601 West Southwest Loop 323               \nTyler    TX 75701
    ## 2493                     6604 Mall Blvd.          \nUnion City    GA 30291
    ## 2494            3100 North University Dr       \nCoral Springs    FL 33065
    ## 2495                3701 East Fowler Ave               \nTampa    FL 33612
    ## 2496                   1800 Clubhouse Dr            \nValdosta    GA 31601
    ## 2497             1500 Northeast 134th St           \nVancouver    WA 98685
    ## 2498                   5818 Valentine Rd             \nVentura    CA 93003
    ## 2499                   5394 Willow Place              \nVerona    NY 13478
    ## 2500              4160 South Frontage Rd           \nVicksburg    MS 39180
    ## 2501               3107 S Laurent Street            \nVictoria    TX 77901
    ## 2502                  7603 N. Navarro St            \nVictoria    TX 77904
    ## 2503                    2800 Pacific Ave      \nVirginia Beach    VA 23451
    ## 2504               5438 West Cypress Ave             \nVisalia    CA 93277
    ## 2505              11770 Business Park Dr             \nWaldorf    MD 20601
    ## 2506            13450 Vera McGowan Drive              \nWalker    LA 70785
    ## 2507                  776 Silverstone Dr         \nWalla Walla    WA 99362
    ## 2508                    4080 Watson Blvd       \nWarner Robins    GA 31093
    ## 2509                   36 Jefferson Blvd             \nWarwick    RI  2888
    ## 2510                    1910 Stewart Ave              \nWausau    WI 54401
    ## 2511                      311 Stadium Dr          \nWaxahachie    TX 75165
    ## 2512             4300 Carriage Way Drive         \nWeatherford    OK 73096
    ## 2513                        1915 Wall St         \nWeatherford    TX 76086
    ## 2514            1905 North Wenatchee Ave           \nWenatchee    WA 98801
    ## 2515                        109 Route 36    \nWest Long Branch    NJ  7764
    ## 2516                 507 Constitution Dr         \nWest Monroe    LA 71292
    ## 2517                5981 Okeechobee Blvd     \nWest Palm Beach    FL 33417
    ## 2518          1910 Palm Beach Lakes Blvd     \nWest Palm Beach    FL 33409
    ## 2519                5500 West Kellogg Dr             \nWichita    KS 67209
    ## 2520              1128 Central Fwy North       \nWichita Falls    TX 76306
    ## 2521            2511 E. Montgomery Place       \nWichita Falls    TX 76308
    ## 2522               2660 N. Greenwich Ct.             \nWichita    KS 67226
    ## 2523          1100 West Cataract Lake Rd            \nWilliams    AZ 86046
    ## 2524                     600 Bypass Road        \nWilliamsburg    VA 23185
    ## 2525               8815 Southwest Sun Pl         \nWilsonville    OR 97070
    ## 2526                  1055 Millwood Pike          \nWinchester    VA 22602
    ## 2527                          226 Spur 5              \nWinnie    TX 77665
    ## 2528                    2020 Griffith Rd       \nWinston-Salem    NC 27103
    ## 2529                   120 Arney Road NE            \nWoodburn    OR 97071
    ## 2530                  700 Bielenberg Dr.            \nWoodbury    MN 55125
    ## 2531                    28673 I-45 North       \nThe Woodlands    TX 77381
    ## 2532                     6930 FM 1488 Rd            \nMagnolia    TX 77354
    ## 2533                   3410 Williams Ave            \nWoodward    OK 73801
    ## 2534                     6003 Woodway Dr                \nWaco    TX 76712
    ## 2535                       2210 W FM 544               \nWylie    TX 75098
    ## 2536                   1800 East Main St          \nWytheville    VA 24382
    ## 2537                   1405 Kenneth Road                \nYork    PA 17408
    ## 2538                 792 Zion Park Blvd.          \nSpringdale    UT 84767
    ## 2539                                <NA>                  <NA>    DE    NA
    ## 2540                                <NA>                  <NA>    HI    NA
    ## 2541                                <NA>                  <NA>    DC    NA
    ##       longitude latitude establishment                 name      area
    ## 1    -149.87670 61.19530       Denny's                 <NA>        NA
    ## 2    -149.80900 61.20970       Denny's                 <NA>        NA
    ## 3    -147.76000 64.83660       Denny's                 <NA>        NA
    ## 4     -85.46810 32.60330       Denny's                 <NA>        NA
    ## 5     -86.83170 33.56150       Denny's                 <NA>        NA
    ## 6     -86.80370 33.50070       Denny's                 <NA>        NA
    ## 7     -86.87220 34.20680       Denny's                 <NA>        NA
    ## 8     -85.40060 31.19040       Denny's                 <NA>        NA
    ## 9     -86.42090 32.19530       Denny's                 <NA>        NA
    ## 10    -86.65560 34.73790       Denny's                 <NA>        NA
    ## 11    -94.18830 36.33510       Denny's                 <NA>        NA
    ## 12    -94.19660 36.05340       Denny's                 <NA>        NA
    ## 13    -94.36800 35.36090       Denny's                 <NA>        NA
    ## 14    -92.39400 34.75120       Denny's                 <NA>        NA
    ## 15    -93.09000 35.28420       Denny's                 <NA>        NA
    ## 16    -94.18390 36.17530       Denny's                 <NA>        NA
    ## 17    -94.04320 33.36220       Denny's                 <NA>        NA
    ## 18    -93.94030 33.50540       Denny's                 <NA>        NA
    ## 19    -90.13790 35.15320       Denny's                 <NA>        NA
    ## 20   -112.14950 33.87100       Denny's                 <NA>        NA
    ## 21   -111.60110 33.41460       Denny's                 <NA>        NA
    ## 22   -110.30710 31.98090       Denny's                 <NA>        NA
    ## 23   -112.55640 33.44350       Denny's                 <NA>        NA
    ## 24   -114.58630 35.12150       Denny's                 <NA>        NA
    ## 25   -111.88310 34.57670       Denny's                 <NA>        NA
    ## 26   -111.70780 32.87960       Denny's                 <NA>        NA
    ## 27   -111.87270 33.30610       Denny's                 <NA>        NA
    ## 28   -111.96890 33.30540       Denny's                 <NA>        NA
    ## 29   -109.56340 36.15580       Denny's                 <NA>        NA
    ## 30   -112.00090 34.72130       Denny's                 <NA>        NA
    ## 31   -109.56050 31.33920       Denny's                 <NA>        NA
    ## 32   -111.55040 32.73240       Denny's                 <NA>        NA
    ## 33   -111.66130 35.17900       Denny's                 <NA>        NA
    ## 34   -111.62390 35.19230       Denny's                 <NA>        NA
    ## 35   -112.45310 34.81380       Denny's                 <NA>        NA
    ## 36   -111.65560 35.19080       Denny's                 <NA>        NA
    ## 37   -111.71310 33.57220       Denny's                 <NA>        NA
    ## 38   -111.80720 33.37450       Denny's                 <NA>        NA
    ## 39   -111.75300 33.33560       Denny's                 <NA>        NA
    ## 40   -111.78960 33.27800       Denny's                 <NA>        NA
    ## 41   -112.15240 33.58190       Denny's                 <NA>        NA
    ## 42   -112.27200 33.50830       Denny's                 <NA>        NA
    ## 43   -112.17020 33.61070       Denny's                 <NA>        NA
    ## 44   -112.35820 33.46000       Denny's                 <NA>        NA
    ## 45   -110.98370 31.90740       Denny's                 <NA>        NA
    ## 46   -110.13720 34.93250       Denny's                 <NA>        NA
    ## 47   -114.00840 35.21980       Denny's                 <NA>        NA
    ## 48   -114.00780 35.22040       Denny's                 <NA>        NA
    ## 49   -114.34380 34.47450       Denny's                 <NA>        NA
    ## 50   -112.04780 33.06520       Denny's                 <NA>        NA
    ## 51   -111.80500 33.41520       Denny's                 <NA>        NA
    ## 52   -111.68490 33.39140       Denny's                 <NA>        NA
    ## 53   -111.84010 33.39420       Denny's                 <NA>        NA
    ## 54   -110.93140 31.34410       Denny's                 <NA>        NA
    ## 55   -111.47520 36.92180       Denny's                 <NA>        NA
    ## 56   -111.32310 34.23830       Denny's                 <NA>        NA
    ## 57   -112.24580 33.58830       Denny's                 <NA>        NA
    ## 58   -112.23100 33.63810       Denny's                 <NA>        NA
    ## 59   -112.11260 33.47960       Denny's                 <NA>        NA
    ## 60   -112.06500 33.50930       Denny's                 <NA>        NA
    ## 61   -112.13310 33.52400       Denny's                 <NA>        NA
    ## 62   -112.03640 33.59680       Denny's                 <NA>        NA
    ## 63   -112.11460 33.68380       Denny's                 <NA>        NA
    ## 64   -112.16890 33.49540       Denny's                 <NA>        NA
    ## 65   -112.01340 33.64050       Denny's                 <NA>        NA
    ## 66   -112.22040 33.46390       Denny's                 <NA>        NA
    ## 67   -112.20340 33.46010       Denny's                 <NA>        NA
    ## 68   -112.11780 33.56790       Denny's                 <NA>        NA
    ## 69   -112.11880 33.63980       Denny's                 <NA>        NA
    ## 70   -111.92970 33.65530       Denny's                 <NA>        NA
    ## 71   -112.12870 33.79840       Denny's                 <NA>        NA
    ## 72   -112.48640 34.56350       Denny's                 <NA>        NA
    ## 73   -112.32650 34.58590       Denny's                 <NA>        NA
    ## 74   -111.58490 33.17980       Denny's                 <NA>        NA
    ## 75   -111.92610 33.48640       Denny's                 <NA>        NA
    ## 76   -111.88360 33.53840       Denny's                 <NA>        NA
    ## 77   -111.91740 33.46570       Denny's                 <NA>        NA
    ## 78   -110.02390 34.20950       Denny's                 <NA>        NA
    ## 79   -110.27370 31.55460       Denny's                 <NA>        NA
    ## 80   -109.11320 35.65840       Denny's                 <NA>        NA
    ## 81   -112.37740 33.65970       Denny's                 <NA>        NA
    ## 82   -111.92630 33.43500       Denny's                 <NA>        NA
    ## 83   -111.97630 33.42150       Denny's                 <NA>        NA
    ## 84   -111.95960 33.40740       Denny's                 <NA>        NA
    ## 85   -111.92640 33.38500       Denny's                 <NA>        NA
    ## 86   -111.96970 33.37830       Denny's                 <NA>        NA
    ## 87   -109.74840 32.84820       Denny's                 <NA>        NA
    ## 88   -111.51780 35.28120       Denny's                 <NA>        NA
    ## 89   -110.97820 32.29740       Denny's                 <NA>        NA
    ## 90   -110.99660 32.13390       Denny's                 <NA>        NA
    ## 91   -110.93260 32.13410       Denny's                 <NA>        NA
    ## 92   -110.85510 32.22090       Denny's                 <NA>        NA
    ## 93   -110.91220 32.16310       Denny's                 <NA>        NA
    ## 94   -110.94610 32.20740       Denny's                 <NA>        NA
    ## 95   -111.06840 32.33760       Denny's                 <NA>        NA
    ## 96   -110.98330 32.23100       Denny's                 <NA>        NA
    ## 97   -112.73800 33.98070       Denny's                 <NA>        NA
    ## 98   -110.67490 35.01670       Denny's                 <NA>        NA
    ## 99   -112.29970 33.60550       Denny's                 <NA>        NA
    ## 100  -114.60550 32.69850       Denny's                 <NA>        NA
    ## 101  -114.54200 32.67810       Denny's                 <NA>        NA
    ## 102  -114.44430 32.66820       Denny's                 <NA>        NA
    ## 103  -117.39960 34.50910       Denny's                 <NA>        NA
    ## 104  -118.13090 34.09330       Denny's                 <NA>        NA
    ## 105  -117.92700 33.80320       Denny's                 <NA>        NA
    ## 106  -117.91530 33.80850       Denny's                 <NA>        NA
    ## 107  -117.91500 33.79670       Denny's                 <NA>        NA
    ## 108  -117.94150 33.83080       Denny's                 <NA>        NA
    ## 109  -117.88920 33.85510       Denny's                 <NA>        NA
    ## 110  -117.88910 33.80340       Denny's                 <NA>        NA
    ## 111  -121.83870 38.00260       Denny's                 <NA>        NA
    ## 112  -121.76790 37.96290       Denny's                 <NA>        NA
    ## 113  -117.22140 34.52690       Denny's                 <NA>        NA
    ## 114  -117.24930 34.47100       Denny's                 <NA>        NA
    ## 115  -118.03120 34.14000       Denny's                 <NA>        NA
    ## 116  -118.08240 33.87750       Denny's                 <NA>        NA
    ## 117  -120.66570 35.48680       Denny's                 <NA>        NA
    ## 118  -120.61400 37.34570       Denny's                 <NA>        NA
    ## 119  -121.05940 38.92320       Denny's                 <NA>        NA
    ## 120  -116.07050 35.26820       Denny's                 <NA>        NA
    ## 121  -119.03370 35.31760       Denny's                 <NA>        NA
    ## 122  -118.96760 35.39620       Denny's                 <NA>        NA
    ## 123  -119.02720 35.29590       Denny's                 <NA>        NA
    ## 124  -118.91830 35.35440       Denny's                 <NA>        NA
    ## 125  -119.18690 35.53470       Denny's                 <NA>        NA
    ## 126  -119.04320 35.38390       Denny's                 <NA>        NA
    ## 127  -119.07630 35.44130       Denny's                 <NA>        NA
    ## 128  -119.09210 35.31500       Denny's                 <NA>        NA
    ## 129  -119.09870 35.38350       Denny's                 <NA>        NA
    ## 130  -117.95920 34.07250       Denny's                 <NA>        NA
    ## 131  -116.94590 33.92760       Denny's                 <NA>        NA
    ## 132  -117.01020 34.89800       Denny's                 <NA>        NA
    ## 133  -117.08280 34.85460       Denny's                 <NA>        NA
    ## 134  -117.08290 34.84920       Denny's                 <NA>        NA
    ## 135  -116.97670 33.92620       Denny's                 <NA>        NA
    ## 136  -118.14260 33.87550       Denny's                 <NA>        NA
    ## 137  -118.13560 33.90400       Denny's                 <NA>        NA
    ## 138  -116.90330 34.24360       Denny's                 <NA>        NA
    ## 139  -118.39530 37.37150       Denny's                 <NA>        NA
    ## 140  -114.60600 33.60660       Denny's                 <NA>        NA
    ## 141  -117.99820 33.85900       Denny's                 <NA>        NA
    ## 142  -118.34890 34.19500       Denny's                 <NA>        NA
    ## 143  -118.31290 34.16330       Denny's                 <NA>        NA
    ## 144  -119.39790 35.40120       Denny's                 <NA>        NA
    ## 145  -115.50040 32.69400       Denny's                 <NA>        NA
    ## 146  -117.05820 33.99560       Denny's                 <NA>        NA
    ## 147  -119.04910 34.21830       Denny's                 <NA>        NA
    ## 148  -120.96740 38.65820       Denny's                 <NA>        NA
    ## 149  -121.93160 37.28580       Denny's                 <NA>        NA
    ## 150  -118.60600 34.22050       Denny's                 <NA>        NA
    ## 151  -118.46550 34.41570       Denny's                 <NA>        NA
    ## 152  -117.34330 33.16280       Denny's                 <NA>        NA
    ## 153  -121.32810 38.63560       Denny's                 <NA>        NA
    ## 154  -118.26420 33.84530       Denny's                 <NA>        NA
    ## 155  -118.62120 34.49500       Denny's                 <NA>        NA
    ## 156  -116.46030 33.77870       Denny's                 <NA>        NA
    ## 157  -120.97420 37.60920       Denny's                 <NA>        NA
    ## 158  -121.84490 39.75250       Denny's                 <NA>        NA
    ## 159  -117.68950 34.03260       Denny's                 <NA>        NA
    ## 160  -117.68980 33.97180       Denny's                 <NA>        NA
    ## 161  -117.06410 32.64800       Denny's                 <NA>        NA
    ## 162  -117.09740 32.64030       Denny's                 <NA>        NA
    ## 163  -116.96240 32.62140       Denny's                 <NA>        NA
    ## 164  -121.27160 38.67870       Denny's                 <NA>        NA
    ## 165  -118.16200 34.01110       Denny's                 <NA>        NA
    ## 166  -118.12700 33.97480       Denny's                 <NA>        NA
    ## 167  -117.71920 34.08060       Denny's                 <NA>        NA
    ## 168  -119.69650 36.83750       Denny's                 <NA>        NA
    ## 169  -119.72420 36.80840       Denny's                 <NA>        NA
    ## 170  -120.25640 36.25420       Denny's                 <NA>        NA
    ## 171  -117.30850 34.04870       Denny's                 <NA>        NA
    ## 172  -117.32530 34.06700       Denny's                 <NA>        NA
    ## 173  -122.05230 37.96860       Denny's                 <NA>        NA
    ## 174  -122.12620 38.22010       Denny's                 <NA>        NA
    ## 175  -122.19470 39.90650       Denny's                 <NA>        NA
    ## 176  -117.51680 33.88510       Denny's                 <NA>        NA
    ## 177  -117.53750 33.84320       Denny's                 <NA>        NA
    ## 178  -117.60190 33.87930       Denny's                 <NA>        NA
    ## 179  -117.91930 33.68570       Denny's                 <NA>        NA
    ## 180  -124.19530 41.75550       Denny's                 <NA>        NA
    ## 181  -118.39340 34.00410       Denny's                 <NA>        NA
    ## 182  -122.47130 37.67510       Denny's                 <NA>        NA
    ## 183  -117.68670 33.46570       Denny's                 <NA>        NA
    ## 184  -117.25590 32.97950       Denny's                 <NA>        NA
    ## 185  -119.24960 35.79040       Denny's                 <NA>        NA
    ## 186  -121.83920 38.45800       Denny's                 <NA>        NA
    ## 187  -118.13210 33.93950       Denny's                 <NA>        NA
    ## 188  -117.98270 34.13950       Denny's                 <NA>        NA
    ## 189  -116.93190 32.74390       Denny's                 <NA>        NA
    ## 190  -117.00170 32.80180       Denny's                 <NA>        NA
    ## 191  -116.97800 32.79500       Denny's                 <NA>        NA
    ## 192  -116.95100 32.80370       Denny's                 <NA>        NA
    ## 193  -115.56860 32.77680       Denny's                 <NA>        NA
    ## 194  -115.53490 32.76280       Denny's                 <NA>        NA
    ## 195  -122.31540 37.92110       Denny's                 <NA>        NA
    ## 196  -118.02490 34.07200       Denny's                 <NA>        NA
    ## 197  -118.05520 34.07210       Denny's                 <NA>        NA
    ## 198  -118.37540 33.91650       Denny's                 <NA>        NA
    ## 199  -121.38070 38.40900       Denny's                 <NA>        NA
    ## 200  -122.29590 37.83820       Denny's                 <NA>        NA
    ## 201  -117.28990 33.04870       Denny's                 <NA>        NA
    ## 202  -117.09230 33.12820       Denny's                 <NA>        NA
    ## 203  -124.16990 40.80140       Denny's                 <NA>        NA
    ## 204  -122.06550 38.25690       Denny's                 <NA>        NA
    ## 205  -122.06490 38.25830       Denny's                 <NA>        NA
    ## 206  -117.25140 33.37650       Denny's                 <NA>        NA
    ## 207  -121.16990 38.67130       Denny's                 <NA>        NA
    ## 208  -117.43340 34.07040       Denny's                 <NA>        NA
    ## 209  -117.51160 34.12140       Denny's                 <NA>        NA
    ## 210  -117.66480 33.68030       Denny's                 <NA>        NA
    ## 211  -117.95470 33.72750       Denny's                 <NA>        NA
    ## 212  -119.68380 36.62800       Denny's                 <NA>        NA
    ## 213  -122.00060 37.53360       Denny's                 <NA>        NA
    ## 214  -121.92790 37.49120       Denny's                 <NA>        NA
    ## 215  -119.78940 36.74400       Denny's                 <NA>        NA
    ## 216  -119.79050 36.77840       Denny's                 <NA>        NA
    ## 217  -119.77180 36.70750       Denny's                 <NA>        NA
    ## 218  -119.76950 36.80890       Denny's                 <NA>        NA
    ## 219  -119.82440 36.80840       Denny's                 <NA>        NA
    ## 220  -119.78350 36.83760       Denny's                 <NA>        NA
    ## 221  -119.70020 36.73790       Denny's                 <NA>        NA
    ## 222  -119.83010 36.75660       Denny's                 <NA>        NA
    ## 223  -123.80590 39.45180       Denny's                 <NA>        NA
    ## 224  -117.87930 33.87800       Denny's                 <NA>        NA
    ## 225  -117.92430 33.86100       Denny's                 <NA>        NA
    ## 226  -121.29820 38.25310       Denny's                 <NA>        NA
    ## 227  -117.92030 33.76960       Denny's                 <NA>        NA
    ## 228  -118.30910 33.86150       Denny's                 <NA>        NA
    ## 229  -121.56530 37.02390       Denny's                 <NA>        NA
    ## 230  -117.87260 34.12130       Denny's                 <NA>        NA
    ## 231  -121.01580 37.10310       Denny's                 <NA>        NA
    ## 232  -117.98880 34.01830       Denny's                 <NA>        NA
    ## 233  -119.67200 36.32020       Denny's                 <NA>        NA
    ## 234  -118.35280 33.91230       Denny's                 <NA>        NA
    ## 235  -118.36130 33.90180       Denny's                 <NA>        NA
    ## 236  -122.06590 37.60900       Denny's                 <NA>        NA
    ## 237  -116.99970 33.74740       Denny's                 <NA>        NA
    ## 238  -117.37590 34.42670       Denny's                 <NA>        NA
    ## 239  -118.32230 34.09800       Denny's                 <NA>        NA
    ## 240  -117.98900 33.69420       Denny's                 <NA>        NA
    ## 241  -116.23180 33.71490       Denny's                 <NA>        NA
    ## 242  -117.75950 33.67620       Denny's                 <NA>        NA
    ## 243  -120.77310 38.34690       Denny's                 <NA>        NA
    ## 244  -119.96030 35.98810       Denny's                 <NA>        NA
    ## 245  -121.13780 36.20530       Denny's                 <NA>        NA
    ## 246  -119.55990 36.51790       Denny's                 <NA>        NA
    ## 247  -117.96780 33.91800       Denny's                 <NA>        NA
    ## 248  -117.01470 32.75570       Denny's                 <NA>        NA
    ## 249  -117.95930 34.02090       Denny's                 <NA>        NA
    ## 250  -117.29330 33.65760       Denny's                 <NA>        NA
    ## 251  -117.69920 33.62170       Denny's                 <NA>        NA
    ## 252  -116.90130 32.82620       Denny's                 <NA>        NA
    ## 253  -118.16060 33.83240       Denny's                 <NA>        NA
    ## 254  -118.12430 33.86030       Denny's                 <NA>        NA
    ## 255  -118.08640 33.83150       Denny's                 <NA>        NA
    ## 256  -118.14940 34.70340       Denny's                 <NA>        NA
    ## 257  -118.16590 34.67510       Denny's                 <NA>        NA
    ## 258  -121.29360 37.81000       Denny's                 <NA>        NA
    ## 259  -118.48940 35.13200       Denny's                 <NA>        NA
    ## 260  -121.25930 38.11630       Denny's                 <NA>        NA
    ## 261  -121.39360 38.11860       Denny's                 <NA>        NA
    ## 262  -118.18950 33.77410       Denny's                 <NA>        NA
    ## 263  -118.12350 33.77590       Denny's                 <NA>        NA
    ## 264  -118.18500 33.81760       Denny's                 <NA>        NA
    ## 265  -118.12520 33.80800       Denny's                 <NA>        NA
    ## 266  -118.29170 34.07780       Denny's                 <NA>        NA
    ## 267  -118.29170 34.06230       Denny's                 <NA>        NA
    ## 268  -118.30780 34.06170       Denny's                 <NA>        NA
    ## 269  -118.23180 34.05480       Denny's                 <NA>        NA
    ## 270  -118.33510 34.01850       Denny's                 <NA>        NA
    ## 271  -118.26270 34.04630       Denny's                 <NA>        NA
    ## 272  -118.16290 34.04060       Denny's                 <NA>        NA
    ## 273  -118.31480 34.09810       Denny's                 <NA>        NA
    ## 274  -118.37740 33.94540       Denny's                 <NA>        NA
    ## 275  -118.38050 33.95980       Denny's                 <NA>        NA
    ## 276  -118.23910 33.92730       Denny's                 <NA>        NA
    ## 277  -118.24510 34.11410       Denny's                 <NA>        NA
    ## 278  -120.87780 37.05680       Denny's                 <NA>        NA
    ## 279  -120.82070 37.05710       Denny's                 <NA>        NA
    ## 280  -119.86760 35.61490       Denny's                 <NA>        NA
    ## 281  -118.21140 33.92960       Denny's                 <NA>        NA
    ## 282  -120.13410 37.01820       Denny's                 <NA>        NA
    ## 283  -121.21610 37.78670       Denny's                 <NA>        NA
    ## 284  -121.80290 36.69590       Denny's                 <NA>        NA
    ## 285  -121.59370 39.14550       Denny's                 <NA>        NA
    ## 286  -118.18560 33.98710       Denny's                 <NA>        NA
    ## 287  -124.10260 40.93280       Denny's                 <NA>        NA
    ## 288  -116.08980 33.57120       Denny's                 <NA>        NA
    ## 289  -120.49530 37.30060       Denny's                 <NA>        NA
    ## 290  -121.91290 37.42540       Denny's                 <NA>        NA
    ## 291  -117.54390 33.97390       Denny's                 <NA>        NA
    ## 292  -117.68970 33.60720       Denny's                 <NA>        NA
    ## 293  -120.99400 37.66600       Denny's                 <NA>        NA
    ## 294  -121.02980 37.66620       Denny's                 <NA>        NA
    ## 295  -120.99380 37.64600       Denny's                 <NA>        NA
    ## 296  -118.17510 35.05690       Denny's                 <NA>        NA
    ## 297  -118.12570 34.03280       Denny's                 <NA>        NA
    ## 298  -121.86160 36.59640       Denny's                 <NA>        NA
    ## 299  -121.89270 36.59530       Denny's                 <NA>        NA
    ## 300  -118.86720 34.28030       Denny's                 <NA>        NA
    ## 301  -117.22740 33.93910       Denny's                 <NA>        NA
    ## 302  -121.65500 37.15150       Denny's                 <NA>        NA
    ## 303  -117.19730 33.55440       Denny's                 <NA>        NA
    ## 304  -117.12490 33.59210       Denny's                 <NA>        NA
    ## 305  -116.55060 33.90550       Denny's                 <NA>        NA
    ## 306  -122.27410 38.28050       Denny's                 <NA>        NA
    ## 307  -117.07910 32.66130       Denny's                 <NA>        NA
    ## 308  -117.10590 32.66080       Denny's                 <NA>        NA
    ## 309  -114.61170 34.83910       Denny's                 <NA>        NA
    ## 310  -118.93620 34.19060       Denny's                 <NA>        NA
    ## 311  -121.13120 38.86980       Denny's                 <NA>        NA
    ## 312  -118.56710 34.37850       Denny's                 <NA>        NA
    ## 313  -118.37880 34.17220       Denny's                 <NA>        NA
    ## 314  -117.56240 33.90170       Denny's                 <NA>        NA
    ## 315  -118.41900 34.20120       Denny's                 <NA>        NA
    ## 316  -118.55360 34.23410       Denny's                 <NA>        NA
    ## 317  -118.50290 34.25750       Denny's                 <NA>        NA
    ## 318  -118.08180 33.91680       Denny's                 <NA>        NA
    ## 319  -120.82970 37.77320       Denny's                 <NA>        NA
    ## 320  -119.63410 37.38810       Denny's                 <NA>        NA
    ## 321  -122.19620 37.74530       Denny's                 <NA>        NA
    ## 322  -117.32810 33.18550       Denny's                 <NA>        NA
    ## 323  -117.38980 33.20700       Denny's                 <NA>        NA
    ## 324  -117.28990 33.24350       Denny's                 <NA>        NA
    ## 325  -117.59310 34.02960       Denny's                 <NA>        NA
    ## 326  -117.62550 34.07770       Denny's                 <NA>        NA
    ## 327  -117.61100 34.06840       Denny's                 <NA>        NA
    ## 328  -117.55840 34.07010       Denny's                 <NA>        NA
    ## 329  -117.83630 33.83500       Denny's                 <NA>        NA
    ## 330  -117.88310 33.78790       Denny's                 <NA>        NA
    ## 331  -121.22840 38.67860       Denny's                 <NA>        NA
    ## 332  -121.54680 39.50580       Denny's                 <NA>        NA
    ## 333  -119.17750 34.21390       Denny's                 <NA>        NA
    ## 334  -117.25550 32.79650       Denny's                 <NA>        NA
    ## 335  -116.54680 33.83950       Denny's                 <NA>        NA
    ## 336  -118.15310 34.60180       Denny's                 <NA>        NA
    ## 337  -118.04530 34.55480       Denny's                 <NA>        NA
    ## 338  -118.44880 34.22480       Denny's                 <NA>        NA
    ## 339  -118.09610 34.14610       Denny's                 <NA>        NA
    ## 340  -120.68640 35.64200       Denny's                 <NA>        NA
    ## 341  -121.17610 37.46670       Denny's                 <NA>        NA
    ## 342  -117.21890 33.78240       Denny's                 <NA>        NA
    ## 343  -122.67050 38.26950       Denny's                 <NA>        NA
    ## 344  -118.08860 34.00330       Denny's                 <NA>        NA
    ## 345  -120.62180 35.13560       Denny's                 <NA>        NA
    ## 346  -117.87250 33.86250       Denny's                 <NA>        NA
    ## 347  -120.83060 38.72260       Denny's                 <NA>        NA
    ## 348  -122.06450 37.97480       Denny's                 <NA>        NA
    ## 349  -117.79820 34.04070       Denny's                 <NA>        NA
    ## 350  -117.77780 34.07420       Denny's                 <NA>        NA
    ## 351  -117.81650 34.05020       Denny's                 <NA>        NA
    ## 352  -119.19510 34.17600       Denny's                 <NA>        NA
    ## 353  -119.04570 36.08040       Denny's                 <NA>        NA
    ## 354  -119.02530 36.04980       Denny's                 <NA>        NA
    ## 355  -116.88330 33.03300       Denny's                 <NA>        NA
    ## 356  -121.26960 38.61410       Denny's                 <NA>        NA
    ## 357  -117.54560 34.10630       Denny's                 <NA>        NA
    ## 358  -118.30970 33.75550       Denny's                 <NA>        NA
    ## 359  -122.22880 40.17910       Denny's                 <NA>        NA
    ## 360  -122.35810 40.57070       Denny's                 <NA>        NA
    ## 361  -122.36200 40.57100       Denny's                 <NA>        NA
    ## 362  -117.18270 34.05730       Denny's                 <NA>        NA
    ## 363  -117.20870 34.06850       Denny's                 <NA>        NA
    ## 364  -118.38200 33.87200       Denny's                 <NA>        NA
    ## 365  -122.21420 37.48710       Denny's                 <NA>        NA
    ## 366  -118.53610 34.20840       Denny's                 <NA>        NA
    ## 367  -117.39820 34.10670       Denny's                 <NA>        NA
    ## 368  -117.67000 35.62240       Denny's                 <NA>        NA
    ## 369  -121.14250 37.75690       Denny's                 <NA>        NA
    ## 370  -117.43680 33.90860       Denny's                 <NA>        NA
    ## 371  -117.40610 33.93730       Denny's                 <NA>        NA
    ## 372  -117.33880 33.97570       Denny's                 <NA>        NA
    ## 373  -121.22480 38.78850       Denny's                 <NA>        NA
    ## 374  -121.26680 38.74320       Denny's                 <NA>        NA
    ## 375  -121.31150 38.76310       Denny's                 <NA>        NA
    ## 376  -117.88900 33.99140       Denny's                 <NA>        NA
    ## 377  -117.43590 34.01060       Denny's                 <NA>        NA
    ## 378  -121.50220 38.59730       Denny's                 <NA>        NA
    ## 379  -121.38250 38.63730       Denny's                 <NA>        NA
    ## 380  -121.44180 38.49610       Denny's                 <NA>        NA
    ## 381  -121.41540 38.58940       Denny's                 <NA>        NA
    ## 382  -121.41060 38.55530       Denny's                 <NA>        NA
    ## 383  -121.41630 38.47430       Denny's                 <NA>        NA
    ## 384  -121.52610 38.65250       Denny's                 <NA>        NA
    ## 385  -121.36310 38.65960       Denny's                 <NA>        NA
    ## 386  -121.07550 37.70150       Denny's                 <NA>        NA
    ## 387  -121.62160 36.65910       Denny's                 <NA>        NA
    ## 388  -121.65620 36.72170       Denny's                 <NA>        NA
    ## 389  -121.63640 36.65630       Denny's                 <NA>        NA
    ## 390  -117.27240 34.13590       Denny's                 <NA>        NA
    ## 391  -117.36210 34.18990       Denny's                 <NA>        NA
    ## 392  -117.62340 33.43810       Denny's                 <NA>        NA
    ## 393  -117.13830 32.75520       Denny's                 <NA>        NA
    ## 394  -117.10060 32.74960       Denny's                 <NA>        NA
    ## 395  -117.22690 32.72740       Denny's                 <NA>        NA
    ## 396  -117.15300 32.76690       Denny's                 <NA>        NA
    ## 397  -117.15800 32.77220       Denny's                 <NA>        NA
    ## 398  -117.20500 32.75530       Denny's                 <NA>        NA
    ## 399  -117.22080 32.75510       Denny's                 <NA>        NA
    ## 400  -117.07160 32.77260       Denny's                 <NA>        NA
    ## 401  -117.20110 32.83270       Denny's                 <NA>        NA
    ## 402  -117.04790 32.77400       Denny's                 <NA>        NA
    ## 403  -117.16800 32.87830       Denny's                 <NA>        NA
    ## 404  -117.13880 32.83430       Denny's                 <NA>        NA
    ## 405  -117.07630 33.01640       Denny's                 <NA>        NA
    ## 406  -117.11290 32.91680       Denny's                 <NA>        NA
    ## 407  -117.14410 32.87010       Denny's                 <NA>        NA
    ## 408  -117.08030 32.57650       Denny's                 <NA>        NA
    ## 409  -117.81740 34.10390       Denny's                 <NA>        NA
    ## 410  -118.44050 34.28310       Denny's                 <NA>        NA
    ## 411  -122.40460 37.78430       Denny's                 <NA>        NA
    ## 412  -122.41720 37.80710       Denny's                 <NA>        NA
    ## 413  -116.97210 33.80420       Denny's                 <NA>        NA
    ## 414  -121.87390 37.31650       Denny's                 <NA>        NA
    ## 415  -121.91060 37.36320       Denny's                 <NA>        NA
    ## 416  -121.88090 37.27460       Denny's                 <NA>        NA
    ## 417  -121.82320 37.29910       Denny's                 <NA>        NA
    ## 418  -121.82930 37.32070       Denny's                 <NA>        NA
    ## 419  -121.87020 37.25050       Denny's                 <NA>        NA
    ## 420  -121.97730 37.30990       Denny's                 <NA>        NA
    ## 421  -121.91940 37.37460       Denny's                 <NA>        NA
    ## 422  -121.86110 37.38610       Denny's                 <NA>        NA
    ## 423  -122.17050 37.70890       Denny's                 <NA>        NA
    ## 424  -122.12430 37.70930       Denny's                 <NA>        NA
    ## 425  -120.67660 35.26380       Denny's                 <NA>        NA
    ## 426  -117.17990 33.13680       Denny's                 <NA>        NA
    ## 427  -122.28480 37.54590       Denny's                 <NA>        NA
    ## 428  -122.33050 37.95530       Denny's                 <NA>        NA
    ## 429  -117.04420 32.55050       Denny's                 <NA>        NA
    ## 430  -119.55640 36.70870       Denny's                 <NA>        NA
    ## 431  -117.83360 33.75980       Denny's                 <NA>        NA
    ## 432  -117.88540 33.71340       Denny's                 <NA>        NA
    ## 433  -117.88840 33.76010       Denny's                 <NA>        NA
    ## 434  -119.74040 34.44050       Denny's                 <NA>        NA
    ## 435  -121.95640 37.35240       Denny's                 <NA>        NA
    ## 436  -121.99870 37.35230       Denny's                 <NA>        NA
    ## 437  -118.42280 34.42380       Denny's                 <NA>        NA
    ## 438  -122.02350 36.98520       Denny's                 <NA>        NA
    ## 439  -120.41980 34.95300       Denny's                 <NA>        NA
    ## 440  -118.48890 34.01740       Denny's                 <NA>        NA
    ## 441  -119.06920 34.34630       Denny's                 <NA>        NA
    ## 442  -122.72730 38.45990       Denny's                 <NA>        NA
    ## 443  -122.71420 38.42130       Denny's                 <NA>        NA
    ## 444  -116.98800 32.84210       Denny's                 <NA>        NA
    ## 445  -118.08020 33.75950       Denny's                 <NA>        NA
    ## 446  -119.62880 36.57430       Denny's                 <NA>        NA
    ## 447  -118.46620 34.17100       Denny's                 <NA>        NA
    ## 448  -118.74380 34.27980       Denny's                 <NA>        NA
    ## 449  -121.32160 36.41750       Denny's                 <NA>        NA
    ## 450  -118.16450 33.94920       Denny's                 <NA>        NA
    ## 451  -119.97770 38.93440       Denny's                 <NA>        NA
    ## 452  -122.40840 37.65020       Denny's                 <NA>        NA
    ## 453  -117.99270 33.77430       Denny's                 <NA>        NA
    ## 454  -121.29590 37.93720       Denny's                 <NA>        NA
    ## 455  -121.34170 37.98460       Denny's                 <NA>        NA
    ## 456  -121.31300 37.99170       Denny's                 <NA>        NA
    ## 457  -121.24710 37.98650       Denny's                 <NA>        NA
    ## 458  -121.22060 37.90310       Denny's                 <NA>        NA
    ## 459  -118.37030 34.21610       Denny's                 <NA>        NA
    ## 460  -122.02820 37.39580       Denny's                 <NA>        NA
    ## 461  -122.03510 37.37500       Denny's                 <NA>        NA
    ## 462  -118.47800 34.30310       Denny's                 <NA>        NA
    ## 463  -118.42820 34.31080       Denny's                 <NA>        NA
    ## 464  -118.44690 35.14090       Denny's                 <NA>        NA
    ## 465  -117.15350 33.49930       Denny's                 <NA>        NA
    ## 466  -118.07360 34.10150       Denny's                 <NA>        NA
    ## 467  -116.40300 33.81850       Denny's                 <NA>        NA
    ## 468  -118.35320 33.83560       Denny's                 <NA>        NA
    ## 469  -118.34060 33.82430       Denny's                 <NA>        NA
    ## 470  -121.43510 37.76350       Denny's                 <NA>        NA
    ## 471  -119.33700 36.22580       Denny's                 <NA>        NA
    ## 472  -120.84900 37.47560       Denny's                 <NA>        NA
    ## 473  -117.81390 33.73340       Denny's                 <NA>        NA
    ## 474  -116.05500 34.13560       Denny's                 <NA>        NA
    ## 475  -123.19810 39.15150       Denny's                 <NA>        NA
    ## 476  -117.66160 34.10700       Denny's                 <NA>        NA
    ## 477  -121.95870 38.37080       Denny's                 <NA>        NA
    ## 478  -118.58140 34.42230       Denny's                 <NA>        NA
    ## 479  -122.23100 38.12410       Denny's                 <NA>        NA
    ## 480  -122.25620 38.13600       Denny's                 <NA>        NA
    ## 481  -118.44740 34.20120       Denny's                 <NA>        NA
    ## 482  -118.49240 34.20030       Denny's                 <NA>        NA
    ## 483  -118.49410 34.20120       Denny's                 <NA>        NA
    ## 484  -119.18380 34.17910       Denny's                 <NA>        NA
    ## 485  -117.32080 34.50990       Denny's                 <NA>        NA
    ## 486  -119.31370 36.30930       Denny's                 <NA>        NA
    ## 487  -119.34960 36.32900       Denny's                 <NA>        NA
    ## 488  -117.25220 33.19620       Denny's                 <NA>        NA
    ## 489  -121.54870 38.57360       Denny's                 <NA>        NA
    ## 490  -117.87240 34.07350       Denny's                 <NA>        NA
    ## 491  -119.34600 35.60200       Denny's                 <NA>        NA
    ## 492  -117.90780 34.08210       Denny's                 <NA>        NA
    ## 493  -121.26690 37.54070       Denny's                 <NA>        NA
    ## 494  -118.44310 34.06080       Denny's                 <NA>        NA
    ## 495  -118.04940 33.97920       Denny's                 <NA>        NA
    ## 496  -118.08160 33.96830       Denny's                 <NA>        NA
    ## 497  -117.24340 33.59690       Denny's                 <NA>        NA
    ## 498  -118.28350 33.79070       Denny's                 <NA>        NA
    ## 499  -121.78260 38.70020       Denny's                 <NA>        NA
    ## 500  -121.74840 38.67730       Denny's                 <NA>        NA
    ## 501  -118.57210 34.17060       Denny's                 <NA>        NA
    ## 502  -118.60680 34.17200       Denny's                 <NA>        NA
    ## 503  -117.74380 33.87260       Denny's                 <NA>        NA
    ## 504  -117.07220 34.03400       Denny's                 <NA>        NA
    ## 505  -116.42320 34.11990       Denny's                 <NA>        NA
    ## 506  -104.82040 39.72560       Denny's                 <NA>        NA
    ## 507  -104.79300 39.76190       Denny's                 <NA>        NA
    ## 508  -104.86590 39.68940       Denny's                 <NA>        NA
    ## 509  -105.25580 40.00020       Denny's                 <NA>        NA
    ## 510  -102.27880 39.29730       Denny's                 <NA>        NA
    ## 511  -108.45770 39.08810       Denny's                 <NA>        NA
    ## 512  -104.83100 38.83700       Denny's                 <NA>        NA
    ## 513  -104.79970 38.79600       Denny's                 <NA>        NA
    ## 514  -104.75780 38.83730       Denny's                 <NA>        NA
    ## 515  -104.80390 38.94900       Denny's                 <NA>        NA
    ## 516  -108.55970 37.34900       Denny's                 <NA>        NA
    ## 517  -105.02520 39.74340       Denny's                 <NA>        NA
    ## 518  -104.99750 39.71120       Denny's                 <NA>        NA
    ## 519  -104.84760 39.77250       Denny's                 <NA>        NA
    ## 520  -107.88420 37.27140       Denny's                 <NA>        NA
    ## 521  -104.99090 39.65330       Denny's                 <NA>        NA
    ## 522  -105.00750 40.58310       Denny's                 <NA>        NA
    ## 523  -104.69770 38.71830       Denny's                 <NA>        NA
    ## 524  -105.14510 39.76300       Denny's                 <NA>        NA
    ## 525  -108.54410 39.10700       Denny's                 <NA>        NA
    ## 526  -105.00400 39.54830       Denny's                 <NA>        NA
    ## 527  -105.13190 39.72430       Denny's                 <NA>        NA
    ## 528  -105.08150 39.65020       Denny's                 <NA>        NA
    ## 529  -103.71210 39.27030       Denny's                 <NA>        NA
    ## 530  -107.86520 38.45580       Denny's                 <NA>        NA
    ## 531  -104.61320 38.31230       Denny's                 <NA>        NA
    ## 532  -104.98230 39.88510       Denny's                 <NA>        NA
    ## 533  -105.10880 39.78560       Denny's                 <NA>        NA
    ## 534  -105.03760 38.97450       Denny's                 <NA>        NA
    ## 535   -73.41970 41.40570       Denny's                 <NA>        NA
    ## 536   -72.58140 41.99800       Denny's                 <NA>        NA
    ## 537   -72.71340 41.74240       Denny's                 <NA>        NA
    ## 538   -72.64330 41.55450       Denny's                 <NA>        NA
    ## 539   -72.97650 41.33430       Denny's                 <NA>        NA
    ## 540   -72.11870 41.36940       Denny's                 <NA>        NA
    ## 541   -72.87360 41.63440       Denny's                 <NA>        NA
    ## 542   -72.49690 41.82660       Denny's                 <NA>        NA
    ## 543   -73.00790 41.56660       Denny's                 <NA>        NA
    ## 544   -72.97410 41.27090       Denny's                 <NA>        NA
    ## 545   -72.44490 41.28990       Denny's                 <NA>        NA
    ## 546   -72.65470 41.68490       Denny's                 <NA>        NA
    ## 547   -76.97940 38.90630       Denny's                 <NA>        NA
    ## 548   -76.93860 38.89120       Denny's                 <NA>        NA
    ## 549   -75.68250 39.67580       Denny's                 <NA>        NA
    ## 550   -81.38420 28.66210       Denny's                 <NA>        NA
    ## 551   -81.50410 28.67310       Denny's                 <NA>        NA
    ## 552   -81.84350 27.90530       Denny's                 <NA>        NA
    ## 553   -82.69930 28.33300       Denny's                 <NA>        NA
    ## 554   -80.07650 26.38020       Denny's                 <NA>        NA
    ## 555   -80.11210 26.35110       Denny's                 <NA>        NA
    ## 556   -82.56870 27.46240       Denny's                 <NA>        NA
    ## 557   -82.30210 27.93760       Denny's                 <NA>        NA
    ## 558   -81.94080 26.62830       Denny's                 <NA>        NA
    ## 559   -81.33780 28.65880       Denny's                 <NA>        NA
    ## 560   -82.73400 28.01310       Denny's                 <NA>        NA
    ## 561   -81.66430 28.34740       Denny's                 <NA>        NA
    ## 562   -80.61060 28.33270       Denny's                 <NA>        NA
    ## 563   -80.25500 25.74980       Denny's                 <NA>        NA
    ## 564   -80.27500 25.71600       Denny's                 <NA>        NA
    ## 565   -82.61040 28.91930       Denny's                 <NA>        NA
    ## 566   -81.65150 28.23550       Denny's                 <NA>        NA
    ## 567   -80.25190 26.05000       Denny's                 <NA>        NA
    ## 568   -81.02690 29.26460       Denny's                 <NA>        NA
    ## 569   -80.97500 29.16190       Denny's                 <NA>        NA
    ## 570   -81.30420 29.04880       Denny's                 <NA>        NA
    ## 571   -81.30420 29.04880       Denny's                 <NA>        NA
    ## 572   -82.33590 26.93810       Denny's                 <NA>        NA
    ## 573   -81.68980 28.82320       Denny's                 <NA>        NA
    ## 574   -80.47500 25.44280       Denny's                 <NA>        NA
    ## 575   -80.15460 26.16690       Denny's                 <NA>        NA
    ## 576   -80.13690 26.10100       Denny's                 <NA>        NA
    ## 577   -81.88810 26.52170       Denny's                 <NA>        NA
    ## 578   -81.80060 26.54830       Denny's                 <NA>        NA
    ## 579   -80.11460 26.18990       Denny's                 <NA>        NA
    ## 580   -80.39900 27.44870       Denny's                 <NA>        NA
    ## 581   -86.60030 30.43930       Denny's                 <NA>        NA
    ## 582   -81.63960 28.11760       Denny's                 <NA>        NA
    ## 583   -80.16270 25.98500       Denny's                 <NA>        NA
    ## 584   -80.30340 25.86660       Denny's                 <NA>        NA
    ## 585   -80.35770 25.88060       Denny's                 <NA>        NA
    ## 586   -80.16490 26.03390       Denny's                 <NA>        NA
    ## 587   -80.20750 26.00730       Denny's                 <NA>        NA
    ## 588   -82.06140 29.83100       Denny's                 <NA>        NA
    ## 589   -81.45350 30.28770       Denny's                 <NA>        NA
    ## 590   -81.52870 30.32390       Denny's                 <NA>        NA
    ## 591   -81.73930 30.19520       Denny's                 <NA>        NA
    ## 592   -81.57390 30.22010       Denny's                 <NA>        NA
    ## 593   -80.46150 25.07590       Denny's                 <NA>        NA
    ## 594   -81.79950 24.55140       Denny's                 <NA>        NA
    ## 595   -81.42910 28.30460       Denny's                 <NA>        NA
    ## 596   -81.38450 28.34060       Denny's                 <NA>        NA
    ## 597   -81.36060 28.29040       Denny's                 <NA>        NA
    ## 598   -81.47240 28.32940       Denny's                 <NA>        NA
    ## 599   -81.51780 28.33290       Denny's                 <NA>        NA
    ## 600   -81.59220 28.33600       Denny's                 <NA>        NA
    ## 601   -81.62450 27.96470       Denny's                 <NA>        NA
    ## 602   -80.11430 26.57210       Denny's                 <NA>        NA
    ## 603   -81.96780 28.08280       Denny's                 <NA>        NA
    ## 604   -81.95690 27.98700       Denny's                 <NA>        NA
    ## 605   -82.76820 28.08440       Denny's                 <NA>        NA
    ## 606   -81.89640 28.84280       Denny's                 <NA>        NA
    ## 607   -83.36070 30.40120       Denny's                 <NA>        NA
    ## 608   -80.20470 26.23710       Denny's                 <NA>        NA
    ## 609   -80.70210 28.07900       Denny's                 <NA>        NA
    ## 610   -80.69790 28.36140       Denny's                 <NA>        NA
    ## 611   -80.18950 25.81060       Denny's                 <NA>        NA
    ## 612   -80.33340 25.73320       Denny's                 <NA>        NA
    ## 613   -80.35970 25.58880       Denny's                 <NA>        NA
    ## 614   -80.29130 25.80760       Denny's                 <NA>        NA
    ## 615   -80.31540 25.80940       Denny's                 <NA>        NA
    ## 616   -80.35160 25.76910       Denny's                 <NA>        NA
    ## 617   -80.41590 25.73220       Denny's                 <NA>        NA
    ## 618   -80.41440 25.62540       Denny's                 <NA>        NA
    ## 619   -80.38060 25.81150       Denny's                 <NA>        NA
    ## 620   -80.39300 25.68600       Denny's                 <NA>        NA
    ## 621   -80.12050 25.85680       Denny's                 <NA>        NA
    ## 622   -80.24600 25.95420       Denny's                 <NA>        NA
    ## 623   -80.30830 25.94350       Denny's                 <NA>        NA
    ## 624   -84.42870 30.49360       Denny's                 <NA>        NA
    ## 625   -80.17210 25.94470       Denny's                 <NA>        NA
    ## 626   -80.16420 25.88750       Denny's                 <NA>        NA
    ## 627   -81.39810 30.32350       Denny's                 <NA>        NA
    ## 628   -82.73380 28.22380       Denny's                 <NA>        NA
    ## 629   -80.94260 29.01430       Denny's                 <NA>        NA
    ## 630   -80.13500 26.18900       Denny's                 <NA>        NA
    ## 631   -82.18570 29.18640       Denny's                 <NA>        NA
    ## 632   -81.54680 28.55160       Denny's                 <NA>        NA
    ## 633   -81.28360 28.91130       Denny's                 <NA>        NA
    ## 634   -81.31060 28.53760       Denny's                 <NA>        NA
    ## 635   -81.36400 28.45220       Denny's                 <NA>        NA
    ## 636   -81.45980 28.46240       Denny's                 <NA>        NA
    ## 637   -81.47080 28.45260       Denny's                 <NA>        NA
    ## 638   -81.42750 28.44500       Denny's                 <NA>        NA
    ## 639   -81.47120 28.43910       Denny's                 <NA>        NA
    ## 640   -81.46170 28.42500       Denny's                 <NA>        NA
    ## 641   -81.45600 28.40560       Denny's                 <NA>        NA
    ## 642   -81.30930 28.46040       Denny's                 <NA>        NA
    ## 643   -81.20510 28.56670       Denny's                 <NA>        NA
    ## 644   -81.50600 28.38520       Denny's                 <NA>        NA
    ## 645   -81.11320 29.25590       Denny's                 <NA>        NA
    ## 646   -80.64820 28.03520       Denny's                 <NA>        NA
    ## 647   -81.21200 29.55590       Denny's                 <NA>        NA
    ## 648   -80.08850 26.63290       Denny's                 <NA>        NA
    ## 649   -85.64990 30.19260       Denny's                 <NA>        NA
    ## 650   -80.24860 26.02000       Denny's                 <NA>        NA
    ## 651   -80.29060 26.00780       Denny's                 <NA>        NA
    ## 652   -87.27860 30.43670       Denny's                 <NA>        NA
    ## 653   -87.22140 30.50610       Denny's                 <NA>        NA
    ## 654   -87.28130 30.53340       Denny's                 <NA>        NA
    ## 655   -82.10470 28.03510       Denny's                 <NA>        NA
    ## 656   -80.25710 26.14710       Denny's                 <NA>        NA
    ## 657   -80.31020 26.14580       Denny's                 <NA>        NA
    ## 658   -81.45200 28.14630       Denny's                 <NA>        NA
    ## 659   -80.09900 26.26460       Denny's                 <NA>        NA
    ## 660   -82.14450 27.01220       Denny's                 <NA>        NA
    ## 661   -81.02440 29.11540       Denny's                 <NA>        NA
    ## 662   -80.29350 27.27980       Denny's                 <NA>        NA
    ## 663   -80.10210 26.78340       Denny's                 <NA>        NA
    ## 664   -80.23140 26.70510       Denny's                 <NA>        NA
    ## 665   -82.32260 28.32540       Denny's                 <NA>        NA
    ## 666   -81.28680 28.75740       Denny's                 <NA>        NA
    ## 667   -82.49420 27.29880       Denny's                 <NA>        NA
    ## 668   -81.49810 27.52340       Denny's                 <NA>        NA
    ## 669   -82.05460 29.21810       Denny's                 <NA>        NA
    ## 670   -82.63110 28.45500       Denny's                 <NA>        NA
    ## 671   -81.32080 29.90420       Denny's                 <NA>        NA
    ## 672   -81.41390 29.91800       Denny's                 <NA>        NA
    ## 673   -81.34440 29.74890       Denny's                 <NA>        NA
    ## 674   -82.67950 27.81740       Denny's                 <NA>        NA
    ## 675   -80.23570 27.17700       Denny's                 <NA>        NA
    ## 676   -82.37570 27.71230       Denny's                 <NA>        NA
    ## 677   -84.30070 30.47880       Denny's                 <NA>        NA
    ## 678   -84.29740 30.44130       Denny's                 <NA>        NA
    ## 679   -80.25200 26.19530       Denny's                 <NA>        NA
    ## 680   -82.50550 27.95950       Denny's                 <NA>        NA
    ## 681   -82.39340 27.99610       Denny's                 <NA>        NA
    ## 682   -82.44000 28.05460       Denny's                 <NA>        NA
    ## 683   -81.95830 28.95150       Denny's                 <NA>        NA
    ## 684   -80.84780 28.55350       Denny's                 <NA>        NA
    ## 685   -82.49040 27.21670       Denny's                 <NA>        NA
    ## 686   -80.71160 28.23030       Denny's                 <NA>        NA
    ## 687   -80.09510 26.70670       Denny's                 <NA>        NA
    ## 688   -81.71440 28.00400       Denny's                 <NA>        NA
    ## 689   -81.38440 28.60570       Denny's                 <NA>        NA
    ## 690   -82.04630 33.51250       Denny's                 <NA>        NA
    ## 691   -81.58000 31.13990       Denny's                 <NA>        NA
    ## 692   -81.50190 31.24650       Denny's                 <NA>        NA
    ## 693   -83.78900 32.61910       Denny's                 <NA>        NA
    ## 694   -84.94220 32.48120       Denny's                 <NA>        NA
    ## 695   -83.74680 31.95910       Denny's                 <NA>        NA
    ## 696   -83.84530 34.28490       Denny's                 <NA>        NA
    ## 697   -84.75340 33.89110       Denny's                 <NA>        NA
    ## 698   -84.12040 33.22610       Denny's                 <NA>        NA
    ## 699   -81.65340 30.78840       Denny's                 <NA>        NA
    ## 700   -83.18970 30.64320       Denny's                 <NA>        NA
    ## 701   -84.12220 33.35170       Denny's                 <NA>        NA
    ## 702   -84.20650 33.90920       Denny's                 <NA>        NA
    ## 703   -84.93600 34.58710       Denny's                 <NA>        NA
    ## 704   -81.32760 31.93050       Denny's                 <NA>        NA
    ## 705   -81.11310 32.01460       Denny's                 <NA>        NA
    ## 706   -81.28630 32.00600       Denny's                 <NA>        NA
    ## 707   -84.01800 33.86230       Denny's                 <NA>        NA
    ## 708   -84.25360 33.54900       Denny's                 <NA>        NA
    ## 709   -84.04640 34.02830       Denny's                 <NA>        NA
    ## 710   -83.12080 33.69000       Denny's                 <NA>        NA
    ## 711   -83.32930 30.84400       Denny's                 <NA>        NA
    ## 712  -157.83150 21.27850       Denny's                 <NA>        NA
    ## 713  -156.45380 20.88720       Denny's                 <NA>        NA
    ## 714  -155.98890 19.64630       Denny's                 <NA>        NA
    ## 715  -157.79880 21.40280       Denny's                 <NA>        NA
    ## 716  -158.09150 21.32890       Denny's                 <NA>        NA
    ## 717  -158.03380 21.38660       Denny's                 <NA>        NA
    ## 718   -92.35800 41.69270       Denny's                 <NA>        NA
    ## 719   -93.77820 41.61480       Denny's                 <NA>        NA
    ## 720   -90.56370 41.55750       Denny's                 <NA>        NA
    ## 721  -116.21350 43.56950       Denny's                 <NA>        NA
    ## 722  -113.79200 42.56280       Denny's                 <NA>        NA
    ## 723  -116.65380 43.66280       Denny's                 <NA>        NA
    ## 724  -112.46630 42.91130       Denny's                 <NA>        NA
    ## 725  -116.78080 47.69810       Denny's                 <NA>        NA
    ## 726  -112.05170 43.50330       Denny's                 <NA>        NA
    ## 727  -116.35520 43.61950       Denny's                 <NA>        NA
    ## 728  -117.58560 47.09180       Denny's                 <NA>        NA
    ## 729  -116.57320 43.60190       Denny's                 <NA>        NA
    ## 730  -116.92130 47.71210       Denny's                 <NA>        NA
    ## 731  -114.47570 42.59180       Denny's                 <NA>        NA
    ## 732   -90.09750 38.57670       Denny's                 <NA>        NA
    ## 733   -87.98320 42.04430       Denny's                 <NA>        NA
    ## 734   -88.38010 41.77220       Denny's                 <NA>        NA
    ## 735   -89.98310 38.50010       Denny's                 <NA>        NA
    ## 736   -88.95150 40.47350       Denny's                 <NA>        NA
    ## 737   -88.18350 41.52870       Denny's                 <NA>        NA
    ## 738   -87.86160 41.16260       Denny's                 <NA>        NA
    ## 739   -87.55880 41.59600       Denny's                 <NA>        NA
    ## 740   -89.24050 37.73050       Denny's                 <NA>        NA
    ## 741   -88.10310 41.89400       Denny's                 <NA>        NA
    ## 742   -88.29330 42.10570       Denny's                 <NA>        NA
    ## 743   -88.25420 40.14260       Denny's                 <NA>        NA
    ## 744   -90.01150 38.67890       Denny's                 <NA>        NA
    ## 745   -88.56670 39.13990       Denny's                 <NA>        NA
    ## 746   -88.57230 39.13500       Denny's                 <NA>        NA
    ## 747   -87.88440 41.92330       Denny's                 <NA>        NA
    ## 748   -87.99630 40.75590       Denny's                 <NA>        NA
    ## 749   -89.95520 38.77680       Denny's                 <NA>        NA
    ## 750   -87.96120 42.38490       Denny's                 <NA>        NA
    ## 751   -88.10910 41.96720       Denny's                 <NA>        NA
    ## 752   -87.80980 41.71870       Denny's                 <NA>        NA
    ## 753   -87.82600 42.18980       Denny's                 <NA>        NA
    ## 754   -88.07970 42.04920       Denny's                 <NA>        NA
    ## 755   -88.43270 42.13750       Denny's                 <NA>        NA
    ## 756   -88.14960 41.57090       Denny's                 <NA>        NA
    ## 757   -89.09780 41.36780       Denny's                 <NA>        NA
    ## 758   -89.67240 39.17530       Denny's                 <NA>        NA
    ## 759   -88.34620 39.48500       Denny's                 <NA>        NA
    ## 760   -87.83250 41.90840       Denny's                 <NA>        NA
    ## 761   -87.84880 41.54310       Denny's                 <NA>        NA
    ## 762   -90.50040 41.46420       Denny's                 <NA>        NA
    ## 763   -88.95720 38.31270       Denny's                 <NA>        NA
    ## 764   -88.32240 41.79130       Denny's                 <NA>        NA
    ## 765   -89.63570 40.60700       Denny's                 <NA>        NA
    ## 766   -88.99560 40.53040       Denny's                 <NA>        NA
    ## 767   -87.80720 41.96380       Denny's                 <NA>        NA
    ## 768   -89.92630 38.58860       Denny's                 <NA>        NA
    ## 769   -87.74100 41.72520       Denny's                 <NA>        NA
    ## 770   -87.80520 41.89240       Denny's                 <NA>        NA
    ## 771   -87.97280 41.85620       Denny's                 <NA>        NA
    ## 772   -87.85130 41.61860       Denny's                 <NA>        NA
    ## 773   -88.02080 42.13920       Denny's                 <NA>        NA
    ## 774   -90.06560 38.76110       Denny's                 <NA>        NA
    ## 775   -90.07180 38.74650       Denny's                 <NA>        NA
    ## 776   -88.97280 42.27000       Denny's                 <NA>        NA
    ## 777   -88.97650 38.62380       Denny's                 <NA>        NA
    ## 778   -87.86160 41.96660       Denny's                 <NA>        NA
    ## 779   -89.00020 42.49280       Denny's                 <NA>        NA
    ## 780   -89.61020 39.75820       Denny's                 <NA>        NA
    ## 781   -89.70620 39.76400       Denny's                 <NA>        NA
    ## 782   -88.27000 39.79010       Denny's                 <NA>        NA
    ## 783   -88.85730 39.01360       Denny's                 <NA>        NA
    ## 784   -87.94500 42.24270       Denny's                 <NA>        NA
    ## 785   -90.16360 38.33840       Denny's                 <NA>        NA
    ## 786   -87.89820 42.34610       Denny's                 <NA>        NA
    ## 787   -87.94380 41.74790       Denny's                 <NA>        NA
    ## 788   -86.51830 38.86760       Denny's                 <NA>        NA
    ## 789   -86.53430 39.18420       Denny's                 <NA>        NA
    ## 790   -85.77340 38.30780       Denny's                 <NA>        NA
    ## 791   -86.98700 38.18410       Denny's                 <NA>        NA
    ## 792   -85.57300 40.11400       Denny's                 <NA>        NA
    ## 793   -87.55080 38.15570       Denny's                 <NA>        NA
    ## 794   -87.53920 38.01160       Denny's                 <NA>        NA
    ## 795   -87.63810 37.97720       Denny's                 <NA>        NA
    ## 796   -87.49260 37.97990       Denny's                 <NA>        NA
    ## 797   -86.61960 38.55010       Denny's                 <NA>        NA
    ## 798   -87.35600 41.56140       Denny's                 <NA>        NA
    ## 799   -86.07770 39.61560       Denny's                 <NA>        NA
    ## 800   -87.55200 38.17410       Denny's                 <NA>        NA
    ## 801   -87.29560 41.28930       Denny's                 <NA>        NA
    ## 802   -86.19050 39.69240       Denny's                 <NA>        NA
    ## 803   -86.00970 39.80390       Denny's                 <NA>        NA
    ## 804   -86.26510 39.69860       Denny's                 <NA>        NA
    ## 805   -86.26890 39.80070       Denny's                 <NA>        NA
    ## 806   -86.12070 39.71470       Denny's                 <NA>        NA
    ## 807   -86.12770 39.63700       Denny's                 <NA>        NA
    ## 808   -86.05900 39.90540       Denny's                 <NA>        NA
    ## 809   -86.22400 39.91420       Denny's                 <NA>        NA
    ## 810   -86.93790 38.42570       Denny's                 <NA>        NA
    ## 811   -86.82570 40.41770       Denny's                 <NA>        NA
    ## 812   -87.23990 41.58600       Denny's                 <NA>        NA
    ## 813   -86.48570 40.04660       Denny's                 <NA>        NA
    ## 814   -87.33160 41.47140       Denny's                 <NA>        NA
    ## 815   -86.89400 41.66710       Denny's                 <NA>        NA
    ## 816   -87.09190 38.39460       Denny's                 <NA>        NA
    ## 817   -87.17550 41.59850       Denny's                 <NA>        NA
    ## 818   -85.78670 38.67860       Denny's                 <NA>        NA
    ## 819   -85.73770 39.52570       Denny's                 <NA>        NA
    ## 820   -85.41410 39.85230       Denny's                 <NA>        NA
    ## 821   -87.41580 39.42600       Denny's                 <NA>        NA
    ## 822   -87.41400 39.46410       Denny's                 <NA>        NA
    ## 823   -87.50110 38.69130       Denny's                 <NA>        NA
    ## 824   -86.03560 39.55050       Denny's                 <NA>        NA
    ## 825   -96.55690 39.18020       Denny's                 <NA>        NA
    ## 826   -94.68960 39.01420       Denny's                 <NA>        NA
    ## 827   -94.66770 38.93870       Denny's                 <NA>        NA
    ## 828   -95.76180 39.03970       Denny's                 <NA>        NA
    ## 829   -95.68750 39.01010       Denny's                 <NA>        NA
    ## 830   -97.24390 37.67940       Denny's                 <NA>        NA
    ## 831   -97.40940 37.67330       Denny's                 <NA>        NA
    ## 832   -97.28930 37.66460       Denny's                 <NA>        NA
    ## 833   -86.86650 37.39030       Denny's                 <NA>        NA
    ## 834   -82.69890 38.36480       Denny's                 <NA>        NA
    ## 835   -85.82680 37.71500       Denny's                 <NA>        NA
    ## 836   -86.55770 36.66860       Denny's                 <NA>        NA
    ## 837   -87.57540 37.86080       Denny's                 <NA>        NA
    ## 838   -84.51610 38.01270       Denny's                 <NA>        NA
    ## 839   -84.48630 38.09590       Denny's                 <NA>        NA
    ## 840   -85.75160 38.21170       Denny's                 <NA>        NA
    ## 841   -87.49470 37.32760       Denny's                 <NA>        NA
    ## 842   -84.33140 37.37550       Denny's                 <NA>        NA
    ## 843   -87.45550 36.69640       Denny's                 <NA>        NA
    ## 844   -87.12310 37.72630       Denny's                 <NA>        NA
    ## 845   -85.70130 37.99370       Denny's                 <NA>        NA
    ## 846   -85.63340 38.23400       Denny's                 <NA>        NA
    ## 847   -85.06750 38.16220       Denny's                 <NA>        NA
    ## 848   -84.62460 38.85820       Denny's                 <NA>        NA
    ## 849   -93.71480 32.51640       Denny's                 <NA>        NA
    ## 850   -93.97910 32.44360       Denny's                 <NA>        NA
    ## 851   -90.20600 30.00570       Denny's                 <NA>        NA
    ## 852   -93.79820 32.40850       Denny's                 <NA>        NA
    ## 853   -72.57740 42.17070       Denny's                 <NA>        NA
    ## 854   -70.93620 42.54900       Denny's                 <NA>        NA
    ## 855   -71.16190 41.67510       Denny's                 <NA>        NA
    ## 856   -72.62990 42.18810       Denny's                 <NA>        NA
    ## 857   -71.14620 42.68800       Denny's                 <NA>        NA
    ## 858   -71.74240 42.53340       Denny's                 <NA>        NA
    ## 859   -72.50100 42.14050       Denny's                 <NA>        NA
    ## 860   -71.77480 42.29470       Denny's                 <NA>        NA
    ## 861   -76.54430 38.98460       Denny's                 <NA>        NA
    ## 862   -76.54480 39.39920       Denny's                 <NA>        NA
    ## 863   -76.50730 39.37560       Denny's                 <NA>        NA
    ## 864   -76.06100 38.55860       Denny's                 <NA>        NA
    ## 865   -76.93270 38.99380       Denny's                 <NA>        NA
    ## 866   -76.50460 39.27830       Denny's                 <NA>        NA
    ## 867   -76.06010 38.78230       Denny's                 <NA>        NA
    ## 868   -76.30540 39.45310       Denny's                 <NA>        NA
    ## 869   -76.93240 39.39900       Denny's                 <NA>        NA
    ## 870   -76.74540 39.10940       Denny's                 <NA>        NA
    ## 871   -77.45100 39.41850       Denny's                 <NA>        NA
    ## 872   -75.61260 38.32550       Denny's                 <NA>        NA
    ## 873   -76.61410 39.20440       Denny's                 <NA>        NA
    ## 874   -77.69300 39.61760       Denny's                 <NA>        NA
    ## 875   -76.71060 39.16050       Denny's                 <NA>        NA
    ## 876   -76.63740 39.15590       Denny's                 <NA>        NA
    ## 877   -78.83550 39.63710       Denny's                 <NA>        NA
    ## 878   -77.20380 39.17240       Denny's                 <NA>        NA
    ## 879   -75.94810 39.62470       Denny's                 <NA>        NA
    ## 880   -79.40520 39.41130       Denny's                 <NA>        NA
    ## 881   -75.05720 38.41790       Denny's                 <NA>        NA
    ## 882   -76.06720 39.58880       Denny's                 <NA>        NA
    ## 883   -75.54160 38.36620       Denny's                 <NA>        NA
    ## 884   -77.07660 39.10300       Denny's                 <NA>        NA
    ## 885   -76.90530 38.63170       Denny's                 <NA>        NA
    ## 886   -76.98890 39.58460       Denny's                 <NA>        NA
    ## 887   -70.23110 44.09790       Denny's                 <NA>        NA
    ## 888   -69.79260 44.33810       Denny's                 <NA>        NA
    ## 889   -68.73940 44.83150       Denny's                 <NA>        NA
    ## 890   -68.41620 44.53960       Denny's                 <NA>        NA
    ## 891   -70.28490 43.65500       Denny's                 <NA>        NA
    ## 892   -70.32780 43.67660       Denny's                 <NA>        NA
    ## 893   -69.09540 44.12990       Denny's                 <NA>        NA
    ## 894   -83.69430 42.25640       Denny's                 <NA>        NA
    ## 895   -85.20070 42.26100       Denny's                 <NA>        NA
    ## 896   -83.54480 42.21650       Denny's                 <NA>        NA
    ## 897   -84.45730 42.72810       Denny's                 <NA>        NA
    ## 898   -82.45580 43.02530       Denny's                 <NA>        NA
    ## 899   -84.67920 42.78690       Denny's                 <NA>        NA
    ## 900   -85.63750 43.01900       Denny's                 <NA>        NA
    ## 901   -86.07970 42.77590       Denny's                 <NA>        NA
    ## 902   -84.45290 42.27070       Denny's                 <NA>        NA
    ## 903   -85.53190 42.26250       Denny's                 <NA>        NA
    ## 904   -85.57130 42.91250       Denny's                 <NA>        NA
    ## 905   -84.65820 42.74110       Denny's                 <NA>        NA
    ## 906   -84.96520 42.28450       Denny's                 <NA>        NA
    ## 907   -83.36410 41.92570       Denny's                 <NA>        NA
    ## 908   -86.23560 43.18890       Denny's                 <NA>        NA
    ## 909   -83.47600 42.49000       Denny's                 <NA>        NA
    ## 910   -82.91500 42.53090       Denny's                 <NA>        NA
    ## 911   -83.96420 43.47990       Denny's                 <NA>        NA
    ## 912   -82.98610 42.62650       Denny's                 <NA>        NA
    ## 913   -83.23520 42.19820       Denny's                 <NA>        NA
    ## 914   -85.56060 44.74960       Denny's                 <NA>        NA
    ## 915   -85.68210 42.88430       Denny's                 <NA>        NA
    ## 916   -93.22310 44.73190       Denny's                 <NA>        NA
    ## 917   -93.26560 45.13040       Denny's                 <NA>        NA
    ## 918   -93.26290 44.86190       Denny's                 <NA>        NA
    ## 919   -93.33230 44.85760       Denny's                 <NA>        NA
    ## 920   -93.30480 45.07160       Denny's                 <NA>        NA
    ## 921   -93.29070 44.76900       Denny's                 <NA>        NA
    ## 922   -93.35670 45.20750       Denny's                 <NA>        NA
    ## 923   -93.01760 45.03330       Denny's                 <NA>        NA
    ## 924   -92.98540 44.94710       Denny's                 <NA>        NA
    ## 925   -93.23320 44.94840       Denny's                 <NA>        NA
    ## 926   -92.99680 45.50910       Denny's                 <NA>        NA
    ## 927   -93.56430 45.28160       Denny's                 <NA>        NA
    ## 928   -92.48760 44.02660       Denny's                 <NA>        NA
    ## 929   -92.46330 44.00530       Denny's                 <NA>        NA
    ## 930   -93.55020 45.19870       Denny's                 <NA>        NA
    ## 931   -90.37820 38.44460       Denny's                 <NA>        NA
    ## 932   -94.28280 39.12050       Denny's                 <NA>        NA
    ## 933   -93.26320 36.64140       Denny's                 <NA>        NA
    ## 934   -89.55890 37.30450       Denny's                 <NA>        NA
    ## 935   -92.37260 38.96680       Denny's                 <NA>        NA
    ## 936   -90.66260 38.50450       Denny's                 <NA>        NA
    ## 937   -90.46740 38.54230       Denny's                 <NA>        NA
    ## 938   -90.30090 38.80810       Denny's                 <NA>        NA
    ## 939   -90.38730 38.77420       Denny's                 <NA>        NA
    ## 940   -94.38020 39.07460       Denny's                 <NA>        NA
    ## 941   -94.41500 39.04940       Denny's                 <NA>        NA
    ## 942   -94.47850 37.05020       Denny's                 <NA>        NA
    ## 943   -94.51770 37.06600       Denny's                 <NA>        NA
    ## 944   -94.58870 39.09500       Denny's                 <NA>        NA
    ## 945   -94.50120 39.13010       Denny's                 <NA>        NA
    ## 946   -94.54030 39.03620       Denny's                 <NA>        NA
    ## 947   -91.94040 38.94290       Denny's                 <NA>        NA
    ## 948   -90.76760 38.80270       Denny's                 <NA>        NA
    ## 949   -92.64950 37.66810       Denny's                 <NA>        NA
    ## 950   -90.44860 38.71450       Denny's                 <NA>        NA
    ## 951   -89.54270 36.75920       Denny's                 <NA>        NA
    ## 952   -94.55870 39.14560       Denny's                 <NA>        NA
    ## 953   -94.39440 36.84150       Denny's                 <NA>        NA
    ## 954   -90.70590 38.71620       Denny's                 <NA>        NA
    ## 955   -94.45450 38.72450       Denny's                 <NA>        NA
    ## 956   -94.47660 38.99580       Denny's                 <NA>        NA
    ## 957   -91.79120 37.94310       Denny's                 <NA>        NA
    ## 958   -93.25460 38.70520       Denny's                 <NA>        NA
    ## 959   -93.29630 37.12820       Denny's                 <NA>        NA
    ## 960   -90.49290 38.77120       Denny's                 <NA>        NA
    ## 961   -94.79660 39.77700       Denny's                 <NA>        NA
    ## 962   -90.40720 38.70660       Denny's                 <NA>        NA
    ## 963   -90.33910 38.51290       Denny's                 <NA>        NA
    ## 964   -90.40520 38.55560       Denny's                 <NA>        NA
    ## 965   -90.28820 38.62380       Denny's                 <NA>        NA
    ## 966   -90.56690 38.79220       Denny's                 <NA>        NA
    ## 967   -92.15790 37.82520       Denny's                 <NA>        NA
    ## 968   -91.15710 38.22600       Denny's                 <NA>        NA
    ## 969   -90.96850 38.98680       Denny's                 <NA>        NA
    ## 970   -91.22750 38.83980       Denny's                 <NA>        NA
    ## 971   -91.14460 38.81930       Denny's                 <NA>        NA
    ## 972   -91.58040 40.39470       Denny's                 <NA>        NA
    ## 973   -91.04000 33.37150       Denny's                 <NA>        NA
    ## 974   -89.13670 30.42130       Denny's                 <NA>        NA
    ## 975   -88.84190 30.45130       Denny's                 <NA>        NA
    ## 976   -90.16070 32.27670       Denny's                 <NA>        NA
    ## 977   -88.80770 33.45450       Denny's                 <NA>        NA
    ## 978  -108.50790 45.78610       Denny's                 <NA>        NA
    ## 979  -108.56660 45.75460       Denny's                 <NA>        NA
    ## 980  -111.35960 47.47050       Denny's                 <NA>        NA
    ## 981  -114.02530 46.84380       Denny's                 <NA>        NA
    ## 982   -82.58200 35.58880       Denny's                 <NA>        NA
    ## 983   -77.81580 36.06050       Denny's                 <NA>        NA
    ## 984   -82.31900 35.60820       Denny's                 <NA>        NA
    ## 985   -80.85130 35.30690       Denny's                 <NA>        NA
    ## 986   -80.88610 35.16350       Denny's                 <NA>        NA
    ## 987   -80.72860 35.30760       Denny's                 <NA>        NA
    ## 988   -80.71620 35.37030       Denny's                 <NA>        NA
    ## 989   -78.95970 35.92020       Denny's                 <NA>        NA
    ## 990   -78.97500 35.04420       Denny's                 <NA>        NA
    ## 991   -77.39590 35.60520       Denny's                 <NA>        NA
    ## 992   -79.35210 36.07020       Denny's                 <NA>        NA
    ## 993   -78.43460 36.33230       Denny's                 <NA>        NA
    ## 994   -82.44870 35.33230       Denny's                 <NA>        NA
    ## 995   -77.38590 34.78230       Denny's                 <NA>        NA
    ## 996   -78.14520 35.57480       Denny's                 <NA>        NA
    ## 997   -80.09420 36.10910       Denny's                 <NA>        NA
    ## 998   -79.00380 34.66780       Denny's                 <NA>        NA
    ## 999   -80.85670 35.59360       Denny's                 <NA>        NA
    ## 1000  -81.69450 35.71810       Denny's                 <NA>        NA
    ## 1001  -78.65380 35.72040       Denny's                 <NA>        NA
    ## 1002  -78.62080 35.82650       Denny's                 <NA>        NA
    ## 1003  -78.28710 35.51780       Denny's                 <NA>        NA
    ## 1004  -81.48170 35.26660       Denny's                 <NA>        NA
    ## 1005  -77.43010 34.55000       Denny's                 <NA>        NA
    ## 1006  -81.89170 35.33270       Denny's                 <NA>        NA
    ## 1007  -80.07260 35.86220       Denny's                 <NA>        NA
    ## 1008  -77.88620 34.21420       Denny's                 <NA>        NA
    ## 1009  -77.95540 35.73830       Denny's                 <NA>        NA
    ## 1010 -100.78170 46.80200       Denny's                 <NA>        NA
    ## 1011  -96.86020 46.86190       Denny's                 <NA>        NA
    ## 1012  -97.08250 47.88950       Denny's                 <NA>        NA
    ## 1013 -101.29600 48.20350       Denny's                 <NA>        NA
    ## 1014  -98.34280 40.88750       Denny's                 <NA>        NA
    ## 1015  -96.24180 41.13700       Denny's                 <NA>        NA
    ## 1016 -100.72550 41.10180       Denny's                 <NA>        NA
    ## 1017 -101.71490 41.11750       Denny's                 <NA>        NA
    ## 1018  -96.04290 41.22630       Denny's                 <NA>        NA
    ## 1019  -71.49590 42.76740       Denny's                 <NA>        NA
    ## 1020  -71.21850 42.76530       Denny's                 <NA>        NA
    ## 1021  -72.32390 43.62680       Denny's                 <NA>        NA
    ## 1022  -74.29330 40.58270       Denny's                 <NA>        NA
    ## 1023  -74.70160 40.14410       Denny's                 <NA>        NA
    ## 1024  -75.48410 39.68510       Denny's                 <NA>        NA
    ## 1025  -75.03890 39.80340       Denny's                 <NA>        NA
    ## 1026  -74.38110 40.42610       Denny's                 <NA>        NA
    ## 1027  -74.52820 39.44590       Denny's                 <NA>        NA
    ## 1028  -74.55680 39.38620       Denny's                 <NA>        NA
    ## 1029  -75.04720 39.75840       Denny's                 <NA>        NA
    ## 1030  -75.04460 39.48710       Denny's                 <NA>        NA
    ## 1031  -75.04090 39.43570       Denny's                 <NA>        NA
    ## 1032 -105.96020 32.88090       Denny's                 <NA>        NA
    ## 1033 -106.74590 35.07840       Denny's                 <NA>        NA
    ## 1034 -106.62980 35.05920       Denny's                 <NA>        NA
    ## 1035 -106.58640 35.16020       Denny's                 <NA>        NA
    ## 1036 -106.58780 35.10550       Denny's                 <NA>        NA
    ## 1037 -106.51600 35.10890       Denny's                 <NA>        NA
    ## 1038 -106.70570 35.10340       Denny's                 <NA>        NA
    ## 1039 -106.54110 35.31750       Denny's                 <NA>        NA
    ## 1040 -104.23570 32.43570       Denny's                 <NA>        NA
    ## 1041 -103.19650 34.43390       Denny's                 <NA>        NA
    ## 1042 -107.75620 32.26910       Denny's                 <NA>        NA
    ## 1043 -106.19150 35.06950       Denny's                 <NA>        NA
    ## 1044 -108.18970 36.73060       Denny's                 <NA>        NA
    ## 1045 -108.73090 35.51900       Denny's                 <NA>        NA
    ## 1046 -108.75890 35.53410       Denny's                 <NA>        NA
    ## 1047 -107.82700 35.12510       Denny's                 <NA>        NA
    ## 1048 -103.17780 32.75760       Denny's                 <NA>        NA
    ## 1049 -108.43730 35.47850       Denny's                 <NA>        NA
    ## 1050 -106.77770 32.30350       Denny's                 <NA>        NA
    ## 1051 -106.72340 32.38240       Denny's                 <NA>        NA
    ## 1052 -108.98310 32.61110       Denny's                 <NA>        NA
    ## 1053 -106.75680 34.81500       Denny's                 <NA>        NA
    ## 1054 -104.43570 36.88500       Denny's                 <NA>        NA
    ## 1055 -104.52310 33.41870       Denny's                 <NA>        NA
    ## 1056 -105.58700 33.33280       Denny's                 <NA>        NA
    ## 1057 -105.99290 35.65450       Denny's                 <NA>        NA
    ## 1058 -107.24920 33.15540       Denny's                 <NA>        NA
    ## 1059 -103.70250 35.15210       Denny's                 <NA>        NA
    ## 1060 -116.75180 36.91570       Denny's                 <NA>        NA
    ## 1061 -119.76750 39.18150       Denny's                 <NA>        NA
    ## 1062 -115.79060 40.83690       Denny's                 <NA>        NA
    ## 1063 -119.21790 39.61490       Denny's                 <NA>        NA
    ## 1064 -115.03750 36.05620       Denny's                 <NA>        NA
    ## 1065 -115.32910 35.77810       Denny's                 <NA>        NA
    ## 1066 -115.18000 36.26140       Denny's                 <NA>        NA
    ## 1067 -115.14130 36.16950       Denny's                 <NA>        NA
    ## 1068 -115.23230 36.15920       Denny's                 <NA>        NA
    ## 1069 -115.18400 36.10070       Denny's                 <NA>        NA
    ## 1070 -115.21000 36.10080       Denny's                 <NA>        NA
    ## 1071 -115.15370 36.14940       Denny's                 <NA>        NA
    ## 1072 -115.16430 36.13300       Denny's                 <NA>        NA
    ## 1073 -115.13690 36.13390       Denny's                 <NA>        NA
    ## 1074 -115.17190 36.12120       Denny's                 <NA>        NA
    ## 1075 -115.17280 36.10390       Denny's                 <NA>        NA
    ## 1076 -115.06210 36.16450       Denny's                 <NA>        NA
    ## 1077 -115.26550 36.14390       Denny's                 <NA>        NA
    ## 1078 -115.11920 36.10010       Denny's                 <NA>        NA
    ## 1079 -115.17230 36.05830       Denny's                 <NA>        NA
    ## 1080 -115.05720 36.10620       Denny's                 <NA>        NA
    ## 1081 -115.11850 36.02010       Denny's                 <NA>        NA
    ## 1082 -115.25230 36.19600       Denny's                 <NA>        NA
    ## 1083 -115.24940 36.24050       Denny's                 <NA>        NA
    ## 1084 -115.29480 36.10000       Denny's                 <NA>        NA
    ## 1085 -115.14000 36.17190       Denny's                 <NA>        NA
    ## 1086 -114.57590 35.15030       Denny's                 <NA>        NA
    ## 1087 -115.12650 36.21850       Denny's                 <NA>        NA
    ## 1088 -115.19810 36.23920       Denny's                 <NA>        NA
    ## 1089 -115.19660 36.20030       Denny's                 <NA>        NA
    ## 1090 -115.98780 36.21110       Denny's                 <NA>        NA
    ## 1091 -115.38740 35.61140       Denny's                 <NA>        NA
    ## 1092 -119.78380 39.50590       Denny's                 <NA>        NA
    ## 1093 -119.80330 39.53560       Denny's                 <NA>        NA
    ## 1094 -119.74560 39.53420       Denny's                 <NA>        NA
    ## 1095  -73.80780 42.71780       Denny's                 <NA>        NA
    ## 1096  -78.81720 42.99090       Denny's                 <NA>        NA
    ## 1097  -76.54920 42.94640       Denny's                 <NA>        NA
    ## 1098  -78.20170 43.00440       Denny's                 <NA>        NA
    ## 1099  -75.89600 42.15730       Denny's                 <NA>        NA
    ## 1100  -73.89010 40.65800       Denny's                 <NA>        NA
    ## 1101  -78.86810 42.94440       Denny's                 <NA>        NA
    ## 1102  -78.78570 42.96350       Denny's                 <NA>        NA
    ## 1103  -76.26900 43.04130       Denny's                 <NA>        NA
    ## 1104  -77.26270 42.87840       Denny's                 <NA>        NA
    ## 1105  -73.60250 40.74540       Denny's                 <NA>        NA
    ## 1106  -73.07430 40.86000       Denny's                 <NA>        NA
    ## 1107  -78.71560 42.93470       Denny's                 <NA>        NA
    ## 1108  -76.12490 43.15110       Denny's                 <NA>        NA
    ## 1109  -78.40580 42.95750       Denny's                 <NA>        NA
    ## 1110  -76.16720 42.60660       Denny's                 <NA>        NA
    ## 1111  -73.70120 42.62320       Denny's                 <NA>        NA
    ## 1112  -77.44440 43.06820       Denny's                 <NA>        NA
    ## 1113  -73.67580 40.70750       Denny's                 <NA>        NA
    ## 1114  -79.30960 42.45360       Denny's                 <NA>        NA
    ## 1115  -77.78870 42.79710       Denny's                 <NA>        NA
    ## 1116  -77.01370 42.85690       Denny's                 <NA>        NA
    ## 1117  -78.85410 42.74910       Denny's                 <NA>        NA
    ## 1118  -74.99250 43.01880       Denny's                 <NA>        NA
    ## 1119  -76.83720 42.15880       Denny's                 <NA>        NA
    ## 1120  -76.51130 42.42640       Denny's                 <NA>        NA
    ## 1121  -73.88060 40.75600       Denny's                 <NA>        NA
    ## 1122  -78.69700 42.86750       Denny's                 <NA>        NA
    ## 1123  -73.77540 42.75590       Denny's                 <NA>        NA
    ## 1124  -78.69690 43.14870       Denny's                 <NA>        NA
    ## 1125  -74.38670 41.45580       Denny's                 <NA>        NA
    ## 1126  -76.14400 43.11510       Denny's                 <NA>        NA
    ## 1127  -75.31600 43.07920       Denny's                 <NA>        NA
    ## 1128  -74.00590 40.71160       Denny's                 <NA>        NA
    ## 1129  -74.06830 41.50710       Denny's                 <NA>        NA
    ## 1130  -79.06270 43.08940       Denny's                 <NA>        NA
    ## 1131  -78.97150 43.08930       Denny's                 <NA>        NA
    ## 1132  -73.32030 40.73540       Denny's                 <NA>        NA
    ## 1133  -75.64110 43.07700       Denny's                 <NA>        NA
    ## 1134  -75.04830 42.44820       Denny's                 <NA>        NA
    ## 1135  -78.74730 42.79880       Denny's                 <NA>        NA
    ## 1136  -77.10700 42.17120       Denny's                 <NA>        NA
    ## 1137  -73.65020 43.33230       Denny's                 <NA>        NA
    ## 1138  -77.61750 43.08720       Denny's                 <NA>        NA
    ## 1139  -77.70540 43.21030       Denny's                 <NA>        NA
    ## 1140  -75.45880 43.20950       Denny's                 <NA>        NA
    ## 1141  -73.74460 43.09810       Denny's                 <NA>        NA
    ## 1142  -73.93390 42.81370       Denny's                 <NA>        NA
    ## 1143  -76.09220 43.08980       Denny's                 <NA>        NA
    ## 1144  -76.17120 43.09340       Denny's                 <NA>        NA
    ## 1145  -76.06990 43.04710       Denny's                 <NA>        NA
    ## 1146  -75.21820 43.11010       Denny's                 <NA>        NA
    ## 1147  -75.97680 42.09580       Denny's                 <NA>        NA
    ## 1148  -77.44700 43.00870       Denny's                 <NA>        NA
    ## 1149  -78.78310 42.82880       Denny's                 <NA>        NA
    ## 1150  -75.93910 43.97690       Denny's                 <NA>        NA
    ## 1151  -81.48520 41.11660       Denny's                 <NA>        NA
    ## 1152  -81.49230 40.99150       Denny's                 <NA>        NA
    ## 1153  -82.20890 41.41750       Denny's                 <NA>        NA
    ## 1154  -82.26560 40.85790       Denny's                 <NA>        NA
    ## 1155  -80.85490 41.78810       Denny's                 <NA>        NA
    ## 1156  -80.74860 41.10010       Denny's                 <NA>        NA
    ## 1157  -83.96870 40.83310       Denny's                 <NA>        NA
    ## 1158  -82.92260 40.26630       Denny's                 <NA>        NA
    ## 1159  -80.66730 41.02440       Denny's                 <NA>        NA
    ## 1160  -81.57590 40.00460       Denny's                 <NA>        NA
    ## 1161  -81.43230 40.79540       Denny's                 <NA>        NA
    ## 1162  -81.61060 41.50850       Denny's                 <NA>        NA
    ## 1163  -81.73190 41.41870       Denny's                 <NA>        NA
    ## 1164  -81.80120 41.43650       Denny's                 <NA>        NA
    ## 1165  -83.02570 40.02220       Denny's                 <NA>        NA
    ## 1166  -84.18700 39.74200       Denny's                 <NA>        NA
    ## 1167  -84.06420 39.78010       Denny's                 <NA>        NA
    ## 1168  -82.11310 41.39940       Denny's                 <NA>        NA
    ## 1169  -83.67060 41.05980       Denny's                 <NA>        NA
    ## 1170  -83.13380 41.36380       Denny's                 <NA>        NA
    ## 1171  -81.45560 41.53900       Denny's                 <NA>        NA
    ## 1172  -80.56840 41.18190       Denny's                 <NA>        NA
    ## 1173  -81.64870 41.39570       Denny's                 <NA>        NA
    ## 1174  -83.53630 39.64520       Denny's                 <NA>        NA
    ## 1175  -82.59040 40.77190       Denny's                 <NA>        NA
    ## 1176  -83.10390 40.58070       Denny's                 <NA>        NA
    ## 1177  -81.79820 41.13640       Denny's                 <NA>        NA
    ## 1178  -81.36590 41.65960       Denny's                 <NA>        NA
    ## 1179  -82.60060 39.93800       Denny's                 <NA>        NA
    ## 1180  -81.47300 40.49340       Denny's                 <NA>        NA
    ## 1181  -81.42080 40.85960       Denny's                 <NA>        NA
    ## 1182  -81.90890 41.42080       Denny's                 <NA>        NA
    ## 1183  -81.74080 41.38050       Denny's                 <NA>        NA
    ## 1184  -83.45900 41.53440       Denny's                 <NA>        NA
    ## 1185  -83.56460 41.58690       Denny's                 <NA>        NA
    ## 1186  -82.68290 41.42710       Denny's                 <NA>        NA
    ## 1187  -80.86780 40.07550       Denny's                 <NA>        NA
    ## 1188  -81.34540 41.24820       Denny's                 <NA>        NA
    ## 1189  -83.19140 41.11320       Denny's                 <NA>        NA
    ## 1190  -83.70870 41.67430       Denny's                 <NA>        NA
    ## 1191  -81.44050 41.62480       Denny's                 <NA>        NA
    ## 1192  -80.66500 41.15360       Denny's                 <NA>        NA
    ## 1193  -80.64860 41.10610       Denny's                 <NA>        NA
    ## 1194  -81.89780 39.96550       Denny's                 <NA>        NA
    ## 1195  -97.16490 34.17340       Denny's                 <NA>        NA
    ## 1196  -95.53110 35.47190       Denny's                 <NA>        NA
    ## 1197  -97.46570 35.65280       Denny's                 <NA>        NA
    ## 1198  -97.97260 35.50580       Denny's                 <NA>        NA
    ## 1199  -95.76850 34.93790       Denny's                 <NA>        NA
    ## 1200  -95.40260 35.74710       Denny's                 <NA>        NA
    ## 1201  -97.60120 35.46390       Denny's                 <NA>        NA
    ## 1202  -97.48620 35.43080       Denny's                 <NA>        NA
    ## 1203  -97.37080 35.43440       Denny's                 <NA>        NA
    ## 1204  -97.54620 35.39170       Denny's                 <NA>        NA
    ## 1205  -95.33940 36.22820       Denny's                 <NA>        NA
    ## 1206  -99.62470 35.30570       Denny's                 <NA>        NA
    ## 1207  -96.91200 35.38490       Denny's                 <NA>        NA
    ## 1208  -95.90460 36.16160       Denny's                 <NA>        NA
    ## 1209  -95.83340 36.16270       Denny's                 <NA>        NA
    ## 1210 -123.05990 44.62860       Denny's                 <NA>        NA
    ## 1211 -122.67880 45.26740       Denny's                 <NA>        NA
    ## 1212 -122.56880 45.40880       Denny's                 <NA>        NA
    ## 1213 -123.04080 44.03440       Denny's                 <NA>        NA
    ## 1214 -123.32240 42.46230       Denny's                 <NA>        NA
    ## 1215 -122.57910 45.43520       Denny's                 <NA>        NA
    ## 1216 -119.25820 45.81530       Denny's                 <NA>        NA
    ## 1217 -121.75280 42.20920       Denny's                 <NA>        NA
    ## 1218 -118.07010 45.33290       Denny's                 <NA>        NA
    ## 1219 -122.87410 42.34960       Denny's                 <NA>        NA
    ## 1220 -123.28800 43.53890       Denny's                 <NA>        NA
    ## 1221 -116.94350 44.02530       Denny's                 <NA>        NA
    ## 1222 -118.80570 45.66090       Denny's                 <NA>        NA
    ## 1223 -122.55610 45.51870       Denny's                 <NA>        NA
    ## 1224 -122.68130 45.60930       Denny's                 <NA>        NA
    ## 1225 -122.78280 45.45620       Denny's                 <NA>        NA
    ## 1226 -122.66140 45.53080       Denny's                 <NA>        NA
    ## 1227 -123.35210 43.21280       Denny's                 <NA>        NA
    ## 1228 -122.99710 44.91980       Denny's                 <NA>        NA
    ## 1229 -122.98740 44.95020       Denny's                 <NA>        NA
    ## 1230 -123.04070 44.08260       Denny's                 <NA>        NA
    ## 1231 -121.20580 45.61010       Denny's                 <NA>        NA
    ## 1232 -123.84430 45.47280       Denny's                 <NA>        NA
    ## 1233 -122.87600 45.15100       Denny's                 <NA>        NA
    ## 1234  -75.43030 40.64120       Denny's                 <NA>        NA
    ## 1235  -78.40600 40.47000       Denny's                 <NA>        NA
    ## 1236  -78.50170 40.01680       Denny's                 <NA>        NA
    ## 1237  -79.84280 40.13750       Denny's                 <NA>        NA
    ## 1238  -76.42910 41.02580       Denny's                 <NA>        NA
    ## 1239  -78.24030 39.99940       Denny's                 <NA>        NA
    ## 1240  -79.09820 41.17340       Denny's                 <NA>        NA
    ## 1241  -77.12390 40.23410       Denny's                 <NA>        NA
    ## 1242  -77.65020 39.91060       Denny's                 <NA>        NA
    ## 1243  -75.29460 39.93030       Denny's                 <NA>        NA
    ## 1244  -80.10320 40.68550       Denny's                 <NA>        NA
    ## 1245  -75.30420 39.86690       Denny's                 <NA>        NA
    ## 1246  -79.57610 40.30610       Denny's                 <NA>        NA
    ## 1247  -76.02490 40.98190       Denny's                 <NA>        NA
    ## 1248  -80.47040 41.23340       Denny's                 <NA>        NA
    ## 1249  -79.69460 40.32140       Denny's                 <NA>        NA
    ## 1250  -78.84750 40.27980       Denny's                 <NA>        NA
    ## 1251  -76.36440 40.03970       Denny's                 <NA>        NA
    ## 1252  -74.90530 40.16330       Denny's                 <NA>        NA
    ## 1253  -79.38200 40.28620       Denny's                 <NA>        NA
    ## 1254  -76.98420 40.24030       Denny's                 <NA>        NA
    ## 1255  -77.52300 41.02630       Denny's                 <NA>        NA
    ## 1256  -79.77280 40.43830       Denny's                 <NA>        NA
    ## 1257  -79.78040 40.36820       Denny's                 <NA>        NA
    ## 1258  -79.75200 40.59220       Denny's                 <NA>        NA
    ## 1259  -75.68110 41.82420       Denny's                 <NA>        NA
    ## 1260  -80.07280 40.39120       Denny's                 <NA>        NA
    ## 1261  -79.95140 40.33770       Denny's                 <NA>        NA
    ## 1262  -80.01140 40.53810       Denny's                 <NA>        NA
    ## 1263  -79.83470 40.53780       Denny's                 <NA>        NA
    ## 1264  -75.75510 41.31510       Denny's                 <NA>        NA
    ## 1265  -75.65470 41.46230       Denny's                 <NA>        NA
    ## 1266  -76.84620 40.82480       Denny's                 <NA>        NA
    ## 1267  -79.73320 40.17030       Denny's                 <NA>        NA
    ## 1268  -77.83210 40.78500       Denny's                 <NA>        NA
    ## 1269  -79.94400 40.34840       Denny's                 <NA>        NA
    ## 1270  -80.27410 40.16450       Denny's                 <NA>        NA
    ## 1271  -75.85490 41.25020       Denny's                 <NA>        NA
    ## 1272  -76.97110 41.25020       Denny's                 <NA>        NA
    ## 1273  -76.75810 39.97860       Denny's                 <NA>        NA
    ## 1274  -71.55000 41.66150       Denny's                 <NA>        NA
    ## 1275  -71.45520 41.76930       Denny's                 <NA>        NA
    ## 1276  -71.50150 41.82450       Denny's                 <NA>        NA
    ## 1277  -71.54560 41.97630       Denny's                 <NA>        NA
    ## 1278  -71.50040 41.68250       Denny's                 <NA>        NA
    ## 1279  -82.67050 34.54640       Denny's                 <NA>        NA
    ## 1280  -81.50920 35.14610       Denny's                 <NA>        NA
    ## 1281  -81.95600 35.01110       Denny's                 <NA>        NA
    ## 1282  -81.02270 34.06440       Denny's                 <NA>        NA
    ## 1283  -81.15460 34.07550       Denny's                 <NA>        NA
    ## 1284  -82.34440 34.89360       Denny's                 <NA>        NA
    ## 1285  -78.89710 33.68060       Denny's                 <NA>        NA
    ## 1286  -78.99030 33.64110       Denny's                 <NA>        NA
    ## 1287  -80.04430 32.93550       Denny's                 <NA>        NA
    ## 1288  -78.68590 33.82340       Denny's                 <NA>        NA
    ## 1289  -80.02850 32.86900       Denny's                 <NA>        NA
    ## 1290  -81.00680 34.85480       Denny's                 <NA>        NA
    ## 1291  -81.99300 34.92070       Denny's                 <NA>        NA
    ## 1292  -80.60260 33.19480       Denny's                 <NA>        NA
    ## 1293  -78.97270 33.62110       Denny's                 <NA>        NA
    ## 1294  -81.10870 34.00350       Denny's                 <NA>        NA
    ## 1295  -80.85530 32.69640       Denny's                 <NA>        NA
    ## 1296 -103.20320 44.10700       Denny's                 <NA>        NA
    ## 1297  -96.67760 43.54550       Denny's                 <NA>        NA
    ## 1298  -96.76600 43.60180       Denny's                 <NA>        NA
    ## 1299  -84.86810 35.21320       Denny's                 <NA>        NA
    ## 1300  -87.17160 36.02530       Denny's                 <NA>        NA
    ## 1301  -88.82360 35.67500       Denny's                 <NA>        NA
    ## 1302  -84.23740 35.88000       Denny's                 <NA>        NA
    ## 1303  -89.82760 35.04910       Denny's                 <NA>        NA
    ## 1304  -83.57800 35.80860       Denny's                 <NA>        NA
    ## 1305  -83.54970 35.78120       Denny's                 <NA>        NA
    ## 1306  -99.64350 32.43940       Denny's                 <NA>        NA
    ## 1307  -99.77280 32.41060       Denny's                 <NA>        NA
    ## 1308  -96.67750 33.10410       Denny's                 <NA>        NA
    ## 1309 -101.81950 35.19200       Denny's                 <NA>        NA
    ## 1310 -101.86630 35.19260       Denny's                 <NA>        NA
    ## 1311 -101.72450 35.19320       Denny's                 <NA>        NA
    ## 1312 -106.58080 31.99760       Denny's                 <NA>        NA
    ## 1313  -97.06380 32.75620       Denny's                 <NA>        NA
    ## 1314  -97.18350 32.64630       Denny's                 <NA>        NA
    ## 1315  -97.13410 32.66710       Denny's                 <NA>        NA
    ## 1316  -97.73030 30.27650       Denny's                 <NA>        NA
    ## 1317  -97.74190 30.23340       Denny's                 <NA>        NA
    ## 1318  -97.69160 30.21780       Denny's                 <NA>        NA
    ## 1319  -97.70490 30.33240       Denny's                 <NA>        NA
    ## 1320  -95.03450 29.79620       Denny's                 <NA>        NA
    ## 1321  -94.98190 29.80560       Denny's                 <NA>        NA
    ## 1322 -101.45420 32.26000       Denny's                 <NA>        NA
    ## 1323  -98.72320 29.78050       Denny's                 <NA>        NA
    ## 1324  -96.39550 30.13780       Denny's                 <NA>        NA
    ## 1325  -95.95130 29.78120       Denny's                 <NA>        NA
    ## 1326  -97.50400 25.94280       Denny's                 <NA>        NA
    ## 1327  -97.48370 25.98830       Denny's                 <NA>        NA
    ## 1328  -96.42680 30.72570       Denny's                 <NA>        NA
    ## 1329  -97.23050 32.70040       Denny's                 <NA>        NA
    ## 1330  -95.86240 32.55460       Denny's                 <NA>        NA
    ## 1331  -94.31910 32.09720       Denny's                 <NA>        NA
    ## 1332  -98.89970 32.37490       Denny's                 <NA>        NA
    ## 1333  -96.33420 30.62720       Denny's                 <NA>        NA
    ## 1334  -95.47800 30.33770       Denny's                 <NA>        NA
    ## 1335  -97.17760 33.28910       Denny's                 <NA>        NA
    ## 1336  -97.45260 27.80340       Denny's                 <NA>        NA
    ## 1337  -97.63110 27.85490       Denny's                 <NA>        NA
    ## 1338  -97.38220 27.71400       Denny's                 <NA>        NA
    ## 1339  -96.44500 32.07620       Denny's                 <NA>        NA
    ## 1340  -95.69550 29.97270       Denny's                 <NA>        NA
    ## 1341  -96.78710 32.81920       Denny's                 <NA>        NA
    ## 1342  -96.83930 32.80760       Denny's                 <NA>        NA
    ## 1343  -96.82470 32.79750       Denny's                 <NA>        NA
    ## 1344  -96.87390 32.76600       Denny's                 <NA>        NA
    ## 1345  -96.69930 32.79450       Denny's                 <NA>        NA
    ## 1346  -96.89640 32.88110       Denny's                 <NA>        NA
    ## 1347  -96.77010 32.88880       Denny's                 <NA>        NA
    ## 1348  -96.89130 32.66170       Denny's                 <NA>        NA
    ## 1349  -96.75050 32.65650       Denny's                 <NA>        NA
    ## 1350  -96.75320 32.93480       Denny's                 <NA>        NA
    ## 1351  -96.71850 32.89910       Denny's                 <NA>        NA
    ## 1352  -96.83030 32.99760       Denny's                 <NA>        NA
    ## 1353  -97.17510 33.23180       Denny's                 <NA>        NA
    ## 1354  -98.18700 26.30500       Denny's                 <NA>        NA
    ## 1355  -98.14450 26.33540       Denny's                 <NA>        NA
    ## 1356 -106.40900 31.78110       Denny's                 <NA>        NA
    ## 1357 -106.32150 31.70240       Denny's                 <NA>        NA
    ## 1358 -106.43510 31.89850       Denny's                 <NA>        NA
    ## 1359 -106.41910 31.88380       Denny's                 <NA>        NA
    ## 1360 -106.39690 31.78950       Denny's                 <NA>        NA
    ## 1361 -106.23930 31.66000       Denny's                 <NA>        NA
    ## 1362 -106.33310 31.74540       Denny's                 <NA>        NA
    ## 1363 -106.30190 31.76940       Denny's                 <NA>        NA
    ## 1364 -106.43700 31.92250       Denny's                 <NA>        NA
    ## 1365  -96.61760 32.33650       Denny's                 <NA>        NA
    ## 1366  -97.07060 32.83730       Denny's                 <NA>        NA
    ## 1367  -97.27600 32.66090       Denny's                 <NA>        NA
    ## 1368  -97.77940 31.15290       Denny's                 <NA>        NA
    ## 1369  -95.14450 29.54290       Denny's                 <NA>        NA
    ## 1370  -96.83970 33.15250       Denny's                 <NA>        NA
    ## 1371  -97.32120 32.68330       Denny's                 <NA>        NA
    ## 1372  -97.43250 32.72150       Denny's                 <NA>        NA
    ## 1373  -97.39370 32.67970       Denny's                 <NA>        NA
    ## 1374  -97.31370 32.86020       Denny's                 <NA>        NA
    ## 1375  -94.77970 29.29660       Denny's                 <NA>        NA
    ## 1376  -96.66140 32.86480       Denny's                 <NA>        NA
    ## 1377  -96.59430 32.84100       Denny's                 <NA>        NA
    ## 1378  -97.00290 32.67330       Denny's                 <NA>        NA
    ## 1379  -96.13030 32.33080       Denny's                 <NA>        NA
    ## 1380  -97.71680 26.19690       Denny's                 <NA>        NA
    ## 1381  -96.04770 30.10730       Denny's                 <NA>        NA
    ## 1382  -94.78560 32.15110       Denny's                 <NA>        NA
    ## 1383  -95.37620 29.72170       Denny's                 <NA>        NA
    ## 1384  -95.43030 29.77840       Denny's                 <NA>        NA
    ## 1385  -95.43950 29.81000       Denny's                 <NA>        NA
    ## 1386  -95.42970 29.67840       Denny's                 <NA>        NA
    ## 1387  -95.22700 29.77220       Denny's                 <NA>        NA
    ## 1388  -95.37130 29.93860       Denny's                 <NA>        NA
    ## 1389  -95.21710 29.61760       Denny's                 <NA>        NA
    ## 1390  -95.25130 29.65170       Denny's                 <NA>        NA
    ## 1391  -95.41130 29.93020       Denny's                 <NA>        NA
    ## 1392  -95.49490 29.84340       Denny's                 <NA>        NA
    ## 1393  -95.53420 29.78510       Denny's                 <NA>        NA
    ## 1394  -95.10630 29.57860       Denny's                 <NA>        NA
    ## 1395  -95.54530 29.95540       Denny's                 <NA>        NA
    ## 1396  -95.61170 29.91870       Denny's                 <NA>        NA
    ## 1397  -95.55680 29.70610       Denny's                 <NA>        NA
    ## 1398  -95.49410 29.72070       Denny's                 <NA>        NA
    ## 1399  -95.59820 29.73620       Denny's                 <NA>        NA
    ## 1400  -95.57570 29.78230       Denny's                 <NA>        NA
    ## 1401  -95.49750 29.71510       Denny's                 <NA>        NA
    ## 1402  -95.51420 29.73750       Denny's                 <NA>        NA
    ## 1403  -95.64570 29.84980       Denny's                 <NA>        NA
    ## 1404  -95.29750 29.70360       Denny's                 <NA>        NA
    ## 1405  -95.40970 29.87140       Denny's                 <NA>        NA
    ## 1406  -95.42620 29.99880       Denny's                 <NA>        NA
    ## 1407  -95.45640 30.01110       Denny's                 <NA>        NA
    ## 1408  -95.47300 29.82800       Denny's                 <NA>        NA
    ## 1409  -95.26980 29.99970       Denny's                 <NA>        NA
    ## 1410  -95.32110 30.00800       Denny's                 <NA>        NA
    ## 1411  -95.24350 29.93500       Denny's                 <NA>        NA
    ## 1412  -95.56900 30.71820       Denny's                 <NA>        NA
    ## 1413  -97.00690 32.83740       Denny's                 <NA>        NA
    ## 1414  -97.00790 32.91890       Denny's                 <NA>        NA
    ## 1415  -95.26040 31.94740       Denny's                 <NA>        NA
    ## 1416  -97.60010 30.83830       Denny's                 <NA>        NA
    ## 1417  -95.71900 29.78980       Denny's                 <NA>        NA
    ## 1418  -95.80920 29.78110       Denny's                 <NA>        NA
    ## 1419  -95.77640 29.71510       Denny's                 <NA>        NA
    ## 1420  -96.30940 32.57430       Denny's                 <NA>        NA
    ## 1421  -99.14490 30.04360       Denny's                 <NA>        NA
    ## 1422  -94.86450 32.42850       Denny's                 <NA>        NA
    ## 1423  -97.74710 31.11020       Denny's                 <NA>        NA
    ## 1424  -95.25410 30.04510       Denny's                 <NA>        NA
    ## 1425  -95.03110 29.65400       Denny's                 <NA>        NA
    ## 1426  -95.45370 29.03990       Denny's                 <NA>        NA
    ## 1427  -97.40910 32.81490       Denny's                 <NA>        NA
    ## 1428  -99.50340 27.53180       Denny's                 <NA>        NA
    ## 1429  -99.46390 27.68600       Denny's                 <NA>        NA
    ## 1430  -95.08890 29.49450       Denny's                 <NA>        NA
    ## 1431  -96.97540 33.00830       Denny's                 <NA>        NA
    ## 1432  -98.34720 29.55520       Denny's                 <NA>        NA
    ## 1433  -94.70740 32.45530       Denny's                 <NA>        NA
    ## 1434 -101.85520 33.59010       Denny's                 <NA>        NA
    ## 1435 -101.92240 33.55140       Denny's                 <NA>        NA
    ## 1436  -95.55980 30.22390       Denny's                 <NA>        NA
    ## 1437  -97.08160 32.57700       Denny's                 <NA>        NA
    ## 1438  -98.23180 26.19330       Denny's                 <NA>        NA
    ## 1439  -98.21040 26.23780       Denny's                 <NA>        NA
    ## 1440  -96.63780 33.21290       Denny's                 <NA>        NA
    ## 1441  -95.57880 29.64610       Denny's                 <NA>        NA
    ## 1442  -96.59650 32.79020       Denny's                 <NA>        NA
    ## 1443  -96.62510 32.81140       Denny's                 <NA>        NA
    ## 1444 -102.11230 31.98020       Denny's                 <NA>        NA
    ## 1445  -98.28840 26.19600       Denny's                 <NA>        NA
    ## 1446  -95.56510 29.56650       Denny's                 <NA>        NA
    ## 1447  -96.61820 33.01140       Denny's                 <NA>        NA
    ## 1448  -97.20730 32.82380       Denny's                 <NA>        NA
    ## 1449  -94.67820 31.65260       Denny's                 <NA>        NA
    ## 1450  -94.45720 33.47020       Denny's                 <NA>        NA
    ## 1451  -98.09170 29.70230       Denny's                 <NA>        NA
    ## 1452  -95.19550 30.19410       Denny's                 <NA>        NA
    ## 1453 -102.38500 31.88240       Denny's                 <NA>        NA
    ## 1454  -93.82050 30.11580       Denny's                 <NA>        NA
    ## 1455  -95.65520 31.74480       Denny's                 <NA>        NA
    ## 1456  -95.54220 33.68100       Denny's                 <NA>        NA
    ## 1457  -95.18110 29.66520       Denny's                 <NA>        NA
    ## 1458  -95.22010 29.70980       Denny's                 <NA>        NA
    ## 1459  -95.30000 29.56350       Denny's                 <NA>        NA
    ## 1460 -103.48740 31.39990       Denny's                 <NA>        NA
    ## 1461  -96.70950 33.02520       Denny's                 <NA>        NA
    ## 1462  -96.79540 33.01700       Denny's                 <NA>        NA
    ## 1463  -96.82160 32.52900       Denny's                 <NA>        NA
    ## 1464  -98.79070 26.36450       Denny's                 <NA>        NA
    ## 1465  -97.22880 32.98750       Denny's                 <NA>        NA
    ## 1466  -96.46720 32.89830       Denny's                 <NA>        NA
    ## 1467  -95.80520 29.53260       Denny's                 <NA>        NA
    ## 1468  -97.69280 30.53790       Denny's                 <NA>        NA
    ## 1469  -96.33200 32.96300       Denny's                 <NA>        NA
    ## 1470 -100.44070 31.44280       Denny's                 <NA>        NA
    ## 1471  -98.55120 29.48990       Denny's                 <NA>        NA
    ## 1472  -98.48320 29.42270       Denny's                 <NA>        NA
    ## 1473  -98.50680 29.51820       Denny's                 <NA>        NA
    ## 1474  -98.40300 29.48240       Denny's                 <NA>        NA
    ## 1475  -98.64930 29.41110       Denny's                 <NA>        NA
    ## 1476  -98.52570 29.35630       Denny's                 <NA>        NA
    ## 1477  -98.62720 29.40310       Denny's                 <NA>        NA
    ## 1478  -98.56120 29.52900       Denny's                 <NA>        NA
    ## 1479  -98.48210 29.56650       Denny's                 <NA>        NA
    ## 1480  -98.62520 29.46440       Denny's                 <NA>        NA
    ## 1481  -98.62850 29.45360       Denny's                 <NA>        NA
    ## 1482  -98.36070 29.44570       Denny's                 <NA>        NA
    ## 1483  -98.36790 29.47720       Denny's                 <NA>        NA
    ## 1484  -98.27890 29.59990       Denny's                 <NA>        NA
    ## 1485  -96.53730 32.64810       Denny's                 <NA>        NA
    ## 1486  -97.16490 26.09050       Denny's                 <NA>        NA
    ## 1487  -95.52210 30.07400       Denny's                 <NA>        NA
    ## 1488  -95.53000 30.01760       Denny's                 <NA>        NA
    ## 1489  -95.43330 30.05780       Denny's                 <NA>        NA
    ## 1490  -95.63240 29.60190       Denny's                 <NA>        NA
    ## 1491  -96.31700 32.72300       Denny's                 <NA>        NA
    ## 1492  -94.95160 29.39430       Denny's                 <NA>        NA
    ## 1493  -95.63270 30.08940       Denny's                 <NA>        NA
    ## 1494  -99.87320 32.46020       Denny's                 <NA>        NA
    ## 1495  -95.33160 32.37920       Denny's                 <NA>        NA
    ## 1496  -96.99720 28.86530       Denny's                 <NA>        NA
    ## 1497  -97.12500 31.54800       Denny's                 <NA>        NA
    ## 1498  -97.14560 31.50910       Denny's                 <NA>        NA
    ## 1499  -97.98240 26.15630       Denny's                 <NA>        NA
    ## 1500  -96.12070 29.32810       Denny's                 <NA>        NA
    ## 1501  -98.46730 33.88620       Denny's                 <NA>        NA
    ## 1502  -98.53590 33.86590       Denny's                 <NA>        NA
    ## 1503  -96.68620 32.60480       Denny's                 <NA>        NA
    ## 1504  -94.39230 29.82700       Denny's                 <NA>        NA
    ## 1505  -95.45250 30.18020       Denny's                 <NA>        NA
    ## 1506 -113.07730 37.68180       Denny's                 <NA>        NA
    ## 1507 -112.26790 40.68550       Denny's                 <NA>        NA
    ## 1508 -111.97450 41.07510       Denny's                 <NA>        NA
    ## 1509 -111.82690 40.38900       Denny's                 <NA>        NA
    ## 1510 -111.83450 41.75090       Denny's                 <NA>        NA
    ## 1511 -111.90130 40.62070       Denny's                 <NA>        NA
    ## 1512 -111.85560 40.62320       Denny's                 <NA>        NA
    ## 1513 -109.56080 38.58870       Denny's                 <NA>        NA
    ## 1514 -111.90340 40.67450       Denny's                 <NA>        NA
    ## 1515 -111.83710 39.68720       Denny's                 <NA>        NA
    ## 1516 -111.97000 41.24350       Denny's                 <NA>        NA
    ## 1517 -111.69890 40.30610       Denny's                 <NA>        NA
    ## 1518 -111.76380 40.04110       Denny's                 <NA>        NA
    ## 1519 -111.93970 41.15770       Denny's                 <NA>        NA
    ## 1520 -111.85460 38.93530       Denny's                 <NA>        NA
    ## 1521 -111.89840 40.75850       Denny's                 <NA>        NA
    ## 1522 -111.91690 40.72690       Denny's                 <NA>        NA
    ## 1523 -111.94030 40.77110       Denny's                 <NA>        NA
    ## 1524 -111.98630 40.54100       Denny's                 <NA>        NA
    ## 1525 -111.64470 40.12560       Denny's                 <NA>        NA
    ## 1526 -111.64230 40.18560       Denny's                 <NA>        NA
    ## 1527 -113.58360 37.08760       Denny's                 <NA>        NA
    ## 1528 -113.56210 37.11020       Denny's                 <NA>        NA
    ## 1529 -112.29820 40.54880       Denny's                 <NA>        NA
    ## 1530 -112.20100 41.71090       Denny's                 <NA>        NA
    ## 1531 -109.49940 40.42490       Denny's                 <NA>        NA
    ## 1532 -112.00730 41.22830       Denny's                 <NA>        NA
    ## 1533  -77.08420 38.76090       Denny's                 <NA>        NA
    ## 1534  -76.25360 36.76520       Denny's                 <NA>        NA
    ## 1535  -77.40820 37.35540       Denny's                 <NA>        NA
    ## 1536  -78.08790 39.29140       Denny's                 <NA>        NA
    ## 1537  -77.39290 37.24460       Denny's                 <NA>        NA
    ## 1538  -77.30010 38.62710       Denny's                 <NA>        NA
    ## 1539  -77.45060 37.84260       Denny's                 <NA>        NA
    ## 1540  -77.30650 38.85850       Denny's                 <NA>        NA
    ## 1541  -77.49900 38.24580       Denny's                 <NA>        NA
    ## 1542  -80.94680 36.94520       Denny's                 <NA>        NA
    ## 1543  -76.38190 37.04320       Denny's                 <NA>        NA
    ## 1544  -77.32430 37.26230       Denny's                 <NA>        NA
    ## 1545  -77.50630 38.77750       Denny's                 <NA>        NA
    ## 1546  -78.63040 38.76050       Denny's                 <NA>        NA
    ## 1547  -77.51460 37.60040       Denny's                 <NA>        NA
    ## 1548  -77.60070 37.50510       Denny's                 <NA>        NA
    ## 1549  -79.89250 37.30260       Denny's                 <NA>        NA
    ## 1550  -77.47440 37.93410       Denny's                 <NA>        NA
    ## 1551  -80.09310 37.28790       Denny's                 <NA>        NA
    ## 1552  -77.39430 36.97780       Denny's                 <NA>        NA
    ## 1553  -78.33660 39.00340       Denny's                 <NA>        NA
    ## 1554  -76.08830 36.84170       Denny's                 <NA>        NA
    ## 1555  -76.18510 36.84380       Denny's                 <NA>        NA
    ## 1556  -77.77330 38.73220       Denny's                 <NA>        NA
    ## 1557  -76.71120 37.28460       Denny's                 <NA>        NA
    ## 1558  -78.14090 39.21620       Denny's                 <NA>        NA
    ## 1559  -77.25550 38.65280       Denny's                 <NA>        NA
    ## 1560  -80.99310 36.93650       Denny's                 <NA>        NA
    ## 1561  -72.96780 43.58400       Denny's                 <NA>        NA
    ## 1562  -73.20950 44.44580       Denny's                 <NA>        NA
    ## 1563 -123.82080 46.97150       Denny's                 <NA>        NA
    ## 1564 -122.19870 48.18800       Denny's                 <NA>        NA
    ## 1565 -122.22560 47.30290       Denny's                 <NA>        NA
    ## 1566 -122.14310 47.63020       Denny's                 <NA>        NA
    ## 1567 -122.48540 48.78440       Denny's                 <NA>        NA
    ## 1568 -122.25130 47.20060       Denny's                 <NA>        NA
    ## 1569 -122.21710 47.79050       Denny's                 <NA>        NA
    ## 1570 -122.68240 47.56960       Denny's                 <NA>        NA
    ## 1571 -122.97910 46.72810       Denny's                 <NA>        NA
    ## 1572 -122.95670 46.64730       Denny's                 <NA>        NA
    ## 1573 -122.34660 47.77780       Denny's                 <NA>        NA
    ## 1574 -122.19250 47.97670       Denny's                 <NA>        NA
    ## 1575 -122.23570 47.88190       Denny's                 <NA>        NA
    ## 1576 -122.22850 47.92140       Denny's                 <NA>        NA
    ## 1577 -122.30600 47.31510       Denny's                 <NA>        NA
    ## 1578 -122.31350 47.29030       Denny's                 <NA>        NA
    ## 1579 -122.57350 48.84870       Denny's                 <NA>        NA
    ## 1580 -122.36050 47.24310       Denny's                 <NA>        NA
    ## 1581 -122.89580 46.14510       Denny's                 <NA>        NA
    ## 1582 -119.15900 46.20950       Denny's                 <NA>        NA
    ## 1583 -122.22810 47.39510       Denny's                 <NA>        NA
    ## 1584 -122.17980 47.70930       Denny's                 <NA>        NA
    ## 1585 -122.82320 47.04850       Denny's                 <NA>        NA
    ## 1586 -122.28850 47.82100       Denny's                 <NA>        NA
    ## 1587 -121.97730 47.86090       Denny's                 <NA>        NA
    ## 1588 -119.25210 47.10610       Denny's                 <NA>        NA
    ## 1589 -122.33920 48.43590       Denny's                 <NA>        NA
    ## 1590 -122.30120 47.16190       Denny's                 <NA>        NA
    ## 1591 -122.29340 47.10560       Denny's                 <NA>        NA
    ## 1592 -122.19620 47.53370       Denny's                 <NA>        NA
    ## 1593 -119.27430 46.28650       Denny's                 <NA>        NA
    ## 1594 -122.29630 47.44850       Denny's                 <NA>        NA
    ## 1595 -122.29570 47.43610       Denny's                 <NA>        NA
    ## 1596 -122.32910 47.57910       Denny's                 <NA>        NA
    ## 1597 -122.33390 47.47010       Denny's                 <NA>        NA
    ## 1598 -123.12580 47.23120       Denny's                 <NA>        NA
    ## 1599 -122.41950 47.07200       Denny's                 <NA>        NA
    ## 1600 -117.23940 47.65750       Denny's                 <NA>        NA
    ## 1601 -117.41130 47.69030       Denny's                 <NA>        NA
    ## 1602 -117.28280 47.67580       Denny's                 <NA>        NA
    ## 1603 -117.50720 47.62120       Denny's                 <NA>        NA
    ## 1604 -122.51620 47.25540       Denny's                 <NA>        NA
    ## 1605 -122.43420 47.15930       Denny's                 <NA>        NA
    ## 1606 -122.46320 47.17910       Denny's                 <NA>        NA
    ## 1607 -122.50270 47.15030       Denny's                 <NA>        NA
    ## 1608 -122.51940 47.16670       Denny's                 <NA>        NA
    ## 1609 -120.47520 46.56300       Denny's                 <NA>        NA
    ## 1610 -120.32520 47.44140       Denny's                 <NA>        NA
    ## 1611 -120.56090 46.61460       Denny's                 <NA>        NA
    ## 1612  -88.45810 44.26210       Denny's                 <NA>        NA
    ## 1613  -90.85680 44.29460       Denny's                 <NA>        NA
    ## 1614  -88.07640 44.47070       Denny's                 <NA>        NA
    ## 1615  -88.04800 42.95930       Denny's                 <NA>        NA
    ## 1616  -92.72340 44.95960       Denny's                 <NA>        NA
    ## 1617  -89.00500 42.70690       Denny's                 <NA>        NA
    ## 1618  -89.79320 43.60170       Denny's                 <NA>        NA
    ## 1619  -89.31430 43.12360       Denny's                 <NA>        NA
    ## 1620  -89.50270 43.05860       Denny's                 <NA>        NA
    ## 1621  -90.06120 43.79670       Denny's                 <NA>        NA
    ## 1622  -91.93220 44.90320       Denny's                 <NA>        NA
    ## 1623  -87.90980 42.95500       Denny's                 <NA>        NA
    ## 1624  -87.94850 42.97570       Denny's                 <NA>        NA
    ## 1625  -88.01000 43.09000       Denny's                 <NA>        NA
    ## 1626  -88.00890 43.17780       Denny's                 <NA>        NA
    ## 1627  -89.31200 43.04820       Denny's                 <NA>        NA
    ## 1628  -87.94230 42.86920       Denny's                 <NA>        NA
    ## 1629  -88.25590 43.04250       Denny's                 <NA>        NA
    ## 1630  -87.84630 42.71880       Denny's                 <NA>        NA
    ## 1631  -89.61600 44.85830       Denny's                 <NA>        NA
    ## 1632  -90.50160 44.01810       Denny's                 <NA>        NA
    ## 1633  -88.26470 42.99130       Denny's                 <NA>        NA
    ## 1634  -88.05230 43.05970       Denny's                 <NA>        NA
    ## 1635  -92.19550 44.93500       Denny's                 <NA>        NA
    ## 1636  -89.79360 43.62320       Denny's                 <NA>        NA
    ## 1637  -80.27780 39.28040       Denny's                 <NA>        NA
    ## 1638  -78.97700 39.43820       Denny's                 <NA>        NA
    ## 1639  -79.95840 39.58180       Denny's                 <NA>        NA
    ## 1640 -106.27280 42.85180       Denny's                 <NA>        NA
    ## 1641 -104.85290 41.09900       Denny's                 <NA>        NA
    ## 1642 -107.23400 41.78590       Denny's                 <NA>        NA
    ## 1643 -109.24000 41.60520       Denny's                 <NA>        NA
    ## 1644  -76.18846 39.52322     La Quinta             Maryland  12405.93
    ## 1645  -99.77877 32.41349     La Quinta                Texas 268596.46
    ## 1646  -99.72269 32.49136     La Quinta                Texas 268596.46
    ## 1647  -84.65609 34.08204     La Quinta              Georgia  59425.15
    ## 1648  -96.63652 34.78180     La Quinta             Oklahoma  69898.87
    ## 1649  -96.82874 32.95164     La Quinta                Texas 268596.46
    ## 1650  -98.11925 26.18997     La Quinta                Texas 268596.46
    ## 1651 -106.62173 35.05815     La Quinta           New Mexico 121590.30
    ## 1652 -106.58876 35.16086     La Quinta           New Mexico 121590.30
    ## 1653 -106.62192 35.11068     La Quinta           New Mexico 121590.30
    ## 1654 -106.58523 35.16026     La Quinta           New Mexico 121590.30
    ## 1655 -106.70836 35.10354     La Quinta           New Mexico 121590.30
    ## 1656  -92.52164 31.34590     La Quinta            Louisiana  52378.13
    ## 1657  -98.04351 27.76425     La Quinta                Texas 268596.46
    ## 1658  -96.66739 33.11674     La Quinta                Texas 268596.46
    ## 1659  -97.22377 32.41490     La Quinta                Texas 268596.46
    ## 1660  -95.23011 29.41755     La Quinta                Texas 268596.46
    ## 1661 -101.81877 35.19200     La Quinta                Texas 268596.46
    ## 1662 -101.73149 35.19372     La Quinta                Texas 268596.46
    ## 1663 -101.92195 35.18912     La Quinta                Texas 268596.46
    ## 1664 -117.90993 33.80434     La Quinta           California 163694.74
    ## 1665 -149.91190 61.18843     La Quinta               Alaska 665384.04
    ## 1666  -71.20883 42.69072     La Quinta        Massachusetts  10554.39
    ## 1667 -102.54860 32.32933     La Quinta                Texas 268596.46
    ## 1668  -95.45799 29.16294     La Quinta                Texas 268596.46
    ## 1669  -88.46233 44.26234     La Quinta            Wisconsin  65496.38
    ## 1670  -97.16337 34.18978     La Quinta             Oklahoma  69898.87
    ## 1671  -97.06566 32.75500     La Quinta                Texas 268596.46
    ## 1672  -73.70732 41.12099     La Quinta             New York  54554.98
    ## 1673 -104.42378 32.84168     La Quinta           New Mexico 121590.30
    ## 1674  -95.17355 29.99611     La Quinta                Texas 268596.46
    ## 1675  -84.42862 33.65816     La Quinta              Georgia  59425.15
    ## 1676  -84.45413 33.62373     La Quinta              Georgia  59425.15
    ## 1677  -84.28921 34.05078     La Quinta              Georgia  59425.15
    ## 1678  -84.00576 33.65490     La Quinta              Georgia  59425.15
    ## 1679  -84.75943 33.73149     La Quinta              Georgia  59425.15
    ## 1680  -84.09239 33.97508     La Quinta              Georgia  59425.15
    ## 1681  -84.34943 33.82362     La Quinta              Georgia  59425.15
    ## 1682  -84.48330 33.86299     La Quinta              Georgia  59425.15
    ## 1683  -84.35148 33.92302     La Quinta              Georgia  59425.15
    ## 1684  -84.32882 34.02439     La Quinta              Georgia  59425.15
    ## 1685  -84.75812 33.39712     La Quinta              Georgia  59425.15
    ## 1686  -84.27435 33.55319     La Quinta              Georgia  59425.15
    ## 1687  -71.84153 42.19943     La Quinta        Massachusetts  10554.39
    ## 1688 -122.22708 47.30213     La Quinta           Washington  71297.95
    ## 1689  -82.04630 33.51228     La Quinta              Georgia  59425.15
    ## 1690  -97.69182 30.21756     La Quinta                Texas 268596.46
    ## 1691  -97.73850 30.27219     La Quinta                Texas 268596.46
    ## 1692  -97.79215 30.47572     La Quinta                Texas 268596.46
    ## 1693  -97.81785 30.52437     La Quinta                Texas 268596.46
    ## 1694  -97.71867 30.40689     La Quinta                Texas 268596.46
    ## 1695  -97.70203 30.33911     La Quinta                Texas 268596.46
    ## 1696  -97.74033 30.23346     La Quinta                Texas 268596.46
    ## 1697  -97.69190 30.52977     La Quinta                Texas 268596.46
    ## 1698  -97.75328 30.21475     La Quinta                Texas 268596.46
    ## 1699  -97.81745 30.24088     La Quinta                Texas 268596.46
    ## 1700  -97.70783 30.31979     La Quinta                Texas 268596.46
    ## 1701 -119.07630 35.44045     La Quinta           California 163694.74
    ## 1702 -119.04238 35.38327     La Quinta           California 163694.74
    ## 1703  -76.68506 39.19723     La Quinta             Maryland  12405.93
    ## 1704  -76.61832 39.29308     La Quinta             Maryland  12405.93
    ## 1705  -76.49068 39.33826     La Quinta             Maryland  12405.93
    ## 1706  -76.61359 39.20314     La Quinta             Maryland  12405.93
    ## 1707  -78.19307 43.01731     La Quinta             New York  54554.98
    ## 1708  -91.00763 30.44642     La Quinta            Louisiana  52378.13
    ## 1709  -91.23969 30.44736     La Quinta            Louisiana  52378.13
    ## 1710  -91.06372 30.38241     La Quinta            Louisiana  52378.13
    ## 1711  -91.15053 30.42595     La Quinta            Louisiana  52378.13
    ## 1712  -95.92845 28.98912     La Quinta                Texas 268596.46
    ## 1713  -94.16126 30.03882     La Quinta                Texas 268596.46
    ## 1714  -97.72453 28.41195     La Quinta                Texas 268596.46
    ## 1715 -111.18737 45.76426     La Quinta              Montana 147039.71
    ## 1716 -122.50977 48.78928     La Quinta           Washington  71297.95
    ## 1717  -97.47686 31.03028     La Quinta                Texas 268596.46
    ## 1718 -121.31563 44.02176     La Quinta               Oregon  98378.54
    ## 1719  -94.19938 36.33578     La Quinta             Arkansas  53178.55
    ## 1720 -122.29560 37.86838     La Quinta           California 163694.74
    ## 1721 -101.49633 32.26347     La Quinta                Texas 268596.46
    ## 1722 -108.56049 45.74714     La Quinta              Montana 147039.71
    ## 1723  -88.93566 30.44575     La Quinta          Mississippi  48431.78
    ## 1724  -75.97092 42.12472     La Quinta             New York  54554.98
    ## 1725  -86.70832 33.43099     La Quinta              Alabama  52420.07
    ## 1726  -86.81964 33.45524     La Quinta              Alabama  52420.07
    ## 1727  -86.78366 33.35318     La Quinta              Alabama  52420.07
    ## 1728 -100.77363 46.82965     La Quinta         North Dakota  70698.32
    ## 1729  -94.30408 39.03519     La Quinta             Missouri  69706.99
    ## 1730 -116.21435 43.57857     La Quinta                Idaho  83568.95
    ## 1731 -116.28113 43.61072     La Quinta                Idaho  83568.95
    ## 1732  -81.80676 26.32020     La Quinta              Florida  65757.70
    ## 1733  -81.66881 36.21550     La Quinta       North Carolina  53819.16
    ## 1734  -93.71291 32.51818     La Quinta            Louisiana  52378.13
    ## 1735  -71.08477 42.39449     La Quinta        Massachusetts  10554.39
    ## 1736  -90.41240 29.88989     La Quinta            Louisiana  52378.13
    ## 1737  -86.41825 36.93056     La Quinta             Kentucky  40407.80
    ## 1738 -111.04560 45.69864     La Quinta              Montana 147039.71
    ## 1739  -90.03557 32.27611     La Quinta          Mississippi  48431.78
    ## 1740  -96.39552 30.13730     La Quinta                Texas 268596.46
    ## 1741  -93.84933 30.01707     La Quinta                Texas 268596.46
    ## 1742  -97.75693 33.22161     La Quinta                Texas 268596.46
    ## 1743  -95.79563 36.07495     La Quinta             Oklahoma  69898.87
    ## 1744  -73.95026 40.67881     La Quinta             New York  54554.98
    ## 1745  -73.99303 40.66904     La Quinta             New York  54554.98
    ## 1746  -73.92154 40.66831     La Quinta             New York  54554.98
    ## 1747  -95.96684 29.77745     La Quinta                Texas 268596.46
    ## 1748  -91.94891 30.14327     La Quinta            Louisiana  52378.13
    ## 1749  -97.51905 25.97977     La Quinta                Texas 268596.46
    ## 1750  -98.97898 31.73006     La Quinta                Texas 268596.46
    ## 1751  -81.52339 31.21718     La Quinta              Georgia  59425.15
    ## 1752 -118.03140 33.86041     La Quinta           California 163694.74
    ## 1753  -78.69662 42.94790     La Quinta             New York  54554.98
    ## 1754  -97.31620 32.56387     La Quinta                Texas 268596.46
    ## 1755 -112.50260 45.98376     La Quinta              Montana 147039.71
    ## 1756 -116.66160 43.66190     La Quinta                Idaho  83568.95
    ## 1757  -84.91540 34.47076     La Quinta              Georgia  59425.15
    ## 1758  -90.06816 32.60180     La Quinta          Mississippi  48431.78
    ## 1759  -81.41782 40.85781     La Quinta                 Ohio  44825.58
    ## 1760  -76.85351 38.88879     La Quinta             Maryland  12405.93
    ## 1761 -104.23130 32.37293     La Quinta           New Mexico 121590.30
    ## 1762 -106.33081 42.85745     La Quinta              Wyoming  97813.01
    ## 1763 -104.86556 39.37910     La Quinta             Colorado 104093.67
    ## 1764 -113.07884 37.65395     La Quinta                 Utah  84896.88
    ## 1765  -96.91822 32.61657     La Quinta                Texas 268596.46
    ## 1766  -91.65363 42.03083     La Quinta                 Iowa  56272.81
    ## 1767 -122.90102 42.37711     La Quinta               Oregon  98378.54
    ## 1768  -77.63338 39.93258     La Quinta         Pennsylvania  46054.35
    ## 1769  -88.24470 40.13851     La Quinta             Illinois  57913.55
    ## 1770  -79.96252 32.77668     La Quinta       South Carolina  32020.49
    ## 1771  -80.92088 35.24087     La Quinta       North Carolina  53819.16
    ## 1772  -80.89117 35.17895     La Quinta       North Carolina  53819.16
    ## 1773  -78.49427 38.06057     La Quinta             Virginia  42774.93
    ## 1774  -85.20064 34.98967     La Quinta            Tennessee  42144.25
    ## 1775  -85.15811 35.04450     La Quinta            Tennessee  42144.25
    ## 1776  -85.36267 35.03011     La Quinta            Tennessee  42144.25
    ## 1777  -85.24086 35.12028     La Quinta            Tennessee  42144.25
    ## 1778  -85.15515 35.04622     La Quinta            Tennessee  42144.25
    ## 1779 -104.84758 41.12020     La Quinta              Wyoming  97813.01
    ## 1780  -87.63531 41.88155     La Quinta             Illinois  57913.55
    ## 1781  -87.94416 42.37796     La Quinta             Illinois  57913.55
    ## 1782  -87.58584 41.80615     La Quinta             Illinois  57913.55
    ## 1783  -87.88459 42.19666     La Quinta             Illinois  57913.55
    ## 1784  -87.95845 42.02254     La Quinta             Illinois  57913.55
    ## 1785  -87.97357 41.84778     La Quinta             Illinois  57913.55
    ## 1786  -87.79418 41.55743     La Quinta             Illinois  57913.55
    ## 1787  -87.94314 41.74503     La Quinta             Illinois  57913.55
    ## 1788  -84.63565 39.01189     La Quinta             Kentucky  40407.80
    ## 1789  -84.48477 39.30029     La Quinta                 Ohio  44825.58
    ## 1790  -84.31723 39.29413     La Quinta                 Ohio  44825.58
    ## 1791  -84.43892 39.27202     La Quinta                 Ohio  44825.58
    ## 1792  -95.62307 36.30292     La Quinta             Oklahoma  69898.87
    ## 1793  -87.28069 36.59563     La Quinta            Tennessee  42144.25
    ## 1794  -95.13385 29.54522     La Quinta                Texas 268596.46
    ## 1795  -82.67852 27.89475     La Quinta              Florida  65757.70
    ## 1796  -82.73120 27.96633     La Quinta              Florida  65757.70
    ## 1797  -82.70237 27.86673     La Quinta              Florida  65757.70
    ## 1798  -97.39494 32.37016     La Quinta                Texas 268596.46
    ## 1799  -81.80438 41.44004     La Quinta                 Ohio  44825.58
    ## 1800  -81.90516 41.41401     La Quinta                 Ohio  44825.58
    ## 1801  -81.64920 41.39412     La Quinta                 Ohio  44825.58
    ## 1802  -81.51709 41.29903     La Quinta                 Ohio  44825.58
    ## 1803  -84.89023 35.19401     La Quinta            Tennessee  42144.25
    ## 1804  -95.09886 30.33169     La Quinta                Texas 268596.46
    ## 1805  -73.77206 42.87093     La Quinta             New York  54554.98
    ## 1806  -74.13415 40.82740     La Quinta           New Jersey   8722.58
    ## 1807  -98.99274 35.49423     La Quinta             Oklahoma  69898.87
    ## 1808 -103.19630 34.44398     La Quinta           New Mexico 121590.30
    ## 1809  -95.41217 29.01035     La Quinta                Texas 268596.46
    ## 1810  -80.60306 28.36870     La Quinta              Florida  65757.70
    ## 1811  -80.61078 28.33359     La Quinta              Florida  65757.70
    ## 1812 -116.79095 47.69687     La Quinta                Idaho  83568.95
    ## 1813  -96.28350 30.57947     La Quinta                Texas 268596.46
    ## 1814  -96.33333 30.62742     La Quinta                Texas 268596.46
    ## 1815  -90.01697 38.68286     La Quinta             Illinois  57913.55
    ## 1816 -100.84643 32.40995     La Quinta                Texas 268596.46
    ## 1817 -104.82965 38.89565     La Quinta             Colorado 104093.67
    ## 1818 -104.80397 38.95054     La Quinta             Colorado 104093.67
    ## 1819 -104.79802 38.79224     La Quinta             Colorado 104093.67
    ## 1820  -76.78875 39.17124     La Quinta             Maryland  12405.93
    ## 1821  -80.94679 34.07607     La Quinta       South Carolina  32020.49
    ## 1822  -80.95014 33.96997     La Quinta       South Carolina  32020.49
    ## 1823  -92.37588 38.96832     La Quinta             Missouri  69706.99
    ## 1824  -82.83178 39.93629     La Quinta                 Ohio  44825.58
    ## 1825  -83.13317 40.07737     La Quinta                 Ohio  44825.58
    ## 1826  -85.96094 39.19897     La Quinta              Indiana  36419.55
    ## 1827  -84.97124 32.53607     La Quinta              Georgia  59425.15
    ## 1828  -84.94134 32.48309     La Quinta              Georgia  59425.15
    ## 1829  -84.94823 32.50522     La Quinta              Georgia  59425.15
    ## 1830  -83.14632 39.98061     La Quinta                 Ohio  44825.58
    ## 1831  -96.53870 29.69031     La Quinta                Texas 268596.46
    ## 1832  -95.48315 30.36748     La Quinta                Texas 268596.46
    ## 1833  -92.43712 35.11158     La Quinta             Arkansas  53178.55
    ## 1834  -85.50473 36.13312     La Quinta            Tennessee  42144.25
    ## 1835  -80.24980 26.27442     La Quinta              Florida  65757.70
    ## 1836  -97.46010 27.75674     La Quinta                Texas 268596.46
    ## 1837  -97.22123 27.62097     La Quinta                Texas 268596.46
    ## 1838  -97.45181 27.80309     La Quinta                Texas 268596.46
    ## 1839  -97.57162 27.84770     La Quinta                Texas 268596.46
    ## 1840  -97.31053 27.89840     La Quinta                Texas 268596.46
    ## 1841  -97.35886 27.70164     La Quinta                Texas 268596.46
    ## 1842  -96.44031 32.10019     La Quinta                Texas 268596.46
    ## 1843  -99.24490 28.44464     La Quinta                Texas 268596.46
    ## 1844  -71.54999 41.66289     La Quinta         Rhode Island   1544.89
    ## 1845  -83.88375 33.60847     La Quinta              Georgia  59425.15
    ## 1846  -86.87375 34.20943     La Quinta              Alabama  52420.07
    ## 1847  -95.72008 29.98183     La Quinta                Texas 268596.46
    ## 1848 -102.51441 36.06935     La Quinta                Texas 268596.46
    ## 1849  -97.12301 32.67881     La Quinta                Texas 268596.46
    ## 1850  -96.76327 32.92832     La Quinta                Texas 268596.46
    ## 1851  -97.01573 32.91777     La Quinta                Texas 268596.46
    ## 1852  -96.80707 32.77724     La Quinta                Texas 268596.46
    ## 1853  -97.00628 32.76130     La Quinta                Texas 268596.46
    ## 1854  -97.03209 32.67532     La Quinta                Texas 268596.46
    ## 1855  -96.70550 32.64927     La Quinta                Texas 268596.46
    ## 1856  -96.89668 32.88221     La Quinta                Texas 268596.46
    ## 1857  -96.95981 32.86566     La Quinta                Texas 268596.46
    ## 1858  -96.87689 32.82271     La Quinta                Texas 268596.46
    ## 1859  -96.59466 32.78878     La Quinta                Texas 268596.46
    ## 1860  -96.77066 32.88326     La Quinta                Texas 268596.46
    ## 1861  -96.64789 32.85924     La Quinta                Texas 268596.46
    ## 1862  -96.79235 33.01572     La Quinta                Texas 268596.46
    ## 1863  -96.78654 32.81982     La Quinta                Texas 268596.46
    ## 1864  -85.00281 34.76867     La Quinta              Georgia  59425.15
    ## 1865  -73.40701 41.41597     La Quinta          Connecticut   5543.41
    ## 1866  -90.52328 41.55219     La Quinta                 Iowa  56272.81
    ## 1867 -121.73118 38.54085     La Quinta           California 163694.74
    ## 1868  -84.19180 39.96259     La Quinta                 Ohio  44825.58
    ## 1869  -81.08200 29.17333     La Quinta              Florida  65757.70
    ## 1870  -87.00212 34.56093     La Quinta              Alabama  52420.07
    ## 1871  -97.58883 33.22082     La Quinta                Texas 268596.46
    ## 1872  -95.09955 29.69904     La Quinta                Texas 268596.46
    ## 1873  -80.10955 26.31962     La Quinta              Florida  65757.70
    ## 1874  -80.11924 26.31657     La Quinta              Florida  65757.70
    ## 1875 -100.90544 29.38395     La Quinta                Texas 268596.46
    ## 1876 -107.71328 32.27160     La Quinta           New Mexico 121590.30
    ## 1877  -96.58695 33.76482     La Quinta                Texas 268596.46
    ## 1878  -97.17662 33.23645     La Quinta                Texas 268596.46
    ## 1879 -104.77217 39.82016     La Quinta             Colorado 104093.67
    ## 1880 -104.82663 39.69831     La Quinta             Colorado 104093.67
    ## 1881 -105.16211 39.95895     La Quinta             Colorado 104093.67
    ## 1882 -104.99424 39.76627     La Quinta             Colorado 104093.67
    ## 1883 -104.94098 39.68035     La Quinta             Colorado 104093.67
    ## 1884 -104.88446 39.59604     La Quinta             Colorado 104093.67
    ## 1885 -104.84589 39.77691     La Quinta             Colorado 104093.67
    ## 1886 -105.14480 39.76366     La Quinta             Colorado 104093.67
    ## 1887 -104.99134 39.91468     La Quinta             Colorado 104093.67
    ## 1888 -105.07624 39.65237     La Quinta             Colorado 104093.67
    ## 1889 -104.87880 39.58830     La Quinta             Colorado 104093.67
    ## 1890 -105.05183 39.85298     La Quinta             Colorado 104093.67
    ## 1891  -93.77979 41.60250     La Quinta                 Iowa  56272.81
    ## 1892  -96.82471 32.60109     La Quinta                Texas 268596.46
    ## 1893  -83.44699 42.32159     La Quinta             Michigan  96713.51
    ## 1894  -83.34176 42.24316     La Quinta             Michigan  96713.51
    ## 1895  -83.22628 42.21339     La Quinta             Michigan  96713.51
    ## 1896  -83.01027 42.62826     La Quinta             Michigan  96713.51
    ## 1897  -97.00747 32.83768     La Quinta                Texas 268596.46
    ## 1898  -97.12239 32.83953     La Quinta                Texas 268596.46
    ## 1899  -97.08912 32.83621     La Quinta                Texas 268596.46
    ## 1900 -102.79322 46.89496     La Quinta         North Dakota  70698.32
    ## 1901 -100.04733 37.75453     La Quinta               Kansas  82278.36
    ## 1902  -85.42636 31.24868     La Quinta              Alabama  52420.07
    ## 1903 -121.90789 37.70620     La Quinta           California 163694.74
    ## 1904  -82.93876 32.48694     La Quinta              Georgia  59425.15
    ## 1905  -92.16439 46.81014     La Quinta            Minnesota  86935.83
    ## 1906 -101.97522 35.84222     La Quinta                Texas 268596.46
    ## 1907 -107.86908 37.23990     La Quinta             Colorado 104093.67
    ## 1908  -96.40572 34.00163     La Quinta             Oklahoma  69898.87
    ## 1909  -78.97218 35.96460     La Quinta       North Carolina  53819.16
    ## 1910  -78.89145 35.90796     La Quinta       North Carolina  53819.16
    ## 1911 -100.47501 28.70999     La Quinta                Texas 268596.46
    ## 1912  -98.79170 32.40050     La Quinta                Texas 268596.46
    ## 1913  -76.30978 39.45374     La Quinta             Maryland  12405.93
    ## 1914  -97.42830 35.65176     La Quinta             Oklahoma  69898.87
    ## 1915  -88.56021 39.13901     La Quinta             Illinois  57913.55
    ## 1916  -92.64030 33.19725     La Quinta             Arkansas  53178.55
    ## 1917 -106.40824 31.78098     La Quinta                Texas 268596.46
    ## 1918 -106.56748 31.83897     La Quinta                Texas 268596.46
    ## 1919 -106.36636 31.76620     La Quinta                Texas 268596.46
    ## 1920 -106.34691 31.75199     La Quinta                Texas 268596.46
    ## 1921 -106.33335 31.74587     La Quinta                Texas 268596.46
    ## 1922 -106.56759 31.83763     La Quinta                Texas 268596.46
    ## 1923  -85.83286 37.70916     La Quinta             Kentucky  40407.80
    ## 1924  -99.37393 35.42283     La Quinta             Oklahoma  69898.87
    ## 1925  -81.50049 38.45691     La Quinta        West Virginia  24230.04
    ## 1926  -73.81739 41.07352     La Quinta             New York  54554.98
    ## 1927 -114.87072 39.24660     La Quinta               Nevada 110571.82
    ## 1928  -96.21897 38.42179     La Quinta               Kansas  82278.36
    ## 1929  -97.93847 36.39141     La Quinta             Oklahoma  69898.87
    ## 1930  -96.61422 32.33610     La Quinta                Texas 268596.46
    ## 1931  -80.04103 42.07079     La Quinta         Pennsylvania  46054.35
    ## 1932 -123.08070 44.05829     La Quinta               Oregon  98378.54
    ## 1933  -87.45493 37.97852     La Quinta              Indiana  36419.55
    ## 1934 -122.23404 47.88329     La Quinta           Washington  71297.95
    ## 1935 -147.86600 64.82426     La Quinta               Alaska 665384.04
    ## 1936  -84.06416 39.77484     La Quinta                 Ohio  44825.58
    ## 1937 -122.12758 38.21947     La Quinta           California 163694.74
    ## 1938  -74.27330 40.89288     La Quinta           New Jersey   8722.58
    ## 1939  -96.17737 31.72643     La Quinta                Texas 268596.46
    ## 1940  -73.77519 40.59355     La Quinta             New York  54554.98
    ## 1941  -96.86397 46.84265     La Quinta         North Dakota  70698.32
    ## 1942 -108.19153 36.72993     La Quinta           New Mexico 121590.30
    ## 1943  -94.14718 36.11516     La Quinta             Arkansas  53178.55
    ## 1944  -90.40373 38.21433     La Quinta             Missouri  69706.99
    ## 1945 -111.66361 35.18038     La Quinta              Arizona 113990.30
    ## 1946  -79.80478 34.23461     La Quinta       South Carolina  32020.49
    ## 1947  -98.14532 29.13495     La Quinta                Texas 268596.46
    ## 1948  -83.94098 33.04402     La Quinta              Georgia  59425.15
    ## 1949 -105.00845 40.58090     La Quinta             Colorado 104093.67
    ## 1950  -80.20287 26.18822     La Quinta              Florida  65757.70
    ## 1951  -81.79743 26.54926     La Quinta              Florida  65757.70
    ## 1952  -81.87220 26.59000     La Quinta              Florida  65757.70
    ## 1953  -94.35904 35.35431     La Quinta             Arkansas  53178.55
    ## 1954 -102.90838 30.89849     La Quinta                Texas 268596.46
    ## 1955  -86.61365 30.40415     La Quinta              Florida  65757.70
    ## 1956  -97.17406 32.76121     La Quinta                Texas 268596.46
    ## 1957  -97.42472 32.80273     La Quinta                Texas 268596.46
    ## 1958  -97.20791 32.82124     La Quinta                Texas 268596.46
    ## 1959  -97.31149 32.82629     La Quinta                Texas 268596.46
    ## 1960  -97.41635 32.67839     La Quinta                Texas 268596.46
    ## 1961  -97.44296 32.73452     La Quinta                Texas 268596.46
    ## 1962 -119.68395 36.62792     La Quinta           California 163694.74
    ## 1963  -86.54533 40.27781     La Quinta              Indiana  36419.55
    ## 1964  -98.84919 30.25400     La Quinta                Texas 268596.46
    ## 1965 -121.94488 37.48886     La Quinta           California 163694.74
    ## 1966 -119.88057 36.80995     La Quinta           California 163694.74
    ## 1967 -119.78333 36.83947     La Quinta           California 163694.74
    ## 1968 -119.78008 36.74046     La Quinta           California 163694.74
    ## 1969 -108.73949 39.15328     La Quinta             Colorado 104093.67
    ## 1970  -80.16038 26.03639     La Quinta              Florida  65757.70
    ## 1971  -80.15735 26.20344     La Quinta              Florida  65757.70
    ## 1972  -80.11096 26.19822     La Quinta              Florida  65757.70
    ## 1973  -80.25560 26.10621     La Quinta              Florida  65757.70
    ## 1974  -80.20003 26.18618     La Quinta              Florida  65757.70
    ## 1975  -81.96874 26.49330     La Quinta              Florida  65757.70
    ## 1976  -80.39653 27.41744     La Quinta              Florida  65757.70
    ## 1977  -85.10554 41.17882     La Quinta              Indiana  36419.55
    ## 1978  -97.27201 32.66000     La Quinta                Texas 268596.46
    ## 1979  -86.80062 33.61103     La Quinta              Alabama  52420.07
    ## 1980  -82.41668 29.66170     La Quinta              Florida  65757.70
    ## 1981  -97.15279 33.67410     La Quinta                Texas 268596.46
    ## 1982 -108.66558 35.53137     La Quinta           New Mexico 121590.30
    ## 1983  -94.77983 29.29681     La Quinta                Texas 268596.46
    ## 1984  -73.60351 40.73391     La Quinta             New York  54554.98
    ## 1985  -96.57320 32.84975     La Quinta                Texas 268596.46
    ## 1986 -105.49831 44.27636     La Quinta              Wyoming  97813.01
    ## 1987  -97.75783 32.23834     La Quinta                Texas 268596.46
    ## 1988 -104.69671 47.12243     La Quinta              Montana 147039.71
    ## 1989  -97.46242 29.51254     La Quinta                Texas 268596.46
    ## 1990  -90.95039 30.20771     La Quinta            Louisiana  52378.13
    ## 1991  -86.70593 36.32242     La Quinta            Tennessee  42144.25
    ## 1992  -97.76596 32.43519     La Quinta                Texas 268596.46
    ## 1993  -97.08634 47.91288     La Quinta         North Dakota  70698.32
    ## 1994 -108.54110 39.11451     La Quinta             Colorado 104093.67
    ## 1995 -123.31941 42.46189     La Quinta               Oregon  98378.54
    ## 1996 -111.30703 47.49904     La Quinta              Montana 147039.71
    ## 1997  -79.89346 36.05371     La Quinta       North Carolina  53819.16
    ## 1998  -82.33801 34.85823     La Quinta       South Carolina  32020.49
    ## 1999  -86.07600 39.61559     La Quinta              Indiana  36419.55
    ## 2000  -83.04540 39.88108     La Quinta                 Ohio  44825.58
    ## 2001  -96.14153 32.33351     La Quinta                Texas 268596.46
    ## 2002  -97.39537 35.88140     La Quinta             Oklahoma  69898.87
    ## 2003  -90.45600 30.46884     La Quinta            Louisiana  52378.13
    ## 2004  -76.80467 40.23883     La Quinta         Pennsylvania  46054.35
    ## 2005  -76.71373 40.35191     La Quinta         Pennsylvania  46054.35
    ## 2006  -72.67280 41.91947     La Quinta          Connecticut   5543.41
    ## 2007  -89.34972 31.32772     La Quinta          Mississippi  48431.78
    ## 2008 -112.00326 46.59161     La Quinta              Montana 147039.71
    ## 2009 -104.91057 39.86110     La Quinta             Colorado 104093.67
    ## 2010 -117.34603 34.46891     La Quinta           California 163694.74
    ## 2011  -81.26184 35.70027     La Quinta       North Carolina  53819.16
    ## 2012  -97.09828 32.01763     La Quinta                Texas 268596.46
    ## 2013  -81.56719 31.85852     La Quinta              Georgia  59425.15
    ## 2014 -103.15750 32.73683     La Quinta           New Mexico 121590.30
    ## 2015  -93.23215 36.60644     La Quinta             Missouri  69706.99
    ## 2016  -87.47580 36.82474     La Quinta             Kentucky  40407.80
    ## 2017  -90.00227 34.96161     La Quinta          Mississippi  48431.78
    ## 2018  -93.07142 34.45217     La Quinta             Arkansas  53178.55
    ## 2019  -90.76027 29.62950     La Quinta            Louisiana  52378.13
    ## 2020  -94.97842 29.80560     La Quinta                Texas 268596.46
    ## 2021  -95.33114 29.94277     La Quinta                Texas 268596.46
    ## 2022  -95.29598 29.98447     La Quinta                Texas 268596.46
    ## 2023  -95.16264 29.80076     La Quinta                Texas 268596.46
    ## 2024  -95.55933 29.83316     La Quinta                Texas 268596.46
    ## 2025  -95.61815 29.91685     La Quinta                Texas 268596.46
    ## 2026  -95.20667 29.77307     La Quinta                Texas 268596.46
    ## 2027  -95.60083 29.73788     La Quinta                Texas 268596.46
    ## 2028  -95.45508 29.75091     La Quinta                Texas 268596.46
    ## 2029  -95.44280 29.72842     La Quinta                Texas 268596.46
    ## 2030  -95.26215 29.65399     La Quinta                Texas 268596.46
    ## 2031  -95.42332 30.02397     La Quinta                Texas 268596.46
    ## 2032  -95.02934 29.65214     La Quinta                Texas 268596.46
    ## 2033  -95.04571 29.56450     La Quinta                Texas 268596.46
    ## 2034  -95.44044 30.12399     La Quinta                Texas 268596.46
    ## 2035  -95.46556 29.81835     La Quinta                Texas 268596.46
    ## 2036  -95.55082 29.90398     La Quinta                Texas 268596.46
    ## 2037  -95.50381 29.71415     La Quinta                Texas 268596.46
    ## 2038  -95.58442 29.63470     La Quinta                Texas 268596.46
    ## 2039  -95.65119 29.78426     La Quinta                Texas 268596.46
    ## 2040  -95.55842 29.71726     La Quinta                Texas 268596.46
    ## 2041  -95.55037 29.95721     La Quinta                Texas 268596.46
    ## 2042  -86.76025 34.67557     La Quinta              Alabama  52420.07
    ## 2043  -86.65416 34.73778     La Quinta              Alabama  52420.07
    ## 2044  -95.57009 30.72013     La Quinta                Texas 268596.46
    ## 2045  -94.71660 32.45014     La Quinta                Texas 268596.46
    ## 2046 -111.98200 43.47548     La Quinta                Idaho  83568.95
    ## 2047  -86.25828 39.72708     La Quinta              Indiana  36419.55
    ## 2048  -86.25087 39.72824     La Quinta              Indiana  36419.55
    ## 2049  -86.36683 39.67357     La Quinta              Indiana  36419.55
    ## 2050  -86.15024 39.76692     La Quinta              Indiana  36419.55
    ## 2051  -86.01077 39.80193     La Quinta              Indiana  36419.55
    ## 2052  -86.22811 39.91999     La Quinta              Indiana  36419.55
    ## 2053  -86.08036 39.70199     La Quinta              Indiana  36419.55
    ## 2054 -118.34329 33.93127     La Quinta           California 163694.74
    ## 2055  -93.01565 30.25128     La Quinta            Louisiana  52378.13
    ## 2056 -117.75964 33.67594     La Quinta           California 163694.74
    ## 2057  -73.09548 40.77974     La Quinta             New York  54554.98
    ## 2058  -90.13360 32.26353     La Quinta          Mississippi  48431.78
    ## 2059  -90.14858 32.38503     La Quinta          Mississippi  48431.78
    ## 2060  -88.82824 35.66885     La Quinta            Tennessee  42144.25
    ## 2061  -81.59776 30.24819     La Quinta              Florida  65757.70
    ## 2062  -81.62811 30.18381     La Quinta              Florida  65757.70
    ## 2063  -95.25418 31.94151     La Quinta                Texas 268596.46
    ## 2064 -117.92045 33.69137     La Quinta           California 163694.74
    ## 2065  -94.47845 37.05306     La Quinta             Missouri  69706.99
    ## 2066  -98.53254 28.92565     La Quinta                Texas 268596.46
    ## 2067  -80.08641 26.93488     La Quinta              Florida  65757.70
    ## 2068 -114.29283 48.20970     La Quinta              Montana 147039.71
    ## 2069  -94.65998 39.24418     La Quinta             Missouri  69706.99
    ## 2070  -94.73035 38.95820     La Quinta               Kansas  82278.36
    ## 2071  -94.55883 39.14703     La Quinta             Missouri  69706.99
    ## 2072  -97.89281 28.87089     La Quinta                Texas 268596.46
    ## 2073  -95.76167 29.78492     La Quinta                Texas 268596.46
    ## 2074  -99.08868 40.67216     La Quinta             Nebraska  77347.81
    ## 2075  -84.56825 34.00553     La Quinta              Georgia  59425.15
    ## 2076 -119.17648 46.18602     La Quinta           Washington  71297.95
    ## 2077  -99.12034 30.06551     La Quinta                Texas 268596.46
    ## 2078  -97.74687 31.10988     La Quinta                Texas 268596.46
    ## 2079 -114.03890 35.22337     La Quinta              Arizona 113990.30
    ## 2080  -81.66320 30.78972     La Quinta              Georgia  59425.15
    ## 2081  -82.43118 36.50522     La Quinta            Tennessee  42144.25
    ## 2082  -97.84466 27.49684     La Quinta                Texas 268596.46
    ## 2083  -95.25264 30.04729     La Quinta                Texas 268596.46
    ## 2084  -83.98228 35.81166     La Quinta            Tennessee  42144.25
    ## 2085  -83.99993 35.94574     La Quinta            Tennessee  42144.25
    ## 2086  -83.99822 36.05147     La Quinta            Tennessee  42144.25
    ## 2087  -83.77464 35.99921     La Quinta            Tennessee  42144.25
    ## 2088  -86.12470 40.43796     La Quinta              Indiana  36419.55
    ## 2089  -97.84759 30.03152     La Quinta                Texas 268596.46
    ## 2090 -113.26824 37.21125     La Quinta                 Utah  84896.88
    ## 2091  -92.01314 30.25207     La Quinta            Louisiana  52378.13
    ## 2092  -86.81547 40.41317     La Quinta              Indiana  36419.55
    ## 2093  -84.97365 33.03992     La Quinta              Georgia  59425.15
    ## 2094  -93.23908 30.19793     La Quinta            Louisiana  52378.13
    ## 2095  -93.25720 30.24600     La Quinta            Louisiana  52378.13
    ## 2096 -111.47630 36.90552     La Quinta              Arizona 113990.30
    ## 2097  -81.95376 28.09690     La Quinta              Florida  65757.70
    ## 2098  -81.97110 28.08776     La Quinta              Florida  65757.70
    ## 2099  -76.19312 40.02376     La Quinta         Pennsylvania  46054.35
    ## 2100  -99.44455 27.56103     La Quinta                Texas 268596.46
    ## 2101  -99.50325 27.53200     La Quinta                Texas 268596.46
    ## 2102 -106.78817 32.29234     La Quinta           New Mexico 121590.30
    ## 2103 -106.78728 32.29060     La Quinta           New Mexico 121590.30
    ## 2104 -115.15470 36.11684     La Quinta               Nevada 110571.82
    ## 2105 -115.12373 36.07052     La Quinta               Nevada 110571.82
    ## 2106 -115.06192 36.23800     La Quinta               Nevada 110571.82
    ## 2107 -115.30319 36.14568     La Quinta               Nevada 110571.82
    ## 2108 -115.24774 36.21153     La Quinta               Nevada 110571.82
    ## 2109 -115.19028 36.10052     La Quinta               Nevada 110571.82
    ## 2110  -73.75967 42.75110     La Quinta             New York  54554.98
    ## 2111  -98.44339 34.62476     La Quinta             Oklahoma  69898.87
    ## 2112 -118.37272 33.94562     La Quinta           California 163694.74
    ## 2113  -86.30181 36.18129     La Quinta            Tennessee  42144.25
    ## 2114  -93.23799 31.05379     La Quinta            Louisiana  52378.13
    ## 2115  -76.49688 38.29262     La Quinta             Maryland  12405.93
    ## 2116  -84.38514 37.96074     La Quinta             Kentucky  40407.80
    ## 2117  -84.48159 38.10061     La Quinta             Kentucky  40407.80
    ## 2118  -96.68344 40.85504     La Quinta             Nebraska  77347.81
    ## 2119  -95.39490 32.47430     La Quinta                Texas 268596.46
    ## 2120  -92.27588 34.74349     La Quinta             Arkansas  53178.55
    ## 2121  -92.22075 34.79503     La Quinta             Arkansas  53178.55
    ## 2122  -92.39823 34.72668     La Quinta             Arkansas  53178.55
    ## 2123  -92.50057 34.62083     La Quinta             Arkansas  53178.55
    ## 2124 -121.70060 37.71517     La Quinta           California 163694.74
    ## 2125  -94.95272 30.70347     La Quinta                Texas 268596.46
    ## 2126  -84.12361 33.35333     La Quinta              Georgia  59425.15
    ## 2127 -111.83615 41.71650     La Quinta                 Utah  84896.88
    ## 2128  -94.73205 32.54686     La Quinta                Texas 268596.46
    ## 2129 -120.85411 37.05722     La Quinta           California 163694.74
    ## 2130  -84.38997 35.73299     La Quinta            Tennessee  42144.25
    ## 2131  -85.72795 38.19259     La Quinta             Kentucky  40407.80
    ## 2132  -85.53623 38.22045     La Quinta             Kentucky  40407.80
    ## 2133 -105.13041 40.40803     La Quinta             Colorado 104093.67
    ## 2134 -101.85523 33.58996     La Quinta                Texas 268596.46
    ## 2135 -101.91894 33.60156     La Quinta                Texas 268596.46
    ## 2136 -101.84551 33.53560     La Quinta                Texas 268596.46
    ## 2137 -101.90208 33.57590     La Quinta                Texas 268596.46
    ## 2138  -94.72560 31.30524     La Quinta                Texas 268596.46
    ## 2139  -97.59124 29.65548     La Quinta                Texas 268596.46
    ## 2140  -94.21810 30.25923     La Quinta                Texas 268596.46
    ## 2141  -79.17533 37.36832     La Quinta             Virginia  42774.93
    ## 2142 -122.29134 47.81724     La Quinta           Washington  71297.95
    ## 2143  -83.72213 32.80719     La Quinta              Georgia  59425.15
    ## 2144  -83.68522 32.90460     La Quinta              Georgia  59425.15
    ## 2145  -89.29139 43.14345     La Quinta            Wisconsin  65496.38
    ## 2146  -77.51918 38.80586     La Quinta             Virginia  42774.93
    ## 2147  -71.47271 43.00371     La Quinta        New Hampshire   9349.16
    ## 2148  -73.98662 40.74770     La Quinta             New York  54554.98
    ## 2149  -82.51047 40.69432     La Quinta                 Ohio  44825.58
    ## 2150  -97.14104 32.59324     La Quinta                Texas 268596.46
    ## 2151 -121.14162 37.75344     La Quinta           California 163694.74
    ## 2152  -98.27721 30.56105     La Quinta                Texas 268596.46
    ## 2153  -94.36043 32.48638     La Quinta                Texas 268596.46
    ## 2154  -97.81787 28.11077     La Quinta                Texas 268596.46
    ## 2155  -95.75552 34.89940     La Quinta             Oklahoma  69898.87
    ## 2156  -98.26219 26.19898     La Quinta                Texas 268596.46
    ## 2157  -98.23337 26.19321     La Quinta                Texas 268596.46
    ## 2158  -84.21497 33.45953     La Quinta              Georgia  59425.15
    ## 2159  -96.69972 33.13899     La Quinta                Texas 268596.46
    ## 2160  -77.03011 40.25386     La Quinta         Pennsylvania  46054.35
    ## 2161  -80.70188 28.07934     La Quinta              Florida  65757.70
    ## 2162  -80.72164 28.23155     La Quinta              Florida  65757.70
    ## 2163  -90.00687 35.06759     La Quinta            Tennessee  42144.25
    ## 2164  -89.86158 35.16276     La Quinta            Tennessee  42144.25
    ## 2165  -89.86230 35.09798     La Quinta            Tennessee  42144.25
    ## 2166  -89.78150 35.20080     La Quinta            Tennessee  42144.25
    ## 2167  -90.04627 35.14193     La Quinta            Tennessee  42144.25
    ## 2168  -97.88040 26.15992     La Quinta                Texas 268596.46
    ## 2169 -116.35661 43.59764     La Quinta                Idaho  83568.95
    ## 2170  -88.69152 32.35028     La Quinta          Mississippi  48431.78
    ## 2171  -87.31998 41.46954     La Quinta              Indiana  36419.55
    ## 2172 -111.69214 33.38582     La Quinta              Arizona 113990.30
    ## 2173  -80.26401 25.80654     La Quinta              Florida  65757.70
    ## 2174  -80.31689 25.80967     La Quinta              Florida  65757.70
    ## 2175  -80.33896 25.79953     La Quinta              Florida  65757.70
    ## 2176  -80.36624 25.57937     La Quinta              Florida  65757.70
    ## 2177  -80.32926 25.91345     La Quinta              Florida  65757.70
    ## 2178 -102.15528 32.00676     La Quinta                Texas 268596.46
    ## 2179 -102.12216 31.97182     La Quinta                Texas 268596.46
    ## 2180  -71.48641 42.14894     La Quinta        Massachusetts  10554.39
    ## 2181  -83.24852 33.10417     La Quinta              Georgia  59425.15
    ## 2182  -87.93174 42.91490     La Quinta            Wisconsin  65496.38
    ## 2183  -87.91706 43.11538     La Quinta            Wisconsin  65496.38
    ## 2184  -88.37198 43.04778     La Quinta            Wisconsin  65496.38
    ## 2185  -87.91621 43.10965     La Quinta            Wisconsin  65496.38
    ## 2186  -88.10518 42.95197     La Quinta            Wisconsin  65496.38
    ## 2187  -88.16562 43.03496     La Quinta            Wisconsin  65496.38
    ## 2188  -93.27639 44.86058     La Quinta            Minnesota  86935.83
    ## 2189  -93.34733 44.85576     La Quinta            Minnesota  86935.83
    ## 2190  -93.41128 44.97395     La Quinta            Minnesota  86935.83
    ## 2191  -93.39078 45.08183     La Quinta            Minnesota  86935.83
    ## 2192 -101.31952 48.19893     La Quinta         North Dakota  70698.32
    ## 2193  -98.30645 26.19684     La Quinta                Texas 268596.46
    ## 2194 -114.03478 46.91173     La Quinta              Montana 147039.71
    ## 2195 -109.54731 38.56148     La Quinta                 Utah  84896.88
    ## 2196  -87.85406 30.66621     La Quinta              Alabama  52420.07
    ## 2197  -88.04815 30.87083     La Quinta              Alabama  52420.07
    ## 2198  -88.16757 30.58447     La Quinta              Alabama  52420.07
    ## 2199  -88.12949 30.67519     La Quinta              Alabama  52420.07
    ## 2200 -121.07822 37.70964     La Quinta           California 163694.74
    ## 2201  -90.49905 41.45645     La Quinta             Illinois  57913.55
    ## 2202 -102.90709 31.57179     La Quinta                Texas 268596.46
    ## 2203 -117.25980 33.93965     La Quinta           California 163694.74
    ## 2204  -91.18171 29.69577     La Quinta            Louisiana  52378.13
    ## 2205 -121.63308 37.13385     La Quinta           California 163694.74
    ## 2206  -80.01362 39.65694     La Quinta        West Virginia  24230.04
    ## 2207 -117.03750 46.73386     La Quinta                Idaho  83568.95
    ## 2208  -88.53042 30.44292     La Quinta          Mississippi  48431.78
    ## 2209  -74.95640 39.92647     La Quinta           New Jersey   8722.58
    ## 2210  -95.00291 33.17203     La Quinta                Texas 268596.46
    ## 2211  -95.40295 35.76616     La Quinta             Oklahoma  69898.87
    ## 2212  -78.84848 33.72306     La Quinta       South Carolina  32020.49
    ## 2213  -78.88886 33.71473     La Quinta       South Carolina  32020.49
    ## 2214  -81.78349 26.13932     La Quinta              Florida  65757.70
    ## 2215  -81.68764 26.15669     La Quinta              Florida  65757.70
    ## 2216  -86.69124 36.15552     La Quinta            Tennessee  42144.25
    ## 2217  -86.66630 36.14657     La Quinta            Tennessee  42144.25
    ## 2218  -86.82334 35.91155     La Quinta            Tennessee  42144.25
    ## 2219  -86.76198 36.08397     La Quinta            Tennessee  42144.25
    ## 2220 -118.06634 33.83207     La Quinta           California 163694.74
    ## 2221  -98.09222 29.69946     La Quinta                Texas 268596.46
    ## 2222  -95.22802 30.12745     La Quinta                Texas 268596.46
    ## 2223  -76.87870 40.21476     La Quinta         Pennsylvania  46054.35
    ## 2224  -72.91996 41.29404     La Quinta          Connecticut   5543.41
    ## 2225  -91.85662 29.98672     La Quinta            Louisiana  52378.13
    ## 2226  -90.24054 30.00442     La Quinta            Louisiana  52378.13
    ## 2227  -90.15490 29.99475     La Quinta            Louisiana  52378.13
    ## 2228  -90.06898 29.95086     La Quinta            Louisiana  52378.13
    ## 2229  -89.74942 30.28251     La Quinta            Louisiana  52378.13
    ## 2230  -90.20579 30.00570     La Quinta            Louisiana  52378.13
    ## 2231  -90.03919 29.92465     La Quinta            Louisiana  52378.13
    ## 2232  -73.97770 40.77616     La Quinta             New York  54554.98
    ## 2233  -75.80377 39.63761     La Quinta             Maryland  12405.93
    ## 2234 -124.04929 44.61503     La Quinta               Oregon  98378.54
    ## 2235  -78.99289 43.08958     La Quinta             New York  54554.98
    ## 2236  -76.21100 36.87178     La Quinta             Virginia  42774.93
    ## 2237  -76.18455 36.84364     La Quinta             Virginia  42774.93
    ## 2238  -78.65873 33.84463     La Quinta       South Carolina  32020.49
    ## 2239 -100.72673 41.10847     La Quinta             Nebraska  77347.81
    ## 2240 -111.09313 32.35876     La Quinta              Arizona 113990.30
    ## 2241  -89.93871 38.58855     La Quinta             Illinois  57913.55
    ## 2242 -122.19333 37.74251     La Quinta           California 163694.74
    ## 2243 -122.11782 37.66454     La Quinta           California 163694.74
    ## 2244  -82.18284 29.15084     La Quinta              Florida  65757.70
    ## 2245  -75.07279 38.36138     La Quinta             Maryland  12405.93
    ## 2246  -81.01402 29.23932     La Quinta              Florida  65757.70
    ## 2247 -102.28874 31.91762     La Quinta                Texas 268596.46
    ## 2248 -102.28855 31.91849     La Quinta                Texas 268596.46
    ## 2249 -102.31293 31.87320     La Quinta                Texas 268596.46
    ## 2250  -97.57014 35.61071     La Quinta             Oklahoma  69898.87
    ## 2251  -97.60056 35.45861     La Quinta             Oklahoma  69898.87
    ## 2252  -97.42237 35.44605     La Quinta             Oklahoma  69898.87
    ## 2253  -97.49318 35.31915     La Quinta             Oklahoma  69898.87
    ## 2254  -97.48277 35.20720     La Quinta             Oklahoma  69898.87
    ## 2255  -97.60898 35.54538     La Quinta             Oklahoma  69898.87
    ## 2256  -97.72609 35.46703     La Quinta             Oklahoma  69898.87
    ## 2257  -94.81906 38.85592     La Quinta               Kansas  82278.36
    ## 2258 -122.82118 47.04938     La Quinta           Washington  71297.95
    ## 2259  -95.91405 41.28102     La Quinta                 Iowa  56272.81
    ## 2260  -96.07538 41.28946     La Quinta             Nebraska  77347.81
    ## 2261  -96.07995 41.21168     La Quinta             Nebraska  77347.81
    ## 2262 -117.57290 34.07152     La Quinta           California 163694.74
    ## 2263 -117.85324 33.71015     La Quinta           California 163694.74
    ## 2264  -93.82025 30.11279     La Quinta                Texas 268596.46
    ## 2265 -111.70817 40.27270     La Quinta                 Utah  84896.88
    ## 2266 -111.72216 40.31135     La Quinta                 Utah  84896.88
    ## 2267  -81.31299 28.45906     La Quinta              Florida  65757.70
    ## 2268  -81.33881 28.45223     La Quinta              Florida  65757.70
    ## 2269  -81.46840 28.44071     La Quinta              Florida  65757.70
    ## 2270  -81.46011 28.46313     La Quinta              Florida  65757.70
    ## 2271  -81.36403 28.74865     La Quinta              Florida  65757.70
    ## 2272  -81.40564 28.42845     La Quinta              Florida  65757.70
    ## 2273  -81.20722 28.58853     La Quinta              Florida  65757.70
    ## 2274  -81.45298 28.48103     La Quinta              Florida  65757.70
    ## 2275  -88.58172 44.03283     La Quinta            Wisconsin  65496.38
    ## 2276  -94.67074 38.93515     La Quinta               Kansas  82278.36
    ## 2277  -95.84739 36.28474     La Quinta             Oklahoma  69898.87
    ## 2278  -85.79050 33.61342     La Quinta              Alabama  52420.07
    ## 2279  -88.68294 37.08611     La Quinta             Kentucky  40407.80
    ## 2280  -95.62928 31.73423     La Quinta                Texas 268596.46
    ## 2281 -100.97067 35.54992     La Quinta                Texas 268596.46
    ## 2282  -85.90430 30.23685     La Quinta              Florida  65757.70
    ## 2283  -85.76137 30.17441     La Quinta              Florida  65757.70
    ## 2284  -85.64221 30.18936     La Quinta              Florida  65757.70
    ## 2285  -95.51777 33.67348     La Quinta                Texas 268596.46
    ## 2286  -95.18327 29.71359     La Quinta                Texas 268596.46
    ## 2287  -95.15808 29.65862     La Quinta                Texas 268596.46
    ## 2288 -120.67140 35.64517     La Quinta           California 163694.74
    ## 2289  -95.35936 29.55850     La Quinta                Texas 268596.46
    ## 2290  -99.11895 28.89813     La Quinta                Texas 268596.46
    ## 2291 -103.50110 31.39546     La Quinta                Texas 268596.46
    ## 2292  -87.22123 30.50556     La Quinta              Florida  65757.70
    ## 2293  -89.12999 41.36313     La Quinta             Illinois  57913.55
    ## 2294  -98.17840 26.23457     La Quinta                Texas 268596.46
    ## 2295  -98.17921 26.23446     La Quinta                Texas 268596.46
    ## 2296  -75.30739 39.86742     La Quinta         Pennsylvania  46054.35
    ## 2297 -111.97356 33.30693     La Quinta              Arizona 113990.30
    ## 2298 -112.16634 33.46581     La Quinta              Arizona 113990.30
    ## 2299 -111.85207 33.39058     La Quinta              Arizona 113990.30
    ## 2300 -112.11464 33.62570     La Quinta              Arizona 113990.30
    ## 2301 -111.89006 33.58340     La Quinta              Arizona 113990.30
    ## 2302 -111.97705 33.42034     La Quinta              Arizona 113990.30
    ## 2303 -112.11243 33.47947     La Quinta              Arizona 113990.30
    ## 2304 -112.23285 33.63499     La Quinta              Arizona 113990.30
    ## 2305  -83.55285 35.78238     La Quinta            Tennessee  42144.25
    ## 2306  -83.57977 35.80651     La Quinta            Tennessee  42144.25
    ## 2307  -94.70629 37.38281     La Quinta               Kansas  82278.36
    ## 2308  -80.22261 40.50790     La Quinta         Pennsylvania  46054.35
    ## 2309  -71.91094 41.66427     La Quinta          Connecticut   5543.41
    ## 2310  -96.83226 33.06276     La Quinta                Texas 268596.46
    ## 2311  -80.25739 26.11385     La Quinta              Florida  65757.70
    ## 2312  -73.49646 44.69758     La Quinta             New York  54554.98
    ## 2313  -87.94972 42.56653     La Quinta            Wisconsin  65496.38
    ## 2314 -112.43317 42.90052     La Quinta                Idaho  83568.95
    ## 2315 -117.80381 34.04639     La Quinta           California 163694.74
    ## 2316  -97.06704 36.74564     La Quinta             Oklahoma  69898.87
    ## 2317  -90.07197 38.74517     La Quinta             Illinois  57913.55
    ## 2318  -93.98085 29.93724     La Quinta                Texas 268596.46
    ## 2319  -82.04843 27.01716     La Quinta              Florida  65757.70
    ## 2320  -96.63002 28.63212     La Quinta                Texas 268596.46
    ## 2321  -81.02805 29.10752     La Quinta              Florida  65757.70
    ## 2322 -122.54713 45.56896     La Quinta               Oregon  98378.54
    ## 2323 -122.73042 45.55334     La Quinta               Oregon  98378.54
    ## 2324  -70.28195 43.65567     La Quinta                Maine  35379.74
    ## 2325  -86.40979 32.48599     La Quinta              Alabama  52420.07
    ## 2326 -112.38442 34.55118     La Quinta              Arizona 113990.30
    ## 2327 -104.61645 38.32407     La Quinta             Colorado 104093.67
    ## 2328  -73.92823 40.74399     La Quinta             New York  54554.98
    ## 2329  -80.54037 37.12678     La Quinta             Virginia  42774.93
    ## 2330  -78.78415 35.73580     La Quinta       North Carolina  53819.16
    ## 2331  -78.67605 35.83362     La Quinta       North Carolina  53819.16
    ## 2332  -78.81659 35.85856     La Quinta       North Carolina  53819.16
    ## 2333  -78.81696 35.85991     La Quinta       North Carolina  53819.16
    ## 2334 -121.27088 38.60514     La Quinta           California 163694.74
    ## 2335 -103.14887 44.09956     La Quinta         South Dakota  77115.68
    ## 2336  -97.76997 26.48389     La Quinta                Texas 268596.46
    ## 2337 -122.35795 40.57588     La Quinta           California 163694.74
    ## 2338 -119.78237 39.50954     La Quinta               Nevada 110571.82
    ## 2339  -77.61161 37.50930     La Quinta             Virginia  42774.93
    ## 2340  -77.44641 37.84990     La Quinta             Virginia  42774.93
    ## 2341  -77.42758 37.40111     La Quinta             Virginia  42774.93
    ## 2342  -84.31282 37.76943     La Quinta             Kentucky  40407.80
    ## 2343 -107.77357 39.52396     La Quinta             Colorado 104093.67
    ## 2344  -80.03235 37.32127     La Quinta             Virginia  42774.93
    ## 2345  -92.46287 43.95918     La Quinta            Minnesota  86935.83
    ## 2346  -88.97183 42.26858     La Quinta             Illinois  57913.55
    ## 2347  -97.04180 28.05810     La Quinta                Texas 268596.46
    ## 2348  -96.46907 32.89696     La Quinta                Texas 268596.46
    ## 2349  -85.15327 34.22221     La Quinta              Georgia  59425.15
    ## 2350  -95.81175 29.53287     La Quinta                Texas 268596.46
    ## 2351 -104.51994 33.41577     La Quinta           New Mexico 121590.30
    ## 2352  -97.67419 30.47486     La Quinta                Texas 268596.46
    ## 2353 -105.62196 33.32649     La Quinta           New Mexico 121590.30
    ## 2354  -93.13519 35.30382     La Quinta             Arkansas  53178.55
    ## 2355 -121.50518 38.59789     La Quinta           California 163694.74
    ## 2356 -121.35729 38.65968     La Quinta           California 163694.74
    ## 2357  -71.24587 42.77437     La Quinta        New Hampshire   9349.16
    ## 2358 -122.99615 44.92055     La Quinta               Oregon  98378.54
    ## 2359  -97.61046 38.87837     La Quinta               Kansas  82278.36
    ## 2360  -75.59664 38.36431     La Quinta             Maryland  12405.93
    ## 2361 -112.00821 40.77340     La Quinta                 Utah  84896.88
    ## 2362 -111.98796 41.08835     La Quinta                 Utah  84896.88
    ## 2363 -111.90249 40.61993     La Quinta                 Utah  84896.88
    ## 2364 -111.94858 40.69556     La Quinta                 Utah  84896.88
    ## 2365 -100.47581 31.41603     La Quinta                Texas 268596.46
    ## 2366  -98.48105 29.51886     La Quinta                Texas 268596.46
    ## 2367  -98.42939 29.35111     La Quinta                Texas 268596.46
    ## 2368  -98.49741 29.42025     La Quinta                Texas 268596.46
    ## 2369  -98.36260 29.44569     La Quinta                Texas 268596.46
    ## 2370  -98.40180 29.48860     La Quinta                Texas 268596.46
    ## 2371  -98.34608 29.55586     La Quinta                Texas 268596.46
    ## 2372  -98.62756 29.40052     La Quinta                Texas 268596.46
    ## 2373  -98.49949 29.42430     La Quinta                Texas 268596.46
    ## 2374  -98.55567 29.51654     La Quinta                Texas 268596.46
    ## 2375  -98.47801 29.61199     La Quinta                Texas 268596.46
    ## 2376  -98.67509 29.54598     La Quinta                Texas 268596.46
    ## 2377  -98.48247 29.42382     La Quinta                Texas 268596.46
    ## 2378  -98.62810 29.45469     La Quinta                Texas 268596.46
    ## 2379  -98.52562 29.35571     La Quinta                Texas 268596.46
    ## 2380  -98.63380 29.67440     La Quinta                Texas 268596.46
    ## 2381  -98.53371 29.48389     La Quinta                Texas 268596.46
    ## 2382 -117.28313 34.06575     La Quinta           California 163694.74
    ## 2383 -117.31404 33.10635     La Quinta           California 163694.74
    ## 2384 -117.06205 32.64739     La Quinta           California 163694.74
    ## 2385 -117.10640 32.95062     La Quinta           California 163694.74
    ## 2386 -117.21793 32.80693     La Quinta           California 163694.74
    ## 2387 -117.38450 33.20276     La Quinta           California 163694.74
    ## 2388 -117.19466 32.74928     La Quinta           California 163694.74
    ## 2389 -117.15845 32.76354     La Quinta           California 163694.74
    ## 2390 -117.21700 33.16675     La Quinta           California 163694.74
    ## 2391 -122.40754 37.65068     La Quinta           California 163694.74
    ## 2392 -122.40090 37.61026     La Quinta           California 163694.74
    ## 2393 -121.93855 37.37815     La Quinta           California 163694.74
    ## 2394  -97.98011 29.82481     La Quinta                Texas 268596.46
    ## 2395  -97.90993 29.89674     La Quinta                Texas 268596.46
    ## 2396 -116.55193 48.27598     La Quinta                Idaho  83568.95
    ## 2397  -82.68280 41.42724     La Quinta                 Ohio  44825.58
    ## 2398 -119.71000 34.42687     La Quinta           California 163694.74
    ## 2399 -118.56475 34.37607     La Quinta           California 163694.74
    ## 2400 -106.01923 35.63510     La Quinta           New Mexico 121590.30
    ## 2401 -104.66831 34.94549     La Quinta           New Mexico 121590.30
    ## 2402  -82.44975 27.34022     La Quinta              Florida  65757.70
    ## 2403  -82.54716 27.35327     La Quinta              Florida  65757.70
    ## 2404  -81.24062 32.11421     La Quinta              Georgia  59425.15
    ## 2405  -81.28145 32.00267     La Quinta              Georgia  59425.15
    ## 2406  -81.11301 32.01523     La Quinta              Georgia  59425.15
    ## 2407  -81.12649 32.00005     La Quinta              Georgia  59425.15
    ## 2408  -98.27793 29.60523     La Quinta                Texas 268596.46
    ## 2409  -91.69098 35.25142     La Quinta             Arkansas  53178.55
    ## 2410 -122.19984 47.64273     La Quinta           Washington  71297.95
    ## 2411 -122.33945 47.61764     La Quinta           Washington  71297.95
    ## 2412 -122.29702 47.43514     La Quinta           Washington  71297.95
    ## 2413  -81.43109 27.46121     La Quinta              Florida  65757.70
    ## 2414  -74.04943 40.79019     La Quinta           New Jersey   8722.58
    ## 2415  -97.99256 29.58131     La Quinta                Texas 268596.46
    ## 2416  -83.58313 35.93525     La Quinta            Tennessee  42144.25
    ## 2417  -96.91034 35.38522     La Quinta             Oklahoma  69898.87
    ## 2418  -87.74583 43.75719     La Quinta            Wisconsin  65496.38
    ## 2419  -96.61179 33.66830     La Quinta                Texas 268596.46
    ## 2420  -93.86211 32.44942     La Quinta            Louisiana  52378.13
    ## 2421 -106.06564 39.63054     La Quinta             Colorado 104093.67
    ## 2422  -96.78213 43.51451     La Quinta         South Dakota  77115.68
    ## 2423  -89.82952 30.31176     La Quinta            Louisiana  52378.13
    ## 2424  -86.56809 35.96926     La Quinta            Tennessee  42144.25
    ## 2425  -84.03649 33.85316     La Quinta              Georgia  59425.15
    ## 2426  -86.31381 41.69651     La Quinta              Indiana  36419.55
    ## 2427  -73.17318 44.46725     La Quinta              Vermont   9616.36
    ## 2428  -97.16997 26.13794     La Quinta                Texas 268596.46
    ## 2429 -117.40657 47.74490     La Quinta           Washington  71297.95
    ## 2430 -117.19548 47.69130     La Quinta           Washington  71297.95
    ## 2431  -94.18364 36.17391     La Quinta             Arkansas  53178.55
    ## 2432  -93.34643 37.24284     La Quinta             Missouri  69706.99
    ## 2433  -89.64101 39.74629     La Quinta             Illinois  57913.55
    ## 2434  -93.29573 37.16886     La Quinta             Missouri  69706.99
    ## 2435  -72.59526 42.10948     La Quinta        Massachusetts  10554.39
    ## 2436  -73.07295 44.79386     La Quinta              Vermont   9616.36
    ## 2437  -81.40835 29.91861     La Quinta              Florida  65757.70
    ## 2438 -113.58283 37.06301     La Quinta                 Utah  84896.88
    ## 2439  -90.35523 38.78081     La Quinta             Missouri  69706.99
    ## 2440  -90.46371 38.75075     La Quinta             Missouri  69706.99
    ## 2441  -90.43449 38.69824     La Quinta             Missouri  69706.99
    ## 2442  -82.63875 27.83342     La Quinta              Florida  65757.70
    ## 2443  -73.56222 41.04371     La Quinta          Connecticut   5543.41
    ## 2444  -88.79391 33.48111     La Quinta          Mississippi  48431.78
    ## 2445 -106.81493 40.44840     La Quinta             Colorado 104093.67
    ## 2446  -98.24189 32.20166     La Quinta                Texas 268596.46
    ## 2447  -89.53215 44.52311     La Quinta            Wisconsin  65496.38
    ## 2448  -97.13084 36.11456     La Quinta             Oklahoma  69898.87
    ## 2449 -121.34447 37.98303     La Quinta           California 163694.74
    ## 2450  -71.84476 41.39759     La Quinta          Connecticut   5543.41
    ## 2451  -72.10511 42.11455     La Quinta        Massachusetts  10554.39
    ## 2452  -95.59066 33.11861     La Quinta                Texas 268596.46
    ## 2453  -80.83404 38.29828     La Quinta        West Virginia  24230.04
    ## 2454  -80.33133 26.12410     La Quinta              Florida  65757.70
    ## 2455  -80.33159 26.12362     La Quinta              Florida  65757.70
    ## 2456 -100.39020 32.45240     La Quinta                Texas 268596.46
    ## 2457 -100.40216 32.45014     La Quinta                Texas 268596.46
    ## 2458 -122.41236 47.23959     La Quinta           Washington  71297.95
    ## 2459  -84.30501 30.48190     La Quinta              Florida  65757.70
    ## 2460  -82.52590 27.95956     La Quinta              Florida  65757.70
    ## 2461  -82.68515 27.84013     La Quinta              Florida  65757.70
    ## 2462  -82.32711 27.94383     La Quinta              Florida  65757.70
    ## 2463  -82.33421 27.94458     La Quinta              Florida  65757.70
    ## 2464  -82.50636 28.02445     La Quinta              Florida  65757.70
    ## 2465  -82.35804 27.98946     La Quinta              Florida  65757.70
    ## 2466  -82.42662 28.03363     La Quinta              Florida  65757.70
    ## 2467  -82.37111 28.12371     La Quinta              Florida  65757.70
    ## 2468  -82.52320 27.89316     La Quinta              Florida  65757.70
    ## 2469 -118.41349 35.12595     La Quinta           California 163694.74
    ## 2470 -117.16524 33.52292     La Quinta           California 163694.74
    ## 2471  -97.35668 31.10348     La Quinta                Texas 268596.46
    ## 2472  -87.41088 39.43248     La Quinta              Indiana  36419.55
    ## 2473  -96.27757 32.70332     La Quinta                Texas 268596.46
    ## 2474  -96.89703 33.06372     La Quinta                Texas 268596.46
    ## 2475 -118.91002 34.18285     La Quinta           California 163694.74
    ## 2476  -83.60645 41.54936     La Quinta                 Ohio  44825.58
    ## 2477  -95.63344 30.08439     La Quinta                Texas 268596.46
    ## 2478 -104.52120 37.14049     La Quinta             Colorado 104093.67
    ## 2479 -110.93412 32.12478     La Quinta              Arizona 113990.30
    ## 2480 -110.85656 32.22087     La Quinta              Arizona 113990.30
    ## 2481 -110.90924 32.22387     La Quinta              Arizona 113990.30
    ## 2482 -103.72305 35.15462     La Quinta           New Mexico 121590.30
    ## 2483 -119.33710 36.22661     La Quinta           California 163694.74
    ## 2484  -95.90143 36.16169     La Quinta             Oklahoma  69898.87
    ## 2485  -95.76021 36.16780     La Quinta             Oklahoma  69898.87
    ## 2486  -95.91004 36.10319     La Quinta             Oklahoma  69898.87
    ## 2487 -122.90733 47.00628     La Quinta           Washington  71297.95
    ## 2488  -88.71558 34.27700     La Quinta          Mississippi  48431.78
    ## 2489  -87.52546 33.16970     La Quinta              Alabama  52420.07
    ## 2490 -114.46932 42.59198     La Quinta                Idaho  83568.95
    ## 2491  -95.26581 32.32226     La Quinta                Texas 268596.46
    ## 2492  -95.32055 32.30385     La Quinta                Texas 268596.46
    ## 2493  -84.53781 33.57430     La Quinta              Georgia  59425.15
    ## 2494  -80.25070 26.26840     La Quinta              Florida  65757.70
    ## 2495  -82.41811 28.05358     La Quinta              Florida  65757.70
    ## 2496  -83.32766 30.84036     La Quinta              Georgia  59425.15
    ## 2497 -122.65660 45.71820     La Quinta           Washington  71297.95
    ## 2498 -119.21099 34.25129     La Quinta           California 163694.74
    ## 2499  -75.59253 43.12233     La Quinta             New York  54554.98
    ## 2500  -90.83901 32.34314     La Quinta          Mississippi  48431.78
    ## 2501  -96.97905 28.76754     La Quinta                Texas 268596.46
    ## 2502  -96.99781 28.86622     La Quinta                Texas 268596.46
    ## 2503  -75.97871 36.85640     La Quinta             Virginia  42774.93
    ## 2504 -119.35144 36.32603     La Quinta           California 163694.74
    ## 2505  -76.89823 38.64026     La Quinta             Maryland  12405.93
    ## 2506  -90.86196 30.47309     La Quinta            Louisiana  52378.13
    ## 2507 -118.37190 46.06788     La Quinta           Washington  71297.95
    ## 2508  -83.70226 32.61561     La Quinta              Georgia  59425.15
    ## 2509  -71.43507 41.75233     La Quinta         Rhode Island   1544.89
    ## 2510  -89.65951 44.95911     La Quinta            Wisconsin  65496.38
    ## 2511  -96.83625 32.41658     La Quinta                Texas 268596.46
    ## 2512  -98.65447 35.53415     La Quinta             Oklahoma  69898.87
    ## 2513  -97.79007 32.73230     La Quinta                Texas 268596.46
    ## 2514 -120.33318 47.45163     La Quinta           Washington  71297.95
    ## 2515  -74.01985 40.30118     La Quinta           New Jersey   8722.58
    ## 2516  -92.16572 32.50924     La Quinta            Louisiana  52378.13
    ## 2517  -80.13517 26.70807     La Quinta              Florida  65757.70
    ## 2518  -80.09100 26.71676     La Quinta              Florida  65757.70
    ## 2519  -97.40650 37.67330     La Quinta               Kansas  82278.36
    ## 2520  -98.51871 33.93512     La Quinta                Texas 268596.46
    ## 2521  -98.53322 33.86322     La Quinta                Texas 268596.46
    ## 2522  -97.20012 37.73120     La Quinta               Kansas  82278.36
    ## 2523 -112.19676 35.25083     La Quinta              Arizona 113990.30
    ## 2524  -76.70980 37.28380     La Quinta             Virginia  42774.93
    ## 2525 -122.76757 45.33631     La Quinta               Oregon  98378.54
    ## 2526  -78.15193 39.16033     La Quinta             Virginia  42774.93
    ## 2527  -94.38376 29.82996     La Quinta                Texas 268596.46
    ## 2528  -80.31019 36.06214     La Quinta       North Carolina  53819.16
    ## 2529 -122.88250 45.15223     La Quinta               Oregon  98378.54
    ## 2530  -92.95483 44.93868     La Quinta            Minnesota  86935.83
    ## 2531  -95.45282 30.18001     La Quinta                Texas 268596.46
    ## 2532  -95.57321 30.22108     La Quinta                Texas 268596.46
    ## 2533  -99.37608 36.41109     La Quinta             Oklahoma  69898.87
    ## 2534  -97.19313 31.51053     La Quinta                Texas 268596.46
    ## 2535  -96.56281 33.00785     La Quinta                Texas 268596.46
    ## 2536  -81.06095 36.94772     La Quinta             Virginia  42774.93
    ## 2537  -76.76357 39.97675     La Quinta         Pennsylvania  46054.35
    ## 2538 -112.99730 37.18936     La Quinta                 Utah  84896.88
    ## 2539         NA       NA     La Quinta             Delaware   2488.72
    ## 2540         NA       NA     La Quinta               Hawaii  10931.72
    ## 2541         NA       NA     La Quinta District of Columbia     68.34

We can plot the locations of the two establishments using a scatter plot
and color the points by the establishment type. Note that longitude
should be plotted on the *x*-axis and latitude on the *y*-axis.

**Response**:

### More with visualizing

The following two items ask you to create visualizations. These should
follow best practices such as informative titles, axis labels, etc. See
<http://ggplot2.tidyverse.org/reference/labs.html> for help with the
syntax.

You can also choose different themes to change the overall look of your
plots. See <http://ggplot2.tidyverse.org/reference/ggtheme.html> for
help with these.

The general idea here is that create the map using a dataset of shape
files, then overlay a layer of points using a different dataset.

Note that [choropleth
maps](https://r-charts.com/spatial/choropleth-map-ggplot2/) are slightly
different in that you would need to joining the shape file and the data
containing ammounts to shade each shape.

#### Michigan locations

Filter the data for observations in Michigan only, and create a plot.
Try adjusting the transparency of the points by setting the `alpha`
level (so that it is easier to see the over-plotted ones). Visually,
does Mitch Hedberg’s joke appear to hold in Michigan?

**Response**:

#### Texas locations

Now filter the data for observations in Texas only. Create the plot,
with an appropriate `alpha` level. Visually, does Mitch Hedberg’s joke
appear to hold here?

**Response**:

### Challenge: Dress up your maps

Hadley’s [ggplot2](https://ggplot2-book.org/maps.html) text shows how to
use the `geom_polygon` and `coord_quickmap` layers to create state
outlines when plotting spatial data.

This blog post series on **r-spatial** goes into a lot more detail for
dressing up maps:

-   [Basics](https://www.r-spatial.org/r/2018/10/25/ggplot2-sf.html)
-   [Layers](https://www.r-spatial.org/r/2018/10/25/ggplot2-sf-2.html)
-   [Layout](https://www.r-spatial.org/r/2018/10/25/ggplot2-sf-3.html)

Add state boundaries to each of your maps. Do this within each of the
previous code chunks.

![](README-img/noun_pause.png) **Planned Pause Point**: If you have any
questions, contact your instructor. Otherwise feel free to continue on.

## Attribution

Inspiration for this Activity was provided by [Mine
Çetinkaya-Rundel](http://www2.stat.duke.edu/courses/Spring18/Sta199/).
