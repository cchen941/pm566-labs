Lab10
================
Chen Chen 6381370662
2022-11-18

``` r
# install.packages(c("RSQLite", "DBI"))
if(!require(RSQLite))install.packages(c("RSQLite"))
```

    ## Loading required package: RSQLite

``` r
if(!require(DBI))install.packages(c("DBI"))
```

    ## Loading required package: DBI

``` r
library(RSQLite)
library(DBI)
```

\#Data setup

``` r
# Initialize a temporary in memory database
con <- dbConnect(SQLite(), ":memory:")

# Download tables
actor <- read.csv("https://raw.githubusercontent.com/ivanceras/sakila/master/csv-sakila-db/actor.csv")
rental <- read.csv("https://raw.githubusercontent.com/ivanceras/sakila/master/csv-sakila-db/rental.csv")
customer <- read.csv("https://raw.githubusercontent.com/ivanceras/sakila/master/csv-sakila-db/customer.csv")
payment <- read.csv("https://raw.githubusercontent.com/ivanceras/sakila/master/csv-sakila-db/payment_p2007_01.csv")

# Copy data.frames to database
dbWriteTable(con, "actor", actor)
dbWriteTable(con, "rental", rental)
dbWriteTable(con, "customer", customer)
dbWriteTable(con, "payment", payment)
dbListTables(con)
```

    ## [1] "actor"    "customer" "payment"  "rental"

``` sql
PRAGMA table_info(actor)
```

| cid | name        | type    | notnull | dflt_value |  pk |
|:----|:------------|:--------|--------:|:-----------|----:|
| 0   | actor_id    | INTEGER |       0 | NA         |   0 |
| 1   | first_name  | TEXT    |       0 | NA         |   0 |
| 2   | last_name   | TEXT    |       0 | NA         |   0 |
| 3   | last_update | TEXT    |       0 | NA         |   0 |

4 records

Exercise 1 Retrieve the actor ID, first name and last name for all
actors using the actor table. Sort by last name and then by first name.

