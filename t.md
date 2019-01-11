---
title: "Dataframe Manipulation with R"
author: "Achmad Wildan Al Aziz"
output: 
  html_document:
      self_contained: true
      keep_md: true
---



![Data Science Indonesia](logo DSI.png)

## Examine Data
Data yang digunakan adalah data yang mengungkap bahwa beberapa dokter yang dibayar sebagai "ahali" oleh perusahaan obat Pfizer memiliki masalah kedisiplininan.

```r
pfizer=read.csv("pfizer.csv")
```

variabel yang digunakan adalah sebagai berikut:

  + `org_indiv Full` name of the doctor, or their organization.
  +``first_plus Doctor's` first and middle names.
  + `first_name` `last_name` First and last names.
  + `city` `state` City and state
  + `category of payment` Type of payment, which include *Expert-led Forums*, in which doctors lecture their peers on using Pfizer's drugs, and `Professional Advising.
  + `cash` Value of payments made in cash.
  + `other` Value of payments made in-kind, for example puschase of meals.
  + `total` value of payment, whether cash or in-kind.

dengan `str` *function* dapat diketahui informasi dari setiap variabel, diantaranya tipe data dan beberapa data awal dari  **pfizer**.


```r
str(pfizer)
```

```
## 'data.frame':	10087 obs. of  10 variables:
##  $ org_indiv : Factor w/ 4851 levels "3-D MEDICAL SERVICES LLC",..: 1 2 3 3 3 4 5 5 6 7 ...
##  $ first_plus: Factor w/ 4048 levels "","A MARK","AAKASH MOHAN",..: 3604 3 2244 2244 2244 15 3487 3487 1207 1488 ...
##  $ first_name: Factor w/ 1577 levels "","A","AAKASH",..: 1386 3 844 844 844 8 1330 1330 484 607 ...
##  $ last_name : Factor w/ 3617 levels "---","ABBO","ABEBE",..: 756 36 2 2 2 2628 3 3 52 6 ...
##  $ city      : Factor w/ 1318 levels "A","ABERDEEN",..: 811 904 739 739 739 388 557 557 3 967 ...
##  $ state     : Factor w/ 52 levels "AK","AL","AR",..: 19 5 10 10 10 23 16 16 45 46 ...
##  $ category  : Factor w/ 9 levels "","Business Related Travel",..: 9 4 2 6 9 4 3 4 9 2 ...
##  $ cash      : int  2625 1000 0 0 1800 750 0 825 3000 0 ...
##  $ other     : int  0 0 448 119 0 0 47 0 0 396 ...
##  $ total     : int  2625 1000 448 119 1800 750 47 825 3000 396 ...
```

Tipe data `chr` merupakan *character* atau *string* (teks) yang diperlakukan sebagai *factor* atau *Categorical Variable*. Tipe data `int` adalag integer, diperlakukan sebagai numerik biasa.


Dengan `summary` *function* dapat ditampilkan deskriptif dari data, seperti *mean*, *median*, dan *quartile* untuk data berskala kontinyu. Untuk data kategori bisa diketahui frekuensi dari setiap faktor.


```r
summary(pfizer)
```

```
##                                    org_indiv            first_plus  
##  REGENTS OF THE UNIVERSITY OF CALIFORNIA:   10               : 203  
##  ANOOSHIAN, JOHN VARTAN                 :    5   DAVID ALAN  :  18  
##  COHEN, SETH ALEXANDER                  :    5   RICHARD ALAN:  16  
##  GROW, CHELSEA RENEE                    :    5   RICHARD JOHN:  16  
##  KELLOGG, JASON PAUL                    :    5   WILLIAM     :  16  
##  LIONBERGER, DAVID R                    :    5   MICHAEL D   :  14  
##  (Other)                                :10052   (Other)     :9804  
##    first_name     last_name              city          state     
##  DAVID  : 301   ---    : 203   NEW YORK    : 256   CA     :1172  
##  ROBERT : 296   PATEL  :  54   LOS ANGELES : 188   NY     : 833  
##  JOHN   : 261   BROWN  :  38   HOUSTON     : 160   FL     : 738  
##  MICHAEL: 244   SMITH  :  38   PHILADELPHIA: 143   TX     : 624  
##         : 203   SHAH   :  35   CHICAGO     : 134   PA     : 533  
##  RICHARD: 188   MILLER :  34   BOSTON      : 128   OH     : 462  
##  (Other):8594   (Other):9685   (Other)     :9078   (Other):5725  
##                                                     category   
##  Meals                                                  :3100  
##  Expert-Led Forums                                      :2857  
##  Business Related Travel                                :2152  
##  Professional Advising                                  :1525  
##  Pfizer Sponsored Research initiated before July 1, 2009: 203  
##  Educational Items                                      : 171  
##  (Other)                                                :  79  
##       cash             other             total        
##  Min.   :      0   Min.   :    0.0   Min.   :      0  
##  1st Qu.:      0   1st Qu.:    0.0   1st Qu.:    191  
##  Median :      0   Median :   41.0   Median :    750  
##  Mean   :   3241   Mean   :  266.5   Mean   :   3507  
##  3rd Qu.:   2000   3rd Qu.:  262.0   3rd Qu.:   2000  
##  Max.   :1185466   Max.   :27681.0   Max.   :1185466  
##  NA's   :1         NA's   :3
```