``` r
dbGetQuery(con, "
           SELECT  actor_id, first_name, last_name
           FROM    actor
           ORDER by last_name, first_name
           ")
```

    ##     actor_id  first_name    last_name
    ## 1         58   CHRISTIAN       AKROYD
    ## 2        182      DEBBIE       AKROYD
    ## 3         92     KIRSTEN       AKROYD
    ## 4        118        CUBA        ALLEN
    ## 5        145         KIM        ALLEN
    ## 6        194       MERYL        ALLEN
    ## 7         76    ANGELINA      ASTAIRE
    ## 8        112     RUSSELL       BACALL
    ## 9        190      AUDREY       BAILEY
    ## 10        67     JESSICA       BAILEY
    ## 11       115    HARRISON         BALE
    ## 12       187       RENEE         BALL
    ## 13        47       JULIA    BARRYMORE
    ## 14       158      VIVIEN     BASINGER
    ## 15       174     MICHAEL       BENING
    ## 16       124    SCARLETT       BENING
    ## 17        14      VIVIEN       BERGEN
    ## 18       121        LIZA      BERGMAN
    ## 19        91 CHRISTOPHER        BERRY
    ## 20        60       HENRY        BERRY
    ## 21        12        KARL        BERRY
    ## 22       189        CUBA        BIRCH
    ## 23        25       KEVIN        BLOOM
    ## 24       185     MICHAEL       BOLGER
    ## 25        37         VAL       BOLGER
    ## 26        98       CHRIS      BRIDGES
    ## 27        39      GOLDIE        BRODY
    ## 28       159       LAURA        BRODY
    ## 29       167    LAURENCE      BULLOCK
    ## 30        40      JOHNNY         CAGE
    ## 31        11        ZERO         CAGE
    ## 32       181     MATTHEW       CARREY
    ## 33        86        GREG      CHAPLIN
    ## 34         3          ED        CHASE
    ## 35       176         JON        CHASE
    ## 36       183     RUSSELL        CLOSE
    ## 37        16        FRED      COSTNER
    ## 38       129       DARYL     CRAWFORD
    ## 39        26         RIP     CRAWFORD
    ## 40        49        ANNE       CRONYN
    ## 41       104    PENELOPE       CRONYN
    ## 42       105      SIDNEY        CROWE
    ## 43        57        JUDE       CRUISE
    ## 44       201         TOM       CRUISE
    ## 45       203         TOM       CRUISE
    ## 46       205         TOM       CRUISE
    ## 47       207         TOM       CRUISE
    ## 48        80       RALPH         CRUZ
    ## 49        81    SCARLETT        DAMON
    ## 50         4    JENNIFER        DAVIS
    ## 51       101       SUSAN        DAVIS
    ## 52       110       SUSAN        DAVIS
    ## 53        48     FRANCES    DAY-LEWIS
    ## 54        35        JUDY         DEAN
    ## 55       143       RIVER         DEAN
    ## 56       148       EMILY          DEE
    ## 57       138     LUCILLE          DEE
    ## 58       107        GINA    DEGENERES
    ## 59        41       JODIE    DEGENERES
    ## 60       166        NICK    DEGENERES
    ## 61        89    CHARLIZE        DENCH
    ## 62       123    JULIANNE        DENCH
    ## 63       160       CHRIS         DEPP
    ## 64       100     SPENCER         DEPP
    ## 65       109   SYLVESTER         DERN
    ## 66       173        ALAN     DREYFUSS
    ## 67        36        BURT      DUKAKIS
    ## 68       188        ROCK      DUKAKIS
    ## 69       106     GROUCHO        DUNST
    ## 70        19         BOB      FAWCETT
    ## 71       199       JULIA      FAWCETT
    ## 72        10   CHRISTIAN        GABLE
    ## 73       165          AL      GARLAND
    ## 74       184    HUMPHREY      GARLAND
    ## 75       127       KEVIN      GARLAND
    ## 76       154       MERYL       GIBSON
    ## 77        46      PARKER     GOLDBERG
    ## 78       139        EWAN      GOODING
    ## 79       191     GREGORY      GOODING
    ## 80        71        ADAM        GRANT
    ## 81       179          ED      GUINESS
    ## 82         1    PENELOPE      GUINESS
    ## 83        90        SEAN      GUINESS
    ## 84        32         TIM      HACKMAN
    ## 85       175     WILLIAM      HACKMAN
    ## 86       202         TOM        HANKS
    ## 87       204         TOM        HANKS
    ## 88       206         TOM        HANKS
    ## 89       208         TOM        HANKS
    ## 90       152         BEN       HARRIS
    ## 91       141        CATE       HARRIS
    ## 92        56         DAN       HARRIS
    ## 93        97         MEG        HAWKE
    ## 94       151    GEOFFREY       HESTON
    ## 95       169     KENNETH      HOFFMAN
    ## 96        79         MAE      HOFFMAN
    ## 97        28       WOODY      HOFFMAN
    ## 98       161      HARVEY         HOPE
    ## 99       134        GENE      HOPKINS
    ## 100      113      MORGAN      HOPKINS
    ## 101       50     NATALIE      HOPKINS
    ## 102      132        ADAM       HOPPER
    ## 103      170        MENA       HOPPER
    ## 104       65      ANGELA       HUDSON
    ## 105       52      CARMEN         HUNT
    ## 106      140      WHOOPI         HURT
    ## 107      131        JANE      JACKMAN
    ## 108      119      WARREN      JACKMAN
    ## 109      146      ALBERT    JOHANSSON
    ## 110        8     MATTHEW    JOHANSSON
    ## 111       64         RAY    JOHANSSON
    ## 112       82       WOODY        JOLIE
    ## 113       43        KIRK     JOVOVICH
    ## 114      130       GRETA       KEITEL
    ## 115      198        MARY       KEITEL
    ## 116       74       MILLA       KEITEL
    ## 117       55         FAY       KILMER
    ## 118      153      MINNIE       KILMER
    ## 119      162       OPRAH       KILMER
    ## 120       45       REESE       KILMER
    ## 121       23      SANDRA       KILMER
    ## 122      103     MATTHEW        LEIGH
    ## 123        5      JOHNNY LOLLOBRIGIDA
    ## 124      157       GRETA       MALDEN
    ## 125      136          ED    MANSFIELD
    ## 126       22       ELVIS         MARX
    ## 127       77        CARY  MCCONAUGHEY
    ## 128       70    MICHELLE  MCCONAUGHEY
    ## 129      114      MORGAN    MCDORMAND
    ## 130      177        GENE     MCKELLEN
    ## 131       38         TOM     MCKELLEN
    ## 132      128        CATE      MCQUEEN
    ## 133       27       JULIA      MCQUEEN
    ## 134       42         TOM      MIRANDA
    ## 135      178        LISA       MONROE
    ## 136      120    PENELOPE       MONROE
    ## 137        7       GRACE       MOSTEL
    ## 138       99         JIM       MOSTEL
    ## 139       61   CHRISTIAN       NEESON
    ## 140       62       JAYNE       NEESON
    ## 141        6       BETTE    NICHOLSON
    ## 142      125      ALBERT        NOLTE
    ## 143      150       JAYNE        NOLTE
    ## 144      122       SALMA        NOLTE
    ## 145      108      WARREN        NOLTE
    ## 146       34      AUDREY      OLIVIER
    ## 147       15        CUBA      OLIVIER
    ## 148       69     KENNETH      PALTROW
    ## 149       21     KIRSTEN      PALTROW
    ## 150       33       MILLA         PECK
    ## 151       30      SANDRA         PECK
    ## 152       87     SPENCER         PECK
    ## 153       73        GARY         PENN
    ## 154      133     RICHARD         PENN
    ## 155       88     KENNETH        PESCI
    ## 156      171     OLYMPIA     PFEIFFER
    ## 157       51        GARY      PHOENIX
    ## 158       54    PENELOPE      PINKETT
    ## 159       84       JAMES         PITT
    ## 160       75        BURT        POSEY
    ## 161       93       ELLEN      PRESLEY
    ## 162      135        RITA     REYNOLDS
    ## 163      142        JADA        RYDER
    ## 164      195       JAYNE  SILVERSTONE
    ## 165      180        JEFF  SILVERSTONE
    ## 166       78     GROUCHO      SINATRA
    ## 167       31       SISSY     SOBIESKI
    ## 168       44        NICK     STALLONE
    ## 169       24     CAMERON       STREEP
    ## 170      116         DAN       STREEP
    ## 171      192        JOHN       SUVARI
    ## 172        9         JOE        SWANK
    ## 173      155         IAN        TANDY
    ## 174       66        MARY        TANDY
    ## 175       59      DUSTIN       TAUTOU
    ## 176      193        BURT       TEMPLE
    ## 177       53        MENA       TEMPLE
    ## 178      149     RUSSELL       TEMPLE
    ## 179      200       THORA       TEMPLE
    ## 180      126     FRANCES        TOMEI
    ## 181       18         DAN         TORN
    ## 182       94     KENNETH         TORN
    ## 183      102      WALTER         TORN
    ## 184       20     LUCILLE        TRACY
    ## 185      117       RENEE        TRACY
    ## 186       17       HELEN       VOIGHT
    ## 187       95       DARYL     WAHLBERG
    ## 188        2        NICK     WAHLBERG
    ## 189      196        BELA       WALKEN
    ## 190       29        ALEC        WAYNE
    ## 191      163 CHRISTOPHER         WEST
    ## 192      197       REESE         WEST
    ## 193      172     GROUCHO     WILLIAMS
    ## 194      137      MORGAN     WILLIAMS
    ## 195       72        SEAN     WILLIAMS
    ## 196       83         BEN       WILLIS
    ## 197       96        GENE       WILLIS
    ## 198      164    HUMPHREY       WILLIS
    ## 199      168        WILL       WILSON
    ## 200      147         FAY      WINSLET
    ## 201       68         RIP      WINSLET
    ## 202      144      ANGELA  WITHERSPOON
    ## 203      156         FAY         WOOD
    ## 204       13         UMA         WOOD
    ## 205       63     CAMERON         WRAY
    ## 206      111     CAMERON    ZELLWEGER
    ## 207      186       JULIA    ZELLWEGER
    ## 208       85      MINNIE    ZELLWEGER