## Manipulate Data

Dengan library **dplyr* bisa dilakukan manipulasi dataframe meliputi:

 + Sort: Largest to smallest, oldest to newest, alphabetical etc.
 + Filter: Select a defined subset of the data.
 + Summarize/Aggregate: Deriving one value from a series of other values to produce a summary statistic. Examples include: count, sum, mean, median, maximum, minimum etc. Often you'll group data into categories first, and then aggregate by group.
 + Join: Merging entries from two or more datasets based on common field(s), e.g. unique ID number, last name and first name.

manipulasi Dataframe dengan **dplyr** mirip pada SQL.

function dasar yang berguna pada **dplyr** diantaranya.

  + `select` Choose which columns to include.
  + `filter` Filter the data.
  + `arrange` Sort the data, by size for continuous variables, by date, or alphabetically.
  + `group_by` Group the data by a categorical variable.
  + `summarize` Summarize, or aggregate (for each group if following `group_by`). Often used in conjunction with functions including:
    - `mean` Calculate the mean, or average.
    - `median` Calculate the median.
    - `max` Find the maximum value.
    - `min` Find the minimum value
    - `sum` Add all the values together.
    - `n` Count the number of records.
  + `mutate` Create new column(s) in the data, or change existing column(s).
  + `rename` Rename column(s).
  + `bind_rows` Merge two data frames into one, combining data from columns with the same name.


```r
library(dplyr)
```

### Filter & Sort

Dengan `filter` dan `sort` dapat dilakukan berbagai macam eksplorasi penggalian informasi dari data.

#### Mendapatkan Dokter di California dengan Bayaran $10.000 atau lebih Pada Kategori "Expert-Led Forums".


```r
ca_expert_10000 <- pfizer %>%
  filter(state == "CA" & total >= 10000 & category == "Expert-Led Forums") %>%
  arrange(desc(total))

head(ca_expert_10000)
```

```
##                                               org_indiv     first_plus
## 1                                 SACKS, GERALD MICHAEL GERALD MICHAEL
## 2                                       NIDES, MITCHELL       MITCHELL
## 3                                  POTKIN, STEVEN GARTH   STEVEN GARTH
## 4                                  GINSBERG, DAVID ALAN     DAVID ALAN
## 5                                         LOUIE, SAMUEL         SAMUEL
## 6 INSTITUTE OF CLINICAL OUTCOMES RESEARCH AND EDUCATION       GURKIPAL
##   first_name last_name         city state          category   cash other
## 1     GERALD     SACKS SANTA MONICA    CA Expert-Led Forums 146500     0
## 2   MITCHELL     NIDES  LOS ANGELES    CA Expert-Led Forums  70500     0
## 3     STEVEN    POTKIN       ORANGE    CA Expert-Led Forums  48350     0
## 4      DAVID  GINSBERG  LOS ANGELES    CA Expert-Led Forums  45750     0
## 5     SAMUEL     LOUIE   SACRAMENTO    CA Expert-Led Forums  41250     0
## 6   GURKIPAL     SINGH     WOODSIDE    CA Expert-Led Forums  40000     0
##    total
## 1 146500
## 2  70500
## 3  48350
## 4  45750
## 5  41250
## 6  40000
```


#### Mendapatkan Dokter di California atau New York dengan Bayran $10,000 atau lebih pada Kategori "Expert-Led Forums."


```r
ca_ny_expert_10000 <- pfizer %>%
  filter((state == "CA" | state == "NY") & total >= 10000 & category == "Expert-Led Forums") %>%
  arrange(desc(total))

head(ca_ny_expert_10000)
```

```
##                 org_indiv     first_plus first_name    last_name
## 1   SACKS, GERALD MICHAEL GERALD MICHAEL     GERALD        SACKS
## 2         NIDES, MITCHELL       MITCHELL   MITCHELL        NIDES
## 3   SOLERA CONSULTING LLC STEVEN ABRAHAM     STEVEN       KAPLAN
## 4 STUBBLEFIELD, MICHAEL D      MICHAEL D    MICHAEL STUBBLEFIELD
## 5    POTKIN, STEVEN GARTH   STEVEN GARTH     STEVEN       POTKIN
## 6    GINSBERG, DAVID ALAN     DAVID ALAN      DAVID     GINSBERG
##           city state          category   cash other  total
## 1 SANTA MONICA    CA Expert-Led Forums 146500     0 146500
## 2  LOS ANGELES    CA Expert-Led Forums  70500     0  70500
## 3    CHAPPAQUA    NY Expert-Led Forums  56500     0  56500
## 4     NEW YORK    NY Expert-Led Forums  50500     0  50500
## 5       ORANGE    CA Expert-Led Forums  48350     0  48350
## 6  LOS ANGELES    CA Expert-Led Forums  45750     0  45750
```

#### Dapatkan Dokter Selain di California dengan Bayaran $10,000 atau lebih pada Kategori "Expert-Led Forums"


```r
not_ca_expert_10000 <- pfizer %>%
  filter(state != "CA" & total >= 10000 & category=="Expert-Led Forums") %>%
  arrange(desc(total))

head(not_ca_expert_10000)
```

```
##               org_indiv     first_plus first_name last_name        city
## 1    HESS, TODD MICHAEL   TODD MICHAEL       TODD      HESS  SAINT PAUL
## 2   MILLER, JOHN JOSEPH    JOHN JOSEPH       JOHN    MILLER      EXETER
## 3  WEATHERS, VIVIAN JOY     VIVIAN JOY     VIVIAN  WEATHERS        APEX
## 4      ROBERT B NETT MD  ROBERT BURTON     ROBERT      NETT SAN ANTONIO
## 5 SOLERA CONSULTING LLC STEVEN ABRAHAM     STEVEN    KAPLAN   CHAPPAQUA
## 6   GRIFFIN, JAMES DALE     JAMES DALE      JAMES   GRIFFIN      DALLAS
##   state          category  cash other total
## 1    MN Expert-Led Forums 79000     0 79000
## 2    NH Expert-Led Forums 78000     0 78000
## 3    NC Expert-Led Forums 75400     0 75400
## 4    TX Expert-Led Forums 60750     0 60750
## 5    NY Expert-Led Forums 56500     0 56500
## 6    TX Expert-Led Forums 54250     0 54250
```

#### Dapatkan 20 Dokter Pada Setiap *States* (CA, TX, FL, NY) Dengan Bayaran Tertinggi Pada Kategori "Professioal Advising"

```r
ca_ny_tx_fl_prof_top20 <- pfizer %>%
  filter((state=="CA" | state == "NY" | state == "TX" | state == "FL") & category == "Professional Advising") %>%
  arrange(desc(total)) %>%
  head(20)

head(ca_ny_tx_fl_prof_top20)
```