Try in SQL directly:

``` sql
 SELECT  actor_id, first_name, last_name
           FROM    actor
           ORDER by last_name, first_name
```

| actor_id | first_name | last_name |
|---------:|:-----------|:----------|
|       58 | CHRISTIAN  | AKROYD    |
|      182 | DEBBIE     | AKROYD    |
|       92 | KIRSTEN    | AKROYD    |
|      118 | CUBA       | ALLEN     |
|      145 | KIM        | ALLEN     |
|      194 | MERYL      | ALLEN     |
|       76 | ANGELINA   | ASTAIRE   |
|      112 | RUSSELL    | BACALL    |
|      190 | AUDREY     | BAILEY    |
|       67 | JESSICA    | BAILEY    |

Displaying records 1 - 10

\#Exercise 2 Retrieve the actor ID, first name, and last name for actors
whose last name equals ‘WILLIAMS’ or ‘DAVIS’.

``` r
dbGetQuery(con, "
SELECT actor_id, first_name, last_name
FROM actor
WHERE last_name IN ('WILLIAMS', 'DAVIS')
ORDER BY last_name
")
```

    ##   actor_id first_name last_name
    ## 1        4   JENNIFER     DAVIS
    ## 2      101      SUSAN     DAVIS
    ## 3      110      SUSAN     DAVIS
    ## 4       72       SEAN  WILLIAMS
    ## 5      137     MORGAN  WILLIAMS
    ## 6      172    GROUCHO  WILLIAMS