```
##                                 org_indiv      first_plus first_name
## 1                    BAILES, JOSEPH SWITZ    JOSEPH SWITZ     JOSEPH
## 2                SAWYERS, CHARLES LAZELLE CHARLES LAZELLE    CHARLES
## 3                    MALENKA, ROBERT CHAS     ROBERT CHAS     ROBERT
## 4                     BEUTLER, BRUCE ALAN      BRUCE ALAN      BRUCE
## 5 REGENTS OF THE UNIVERSITY OF CALIFORNIA   DAVID RAYMOND      DAVID
## 6                      PTACEK, LOUIS JOHN      LOUIS JOHN      LOUIS
##   last_name          city state              category   cash other  total
## 1    BAILES        AUSTIN    TX Professional Advising 105000     0 105000
## 2   SAWYERS      NEW YORK    NY Professional Advising 100000     0 100000
## 3   MALENKA      STANFORD    CA Professional Advising  75566     0  75566
## 4   BEUTLER        DALLAS    TX Professional Advising  50000     0  50000
## 5   GANDARA        IRVINE    CA Professional Advising  38500     0  38500
## 6    PTACEK SAN FRANCISCO    CA Professional Advising  37588     0  37588
```

#### Filter the data for all payments for running Expert-Led Forums or for Professional Advising, and arrange alphabetically by doctor (last name, then first name)


```r
expert_advice <- pfizer %>%
  filter(category == "Expert-Led Forums" | category == "Professional Advising") %>%
  arrange(last_name, first_name)

head(expert_advice)
```

```
##                                org_indiv       first_plus first_name
## 1                 ABBO, LILIAN MARGARITA LILIAN MARGARITA     LILIAN
## 2                        ABEBE, SHEILA Y         SHEILA Y     SHEILA
## 3 NEW YORK UNIVERSITY SCHOOL OF MEDICINE       JUDITH ANN     JUDITH
## 4                        ABOLNIK, IGOR Z           IGOR Z       IGOR
## 5                        ABRAKSIA, SAMIR            SAMIR      SAMIR
## 6                        ABRAKSIA, SAMIR            SAMIR      SAMIR
##   last_name         city state              category cash other total
## 1      ABBO        MIAMI    FL Professional Advising 1800     0  1800
## 2     ABEBE INDIANAPOLIS    IN     Expert-Led Forums  825     0   825
## 3     ABERG     NEW YORK    NY Professional Advising 1750     0  1750
## 4   ABOLNIK        PROVO    UT     Expert-Led Forums 1750     0  1750
## 5  ABRAKSIA    BEACHWOOD    OH     Expert-Led Forums 2000     0  2000
## 6  ABRAKSIA    BEACHWOOD    OH Professional Advising 2500     0  2500
```

#### Use pattern matching to filter text


```r
expert_advice <- pfizer %>%
  filter(grepl("Expert|Professional", category)) %>%
  arrange(last_name, first_name)

not_expert_advice <- pfizer %>%
  filter(!grepl("Expert|Professional", category)) %>%
  arrange(last_name, first_name)
```

#### Append one data frame to another


```r
pfizer2 <- bind_rows(expert_advice, not_expert_advice)
head(pfizer2)
```

```
##                                org_indiv       first_plus first_name
## 1                 ABBO, LILIAN MARGARITA LILIAN MARGARITA     LILIAN
## 2                        ABEBE, SHEILA Y         SHEILA Y     SHEILA
## 3 NEW YORK UNIVERSITY SCHOOL OF MEDICINE       JUDITH ANN     JUDITH
## 4                        ABOLNIK, IGOR Z           IGOR Z       IGOR
## 5                        ABRAKSIA, SAMIR            SAMIR      SAMIR
## 6                        ABRAKSIA, SAMIR            SAMIR      SAMIR
##   last_name         city state              category cash other total
## 1      ABBO        MIAMI    FL Professional Advising 1800     0  1800
## 2     ABEBE INDIANAPOLIS    IN     Expert-Led Forums  825     0   825
## 3     ABERG     NEW YORK    NY Professional Advising 1750     0  1750
## 4   ABOLNIK        PROVO    UT     Expert-Led Forums 1750     0  1750
## 5  ABRAKSIA    BEACHWOOD    OH     Expert-Led Forums 2000     0  2000
## 6  ABRAKSIA    BEACHWOOD    OH Professional Advising 2500     0  2500
```


### Group & Summarize

#### Calculate the total payments, by state


```r
state_sum <- pfizer %>%
  group_by(state) %>%
  summarize(sum = sum(total)) %>%
  arrange(desc(sum))
```

#### Calculate some additional summary statistics, by state


```r
state_summary <- pfizer %>%
  group_by(state) %>%
  summarize(sum = sum(total), median = median(total), count = n()) %>%
  arrange(desc(sum))
```

#### Group and summarize for multiple categories


```r
# as above, but group by state and category
state_category_summary <- pfizer %>%
  group_by(state, category) %>%
  summarize(sum = sum(total), median = median(total), count = n()) %>%
  arrange(state, category)
```

## Date & Time Format

```r
fda=read.csv("fda.csv")
```

Data yang digunakan merupakan data surat peringatan yang dikirim oleh FDA (Pengawas Obat dan Makanan di US) kepada Dokter. Variabelnya sebagai berikut.

  + `name_las1` `name_firs`t `name_middle` Doctor's last, first, and middle names.
  + `issued` Date letter was sent.
  + `office` Office within the FDA that sent the letter.


```r
str(fda)
```

```
## 'data.frame':	272 obs. of  5 variables:
##  $ name_last  : Factor w/ 253 levels "ADELGLASS","ADKINSON",..: 1 2 3 4 5 6 7 8 9 10 ...
##  $ name_first : Factor w/ 162 levels "A.","ABDOOL",..: 70 108 96 32 59 24 33 122 119 75 ...
##  $ name_middle: Factor w/ 51 levels "","A","A.","AL",..: 32 18 44 1 8 28 50 1 35 13 ...
##  $ issued     : Factor w/ 239 levels "1996-11-19","1997-01-10",..: 29 46 77 131 122 40 53 89 111 38 ...
##  $ office     : Factor w/ 8 levels "Baltimore District Office",..: 4 2 3 2 3 3 2 3 3 3 ...
```

```r
fda$issued=as.Date(fda$issued)
```

#### Filter the data for letters sent from the start of 2005 onwards


```r
post2005 <- fda %>%
  filter(issued >= "2005-01-01") %>%
  arrange(issued)

head(post2005)
```

```
##   name_last name_first name_middle     issued
## 1    MARCUM       JOHN          W. 2005-01-27
## 2   TENNANT     JERALD          L. 2005-02-15
## 3   NICHOLS      TRENT             2005-02-24
## 4   NICHOLS     TRENT.             2005-02-24
## 5   WILLIAM         A.        GRAY 2005-03-16
## 6      WEIL     LOWELL          S. 2005-03-29
##                                       office
## 1 Center for Devices and Radiological Health
## 2 Center for Devices and Radiological Health
## 3 Center for Devices and Radiological Health
## 4 Center for Devices and Radiological Health
## 5 Center for Devices and Radiological Health
## 6 Center for Devices and Radiological Health
```


#### Count the number of letters issued by year


```r
letters_year <- fda %>%
  mutate(year = format(issued, "%Y")) %>%
  group_by(year) %>%
  summarize(letters=n())

head(letters_year)
```

```
## # A tibble: 6 x 2
##   year  letters
##   <chr>   <int>
## 1 1996        1
## 2 1997       15
## 3 1998        8
## 4 1999       14
## 5 2000       25
## 6 2001       19
```

#### Add columns giving the number of days and weeks that have elapsed since each letter was sent


```r
fda <- fda %>%
  mutate(days_elapsed = Sys.Date() - issued,
          weeks_elapsed = difftime(Sys.Date(), issued, units = "weeks"))

head(fda)
```

```
##   name_last name_first name_middle     issued
## 1 ADELGLASS    JEFFREY          M. 1999-05-25
## 2  ADKINSON         N.    FRANKLIN 2000-04-19
## 3     ALLEN       MARK          S. 2002-01-28
## 4 AMSTERDAM     DANIEL             2004-11-17
## 5   AMSTUTZ     HARLAN          C. 2004-07-19
## 6  ANDERSON         C.      JOSEPH 2000-02-25
##                                         office days_elapsed
## 1      Center for Drug Evaluation and Research    7172 days
## 2 Center for Biologics Evaluation and Research    6842 days
## 3   Center for Devices and Radiological Health    6193 days
## 4 Center for Biologics Evaluation and Research    5169 days
## 5   Center for Devices and Radiological Health    5290 days
## 6   Center for Devices and Radiological Health    6896 days
##     weeks_elapsed
## 1 1024.5714 weeks
## 2  977.4286 weeks
## 3  884.7143 weeks
## 4  738.4286 weeks
## 5  755.7143 weeks
## 6  985.1429 weeks
```