\##Exercise 3 Write a query against the rental table that returns the
IDs of the customers who rented a film on July 5, 2005 (use the
rental.rental_date column, and you can use the date() function to ignore
the time component). Include a single row for each distinct customer ID.

``` r
dbGetQuery(con, "
SELECT DISTINCT customer_id, rental_date
FROM  rental
WHERE date(rental_date) = '2005-07-05'
")
```

    ##    customer_id         rental_date
    ## 1          565 2005-07-05 22:49:24
    ## 2          242 2005-07-05 22:51:44
    ## 3           37 2005-07-05 22:56:33
    ## 4           60 2005-07-05 22:57:34
    ## 5          594 2005-07-05 22:59:53
    ## 6            8 2005-07-05 23:01:21
    ## 7          490 2005-07-05 23:02:37
    ## 8          476 2005-07-05 23:05:17
    ## 9          322 2005-07-05 23:05:44
    ## 10         298 2005-07-05 23:08:53
    ## 11         382 2005-07-05 23:11:43
    ## 12         138 2005-07-05 23:13:07
    ## 13         520 2005-07-05 23:13:22
    ## 14         536 2005-07-05 23:13:51
    ## 15         114 2005-07-05 23:23:11
    ## 16         111 2005-07-05 23:25:54
    ## 17         296 2005-07-05 23:29:55
    ## 18         586 2005-07-05 23:30:36
    ## 19         349 2005-07-05 23:32:49
    ## 20         397 2005-07-05 23:33:40
    ## 21         369 2005-07-05 23:37:13
    ## 22         421 2005-07-05 23:41:08
    ## 23         142 2005-07-05 23:44:37
    ## 24         169 2005-07-05 23:46:19
    ## 25         348 2005-07-05 23:47:30
    ## 26         553 2005-07-05 23:50:04
    ## 27         295 2005-07-05 23:59:15

``` r
dbGetQuery(con, "
SELECT customer_id,
  COUNT(*) as N
FROM rental
WHERE date(rental_date) = '2005-07-05'
GROUP BY customer_id
")
```

    ##    customer_id N
    ## 1            8 1
    ## 2           37 1
    ## 3           60 1
    ## 4          111 1
    ## 5          114 1
    ## 6          138 1
    ## 7          142 1
    ## 8          169 1
    ## 9          242 1
    ## 10         295 1
    ## 11         296 1
    ## 12         298 1
    ## 13         322 1
    ## 14         348 1
    ## 15         349 1
    ## 16         369 1
    ## 17         382 1
    ## 18         397 1
    ## 19         421 1
    ## 20         476 1
    ## 21         490 1
    ## 22         520 1
    ## 23         536 1
    ## 24         553 1
    ## 25         565 1
    ## 26         586 1
    ## 27         594 1

\#Exercise 4 Construct a query that retries all rows from the payment
table where the amount is either 1.99, 7.99, 9.99.

``` r
dbGetQuery(con,"
SELECT *
FROM payment
WHERE amount IN (1.99, 7.99, 9.99)  
LIMIT 10
           ")
```

    ##    payment_id customer_id staff_id rental_id amount               payment_date
    ## 1       16050         269        2         7   1.99 2007-01-24 21:40:19.996577
    ## 2       16056         270        1       193   1.99 2007-01-26 05:10:14.996577
    ## 3       16081         282        2        48   1.99 2007-01-25 04:49:12.996577
    ## 4       16103         294        1       595   1.99 2007-01-28 12:28:20.996577
    ## 5       16133         307        1       614   1.99 2007-01-28 14:01:54.996577
    ## 6       16158         316        1      1065   1.99 2007-01-31 07:23:22.996577
    ## 7       16160         318        1       224   9.99 2007-01-26 08:46:53.996577
    ## 8       16161         319        1        15   9.99 2007-01-24 23:07:48.996577
    ## 9       16180         330        2       967   7.99 2007-01-30 17:40:32.996577
    ## 10      16206         351        1      1137   1.99 2007-01-31 17:48:40.996577

Construct a query that retrieve all rows from the payment table where
the amount is greater then 5

``` r
dbGetQuery(con,"
SELECT *
FROM payment
WHERE amount > 5
LIMIT 10
")
```

    ##    payment_id customer_id staff_id rental_id amount               payment_date
    ## 1       16052         269        2       678   6.99 2007-01-28 21:44:14.996577
    ## 2       16058         271        1      1096   8.99 2007-01-31 11:59:15.996577
    ## 3       16060         272        1       405   6.99 2007-01-27 12:01:05.996577
    ## 4       16061         272        1      1041   6.99 2007-01-31 04:14:49.996577
    ## 5       16068         274        1       394   5.99 2007-01-27 09:54:37.996577
    ## 6       16073         276        1       860  10.99 2007-01-30 01:13:42.996577
    ## 7       16074         277        2       308   6.99 2007-01-26 20:30:05.996577
    ## 8       16082         282        2       282   6.99 2007-01-26 17:24:52.996577
    ## 9       16086         284        1      1145   6.99 2007-01-31 18:42:11.996577
    ## 10      16087         286        2        81   6.99 2007-01-25 10:43:45.996577

Construct a query that retrives all rows from the payment table where
the amount is greater then 5 and less then 8

``` r
dbGetQuery(con,"
SELECT *
FROM payment
WHERE amount > 5 AND amount<8
LIMIT 10
")
```

    ##    payment_id customer_id staff_id rental_id amount               payment_date
    ## 1       16052         269        2       678   6.99 2007-01-28 21:44:14.996577
    ## 2       16060         272        1       405   6.99 2007-01-27 12:01:05.996577
    ## 3       16061         272        1      1041   6.99 2007-01-31 04:14:49.996577
    ## 4       16068         274        1       394   5.99 2007-01-27 09:54:37.996577
    ## 5       16074         277        2       308   6.99 2007-01-26 20:30:05.996577
    ## 6       16082         282        2       282   6.99 2007-01-26 17:24:52.996577
    ## 7       16086         284        1      1145   6.99 2007-01-31 18:42:11.996577
    ## 8       16087         286        2        81   6.99 2007-01-25 10:43:45.996577
    ## 9       16092         288        2       427   6.99 2007-01-27 14:38:30.996577
    ## 10      16094         288        2       565   5.99 2007-01-28 07:54:57.996577

\#Exercise 5 Retrieve all the payment IDs and their amount from the
customers whose last name is ‘DAVIS’.

``` sql
PRAGMA table_info(customer)
```

| cid | name        | type    | notnull | dflt_value |  pk |
|:----|:------------|:--------|--------:|:-----------|----:|
| 0   | customer_id | INTEGER |       0 | NA         |   0 |
| 1   | store_id    | INTEGER |       0 | NA         |   0 |
| 2   | first_name  | TEXT    |       0 | NA         |   0 |
| 3   | last_name   | TEXT    |       0 | NA         |   0 |
| 4   | email       | TEXT    |       0 | NA         |   0 |
| 5   | address_id  | INTEGER |       0 | NA         |   0 |
| 6   | activebool  | TEXT    |       0 | NA         |   0 |
| 7   | create_date | TEXT    |       0 | NA         |   0 |
| 8   | last_update | TEXT    |       0 | NA         |   0 |
| 9   | active      | INTEGER |       0 | NA         |   0 |

Displaying records 1 - 10

``` r
dbGetQuery(con,"
SELECT c.customer_id, p.payment_id, p.amount, c.last_name
FROM customer AS c INNER JOIN payment AS p
ON c.customer_id = p.customer_id
WHERE c.last_name =='DAVIS'
/* This is a comment*/
")
```

    ##   customer_id payment_id amount last_name
    ## 1           6      16685   4.99     DAVIS
    ## 2           6      16686   2.99     DAVIS
    ## 3           6      16687   0.99     DAVIS

\#Exercise 6

Use COUNT(\*) to count the number of rows in rental

``` r
dbGetQuery(con,"
SELECT COUNT(*) AS count
From rental
")
```

    ##   count
    ## 1 16044

Use COUNT(\*) and GROUP BY to count the number of rentals for each
customer_id

``` r
dbGetQuery(con,"
SELECT customer_id, COUNT(*) AS count
From rental
GROUP BY customer_id
LIMIT 8
")
```

    ##   customer_id count
    ## 1           1    32
    ## 2           2    27
    ## 3           3    26
    ## 4           4    22
    ## 5           5    38
    ## 6           6    28
    ## 7           7    33
    ## 8           8    24

Repeat the previous query and sort by the count in descending order

``` r
dbGetQuery(con,"
SELECT customer_id, COUNT(*) AS count
From rental
GROUP BY customer_id
ORDER BY count DESC
LIMIT 8
")
```

    ##   customer_id count
    ## 1         148    46
    ## 2         526    45
    ## 3         236    42
    ## 4         144    42
    ## 5          75    41
    ## 6         469    40
    ## 7         197    40
    ## 8         468    39

Repeat the previous query but use HAVING to only keep the groups with 40
or more.

``` r
dbGetQuery(con,"
SELECT customer_id, COUNT(*) AS count
From rental
GROUP BY customer_id
HAVING count > 40
ORDER BY count DESC
LIMIT 8
")
```

    ##   customer_id count
    ## 1         148    46
    ## 2         526    45
    ## 3         236    42
    ## 4         144    42
    ## 5          75    41

\#Exercise 7 The following query calculates a number of summary
statistics for the payment table using MAX, MIN, AVG and SUM Modify the
above query to do those calculations for each customer_id

``` r
dbGetQuery(con,"
SELECT customer_id,
       MAX(amount) AS maxpayment,
       MIN(amount) AS minpayment,
       AVG(amount) AS avgpayment,
       SUM(amount) AS sunpayment
FROM payment
GROUP BY customer_id
LIMIT 5
")
```

    ##   customer_id maxpayment minpayment avgpayment sunpayment
    ## 1           1       2.99       0.99   1.990000       3.98
    ## 2           2       4.99       4.99   4.990000       4.99
    ## 3           3       2.99       1.99   2.490000       4.98
    ## 4           5       6.99       0.99   3.323333       9.97
    ## 5           6       4.99       0.99   2.990000       8.97

Modify the above query to only keep the customer_ids that have more then
5 payments

``` r
dbGetQuery(con,"
SELECT COUNT(*) AS N,
       customer_id,
       MAX(amount) AS maxpayment,
       MIN(amount) AS minpayment,
       AVG(amount) AS avgpayment,
       SUM(amount) AS sunpayment
FROM payment
GROUP BY customer_id
HAVING N>5
")
```

    ##    N customer_id maxpayment minpayment avgpayment sunpayment
    ## 1  6          19       9.99       0.99   4.490000      26.94
    ## 2  6          53       9.99       0.99   4.490000      26.94
    ## 3  7         109       7.99       0.99   3.990000      27.93
    ## 4  6         161       5.99       0.99   2.990000      17.94
    ## 5  8         197       3.99       0.99   2.615000      20.92
    ## 6  6         207       6.99       0.99   2.990000      17.94
    ## 7  6         239       7.99       2.99   5.656667      33.94
    ## 8  6         245       8.99       0.99   4.823333      28.94
    ## 9  6         251       4.99       1.99   3.323333      19.94
    ## 10 6         269       6.99       0.99   3.156667      18.94
    ## 11 6         274       5.99       2.99   4.156667      24.94
    ## 12 6         371       6.99       0.99   4.323333      25.94
    ## 13 7         506       8.99       0.99   4.132857      28.93
    ## 14 6         596       6.99       0.99   3.823333      22.94

\#clean up

``` r
dbDisconnect(con)
```
