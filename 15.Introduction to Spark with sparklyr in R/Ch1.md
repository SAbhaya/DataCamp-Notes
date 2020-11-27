# Introduction to Spark with sparklyr in R
# Chapter 1 - Light My Fire: Starting To Use Spark With dplyr Syntax

## The connect-work-disconnect pattern

```r

# Load sparklyr
library(sparklyr)

# Connect to your Spark cluster
spark_conn <- spark_connect(master = "local")

# Print the version of Spark
spark_version(spark_conn)

# Disconnect from Spark
spark_disconnect(spark_conn)

```

Output:

```bash
# Load sparklyr
library(sparklyr)
# Connect to your Spark cluster
spark_conn <- spark_connect(master = "local")
# Print the version of Spark
spark_version(spark_conn)
[1] '2.1.0'
# Disconnect from Spark
spark_disconnect(spark_conn)

```
***

## Copying data into Spark

```r

# Load dplyr
library(dplyr)

# Explore track_metadata structure
str(track_metadata)

# Connect to your Spark cluster
spark_conn <- spark_connect("local")

# Copy track_metadata to Spark
track_metadata_tbl <- copy_to(spark_conn, track_metadata)

# List the data frames available in Spark
src_tbls(spark_conn)

# Disconnect from Spark
spark_disconnect(spark_conn)

```

Output:

```bash
# Load dplyr
library(dplyr)
# Explore track_metadata structure
str(track_metadata)
Classes 'tbl_df', 'tbl' and 'data.frame':	1000 obs. of  11 variables:
 $ track_id          : chr  "TRDVOZX128F93283A3" "TRDPMEU12903CC5434" "TRJQDNJ128F426E8CE" "TRRRGCS128F4280BB6" ...
 $ title             : chr  "Jersey Belle Blues" "Get Yourself Together" "Jersey Bull Blues" "High Fever Blues" ...
 $ song_id           : chr  "SOKMEHX12AB0180877" "SOFCPUM12AB018A4EB" "SOKDFLC12A8C1354AE" "SONQUOG12A8C13C76F" ...
 $ release           : chr  "Backwater Blues" "Rambler's Blues" "Complete Recordings_ CD E" "The Panama Limited" ...
 $ artist_id         : chr  "ARDNQ0R1187B9BA1EF" "ARDNQ0R1187B9BA1EF" "ARTDUXM1187B9899ED" "ARNEL2O1187FB4421A" ...
 $ artist_mbid       : chr  "dbfd61ef-fce1-4803-9f18-7bfdd3996508" "dbfd61ef-fce1-4803-9f18-7bfdd3996508" "c71b4f57-29da-4bf2-bccb-9dc81cd2d905" "882af819-887e-4691-a4af-b14613058942" ...
 $ artist_name       : chr  "Lonnie Johnson" "Lonnie Johnson" "Charley Patton" "Bukka White" ...
 $ duration          : num  178 190 193 174 192 ...
 $ artist_familiarity: num  0.559 0.559 0.574 0.572 0.638 ...
 $ artist_hotttnesss : num  0.405 0.405 0.376 0.424 0.405 ...
 $ year              : int  1940 1940 1934 1940 1931 1939 1936 1940 1935 1940 ...
# Connect to your Spark cluster
spark_conn <- spark_connect("local")
# Copy track_metadata to Spark
track_metadata_tbl <- copy_to(spark_conn, track_metadata)
# List the data frames available in Spark
src_tbls(spark_conn)
[1] "track_metadata"
# Disconnect from Spark
spark_disconnect(spark_conn)


```
***

## Big data, tiny tibble

```r
# Link to the track_metadata table in Spark
track_metadata_tbl <- tbl(spark_conn, "track_metadata")

# See how big the dataset is
dim(track_metadata_tbl)

# See how small the tibble is
object_size(track_metadata_tbl)

```

Output:

```bash

# Link to the track_metadata table in Spark
track_metadata_tbl <- tbl(spark_conn, "track_metadata")
# See how big the dataset is
dim(track_metadata_tbl)
[1] 1000   11
# See how small the tibble is
object_size(track_metadata_tbl)
10.1 kB

```

***

## Exploring the structure of tibbles

```r

# Print 5 rows, all columns
print(track_metadata_tbl, n=5, width = Inf)

# Examine structure of tibble
str(track_metadata_tbl)

# Examine structure of data
glimpse(track_metadata_tbl)

```

Output:

```bash

# Print 5 rows, all columns
print(track_metadata_tbl, n=5, width = Inf)
# Source:   table<track_metadata> [?? x 11]
# Database: spark_connection
  track_id           title                 song_id           
  <chr>              <chr>                 <chr>             
1 TRDVOZX128F93283A3 Jersey Belle Blues    SOKMEHX12AB0180877
2 TRDPMEU12903CC5434 Get Yourself Together SOFCPUM12AB018A4EB
3 TRJQDNJ128F426E8CE Jersey Bull Blues     SOKDFLC12A8C1354AE
4 TRRRGCS128F4280BB6 High Fever Blues      SONQUOG12A8C13C76F
5 TRISTAT12903CFCBA1 Safety Mama           SODFTCC12AC95F0214
  release                   artist_id         
  <chr>                     <chr>             
1 Backwater Blues           ARDNQ0R1187B9BA1EF
2 Rambler's Blues           ARDNQ0R1187B9BA1EF
3 Complete Recordings_ CD E ARTDUXM1187B9899ED
4 The Panama Limited        ARNEL2O1187FB4421A
5 The Blues Collection      ARYYCVC1187B9A8510
  artist_mbid                          artist_name    duration
  <chr>                                <chr>             <dbl>
1 dbfd61ef-fce1-4803-9f18-7bfdd3996508 Lonnie Johnson     178.
2 dbfd61ef-fce1-4803-9f18-7bfdd3996508 Lonnie Johnson     190.
3 c71b4f57-29da-4bf2-bccb-9dc81cd2d905 Charley Patton     193.
4 882af819-887e-4691-a4af-b14613058942 Bukka White        174.
5 ffa28768-ecda-42c6-ac49-6ce5c7d33043 Bessie Smith       192.
  artist_familiarity artist_hotttnesss  year
               <dbl>             <dbl> <int>
1              0.559             0.405  1940
2              0.559             0.405  1940
3              0.574             0.376  1934
4              0.572             0.424  1940
5              0.638             0.405  1931
# ... with 995 more rows
# Examine structure of tibble
str(track_metadata_tbl)
List of 2
 $ src:List of 1
  ..$ con:List of 11
  .. ..$ master       : chr "local[8]"
  .. ..$ method       : chr "shell"
  .. ..$ app_name     : chr "sparklyr"
  .. ..$ config       :List of 5
  .. .. ..$ sparklyr.cores.local              : int 8
  .. .. ..$ spark.sql.shuffle.partitions.local: int 8
  .. .. ..$ spark.env.SPARK_LOCAL_IP.local    : chr "127.0.0.1"
  .. .. ..$ sparklyr.csv.embedded             : chr "^1.*"
  .. .. ..$ sparklyr.shell.driver-class-path  : chr ""
  .. .. ..- attr(*, "config")= chr "default"
  .. .. ..- attr(*, "file")= chr "/usr/local/lib/R/site-library/sparklyr/conf/config-template.yml"
  .. ..$ spark_home   : chr "/home/repl/.cache/spark/spark-2.1.0-bin-hadoop2.7"
  .. ..$ backend      :Classes 'sockconn', 'connection'  atomic [1:1] 4
  .. .. .. ..- attr(*, "conn_id")=<externalptr> 
  .. ..$ monitor      :Classes 'sockconn', 'connection'  atomic [1:1] 3
  .. .. .. ..- attr(*, "conn_id")=<externalptr> 
  .. ..$ output_file  : chr "/tmp/Rtmpp30766/file13f83b07a_spark.log"
  .. ..$ spark_context:Classes 'spark_jobj', 'shell_jobj' <environment: 0x4722da0> 
  .. ..$ java_context :Classes 'spark_jobj', 'shell_jobj' <environment: 0x47583f0> 
  .. ..$ hive_context :Classes 'spark_jobj', 'shell_jobj' <environment: 0x47dad88> 
  .. ..- attr(*, "class")= chr [1:3] "spark_connection" "spark_shell_connection" "DBIConnection"
  ..- attr(*, "class")= chr [1:3] "src_spark" "src_sql" "src"
 $ ops:List of 2
  ..$ x   :Classes 'ident', 'character'  chr "track_metadata"
  ..$ vars: chr [1:11] "track_id" "title" "song_id" "release" ...
  ..- attr(*, "class")= chr [1:3] "op_base_remote" "op_base" "op"
 - attr(*, "class")= chr [1:4] "tbl_spark" "tbl_sql" "tbl_lazy" "tbl"
# Examine structure of data
glimpse(track_metadata_tbl)
Observations: 25
Variables: 11
$ track_id           <chr> "TRDVOZX128F93283A3", "TRDPMEU12903CC5434", "TRJ...
$ title              <chr> "Jersey Belle Blues", "Get Yourself Together", "...
$ song_id            <chr> "SOKMEHX12AB0180877", "SOFCPUM12AB018A4EB", "SOK...
$ release            <chr> "Backwater Blues", "Rambler's Blues", "Complete ...
$ artist_id          <chr> "ARDNQ0R1187B9BA1EF", "ARDNQ0R1187B9BA1EF", "ART...
$ artist_mbid        <chr> "dbfd61ef-fce1-4803-9f18-7bfdd3996508", "dbfd61e...
$ artist_name        <chr> "Lonnie Johnson", "Lonnie Johnson", "Charley Pat...
$ duration           <dbl> 177.9195, 190.4061, 192.6265, 174.1057, 191.7383...
$ artist_familiarity <dbl> 0.5589263, 0.5589322, 0.5743001, 0.5724709, 0.63...
$ artist_hotttnesss  <dbl> 0.4048271, 0.4048271, 0.3755936, 0.4244876, 0.40...
$ year               <int> 1940, 1940, 1934, 1940, 1931, 1939, 1936, 1940, ...


```

***

## Selecting columns

```r

# track_metadata_tbl has been pre-defined
track_metadata_tbl

# Manipulate the track metadata
track_metadata_tbl %>%
  # Select columns
  select(artist_name, release, title, year)

# Try to select columns using [ ]
tryCatch({
    # Selection code here
    track_metadata_tbl[, c("artist_name", "release", "title","year")]
  },
  error = print
)

```

Output:

```bash

# track_metadata_tbl has been pre-defined
track_metadata_tbl
# Source:   table<track_metadata> [?? x 11]
# Database: spark_connection
   track_id title song_id release artist_id artist_mbid artist_name duration
   <chr>    <chr> <chr>   <chr>   <chr>     <chr>       <chr>          <dbl>
 1 TRDVOZX~ Jers~ SOKMEH~ Backwa~ ARDNQ0R1~ dbfd61ef-f~ Lonnie Joh~     178.
 2 TRDPMEU~ Get ~ SOFCPU~ Ramble~ ARDNQ0R1~ dbfd61ef-f~ Lonnie Joh~     190.
 3 TRJQDNJ~ Jers~ SOKDFL~ Comple~ ARTDUXM1~ c71b4f57-2~ Charley Pa~     193.
 4 TRRRGCS~ High~ SONQUO~ The Pa~ ARNEL2O1~ 882af819-8~ Bukka White     174.
 5 TRISTAT~ Safe~ SODFTC~ The Bl~ ARYYCVC1~ ffa28768-e~ Bessie Smi~     192.
 6 TRAFHZD~ Stra~ SOTZTG~ Jukebo~ ARJGQDS1~ d59c4cda-1~ Billie Hol~     177.
 7 TRAMWSA~ Old ~ SOSBXR~ 25 Cou~ AR9HABI1~ aff932c2-e~ Red Foley       193.
 8 TRHRKYP~ Aber~ SONNDZ~ The Pa~ ARNEL2O1~ 882af819-8~ Bukka White     160.
 9 TRYMDFV~ 49 H~ SOMKVQ~ Big Jo~ ARFNKDR1~ bf295ac0-2~ Big Joe Wi~     168.
10 TREAPSO~ Parc~ SOWZXJ~ Fixin'~ ARNEL2O1~ 882af819-8~ Bukka White     160.
# ... with 990 more rows, and 3 more variables: artist_familiarity <dbl>,
#   artist_hotttnesss <dbl>, year <int>
# Manipulate the track metadata
track_metadata_tbl %>%
  # Select columns
  select(artist_name, release, title, year)
# Source:   lazy query [?? x 4]
# Database: spark_connection
   artist_name      release                        title                   year
   <chr>            <chr>                          <chr>                  <int>
 1 Lonnie Johnson   Backwater Blues                Jersey Belle Blues      1940
 2 Lonnie Johnson   Rambler's Blues                Get Yourself Together   1940
 3 Charley Patton   Complete Recordings_ CD E      Jersey Bull Blues       1934
 4 Bukka White      The Panama Limited             High Fever Blues        1940
 5 Bessie Smith     The Blues Collection           Safety Mama             1931
 6 Billie Holiday   Jukebox Hits 1935-1946         Strange Fruit           1939
 7 Red Foley        25 Country Memories            Old Shep                1936
 8 Bukka White      The Panama Limited             Aberdeen Mississippi ~  1940
 9 Big Joe Williams Big Joe Williams Vol. 1 1935 ~ 49 Highway Blues        1935
10 Bukka White      Fixin' to Die                  Parchman Farm Blues     1940
# ... with 990 more rows
# Try to select columns using [ ]
tryCatch({
    # Selection code here
    track_metadata_tbl[, c("artist_name", "release", "title","year")]
  },
  error = print
)
<simpleError in track_metadata_tbl[, c("artist_name", "release", "title", "year")]: incorrect number of dimensions>
>


```
***

## Filtering rows

```r

# track_metadata_tbl has been pre-defined
glimpse(track_metadata_tbl)

# Manipulate the track metadata
track_metadata_tbl %>%
  # Select columns
  select(artist_name, release, title, year) %>%
  # Filter rows
  filter(year >= 1960, year < 1970)

```


Output:

```bash

# track_metadata_tbl has been pre-defined
glimpse(track_metadata_tbl)
Observations: 25
Variables: 11
$ track_id           <chr> "TRDVOZX128F93283A3", "TRDPMEU12903CC5434", "TRJ...
$ title              <chr> "Jersey Belle Blues", "Get Yourself Together", "...
$ song_id            <chr> "SOKMEHX12AB0180877", "SOFCPUM12AB018A4EB", "SOK...
$ release            <chr> "Backwater Blues", "Rambler's Blues", "Complete ...
$ artist_id          <chr> "ARDNQ0R1187B9BA1EF", "ARDNQ0R1187B9BA1EF", "ART...
$ artist_mbid        <chr> "dbfd61ef-fce1-4803-9f18-7bfdd3996508", "dbfd61e...
$ artist_name        <chr> "Lonnie Johnson", "Lonnie Johnson", "Charley Pat...
$ duration           <dbl> 177.9195, 190.4061, 192.6265, 174.1057, 191.7383...
$ artist_familiarity <dbl> 0.5589263, 0.5589322, 0.5743001, 0.5724709, 0.63...
$ artist_hotttnesss  <dbl> 0.4048271, 0.4048271, 0.3755936, 0.4244876, 0.40...
$ year               <int> 1940, 1940, 1934, 1940, 1931, 1939, 1936, 1940, ...
# Manipulate the track metadata
track_metadata_tbl %>%
  # Select columns
  select(artist_name, release, title, year) %>%
  # Filter rows
  filter(year >= 1960, year < 1970)
# Source:   lazy query [?? x 4]
# Database: spark_connection
   artist_name            release                       title              year
   <chr>                  <chr>                         <chr>             <int>
 1 Shirelles              Dedicated To The One I Love   Dedicated To The~  1960
 2 Cliff Richard And The~ The Hits In Between           I Love You         1960
 3 Jackson Do Pandeiro    Festa Nordestina              Sebastiana         1960
 4 Maurice Williams & Th~ Soul Train Part 2             Stay               1960
 5 Julie London           Julie . . . At Home/Around M~ Everything Happe~  1960
 6 Link Wray & The Wraym~ Link Wray: Slinky! The Epic ~ Radar              1960
 7 Bo Diddley             Roadrunner The Chess Masters~ Let Me In          1960
 8 Lenny Bruce            The Lenny Bruce Originals_ V~ The Tribunal       1960
 9 Paul Anka              Sing Sing Sing                Lonely Boy         1960
10 Elvis Presley          The King                      Such A Night       1960
# ... with 108 more rows

```

## Arranging rows

```r
# track_metadata_tbl has been pre-defined
track_metadata_tbl

# Manipulate the track metadata
track_metadata_tbl %>%
  # Select columns
  select(artist_name,release,title,year ) %>%
  # Filter rows
  filter(year >= 1960, year < 1970) %>%
  # Arrange rows
  arrange(artist_name, desc(year), title)


```

Output:

```bash

# track_metadata_tbl has been pre-defined
track_metadata_tbl
# Source:   table<track_metadata> [?? x 11]
# Database: spark_connection
   track_id title song_id release artist_id artist_mbid artist_name duration
   <chr>    <chr> <chr>   <chr>   <chr>     <chr>       <chr>          <dbl>
 1 TRDVOZX~ Jers~ SOKMEH~ Backwa~ ARDNQ0R1~ dbfd61ef-f~ Lonnie Joh~     178.
 2 TRDPMEU~ Get ~ SOFCPU~ Ramble~ ARDNQ0R1~ dbfd61ef-f~ Lonnie Joh~     190.
 3 TRJQDNJ~ Jers~ SOKDFL~ Comple~ ARTDUXM1~ c71b4f57-2~ Charley Pa~     193.
 4 TRRRGCS~ High~ SONQUO~ The Pa~ ARNEL2O1~ 882af819-8~ Bukka White     174.
 5 TRISTAT~ Safe~ SODFTC~ The Bl~ ARYYCVC1~ ffa28768-e~ Bessie Smi~     192.
 6 TRAFHZD~ Stra~ SOTZTG~ Jukebo~ ARJGQDS1~ d59c4cda-1~ Billie Hol~     177.
 7 TRAMWSA~ Old ~ SOSBXR~ 25 Cou~ AR9HABI1~ aff932c2-e~ Red Foley       193.
 8 TRHRKYP~ Aber~ SONNDZ~ The Pa~ ARNEL2O1~ 882af819-8~ Bukka White     160.
 9 TRYMDFV~ 49 H~ SOMKVQ~ Big Jo~ ARFNKDR1~ bf295ac0-2~ Big Joe Wi~     168.
10 TREAPSO~ Parc~ SOWZXJ~ Fixin'~ ARNEL2O1~ 882af819-8~ Bukka White     160.
# ... with 990 more rows, and 3 more variables: artist_familiarity <dbl>,
#   artist_hotttnesss <dbl>, year <int>
# Manipulate the track metadata
track_metadata_tbl %>%
  # Select columns
  select(artist_name,release,title,year ) %>%
  # Filter rows
  filter(year >= 1960, year < 1970) %>%
  # Arrange rows
  arrange(artist_name, desc(year), title)
# Source:     lazy query [?? x 4]
# Database:   spark_connection
# Ordered by: artist_name, desc(year), title
   artist_name              release                   title                year
   <chr>                    <chr>                     <chr>               <int>
 1 Albert Ayler             Spirits Rejoice           D.C.                 1965
 2 Barbara Randolph         The Complete Introductio~ I Got A Feeling      1967
 3 Bill Monroe and His Blu~ All The Classic Releases~ Blue Grass Stomp     1961
 4 Blodwyn Pig              Ahead Rings Out           Slow Down (2006 Di~  1969
 5 Blood_ Sweat & Tears     Child Is Father To The M~ Refugee From Yuhup~  1968
 6 Bo Diddley               Roadrunner The Chess Mas~ Let Me In            1960
 7 Bob Dylan                Nashville Skyline/John W~ Country Pie          1969
 8 Bob Dylan                Blonde On Blonde          Obviously Five Bel~  1966
 9 Bob Dylan                Bob Dylan'S Greatest Hit~ Gates Of Eden        1965
10 Bobbie Gentry            Touch 'Em With Love       Touch 'Em With Love  1969
# ... with 108 more rows


```


## Mutating columns

```r
# track_metadata_tbl has been pre-defined
track_metadata_tbl

# Manipulate the track metadata
track_metadata_tbl %>%
  # Select columns
  select(title, duration) %>%
  # Mutate columns
  mutate(duration_minutes = duration/60)

```

Output:

```bash
# track_metadata_tbl has been pre-defined
track_metadata_tbl
# Source:   table<track_metadata> [?? x 11]
# Database: spark_connection
   track_id title song_id release artist_id artist_mbid artist_name duration
   <chr>    <chr> <chr>   <chr>   <chr>     <chr>       <chr>          <dbl>
 1 TRDVOZX~ Jers~ SOKMEH~ Backwa~ ARDNQ0R1~ dbfd61ef-f~ Lonnie Joh~     178.
 2 TRDPMEU~ Get ~ SOFCPU~ Ramble~ ARDNQ0R1~ dbfd61ef-f~ Lonnie Joh~     190.
 3 TRJQDNJ~ Jers~ SOKDFL~ Comple~ ARTDUXM1~ c71b4f57-2~ Charley Pa~     193.
 4 TRRRGCS~ High~ SONQUO~ The Pa~ ARNEL2O1~ 882af819-8~ Bukka White     174.
 5 TRISTAT~ Safe~ SODFTC~ The Bl~ ARYYCVC1~ ffa28768-e~ Bessie Smi~     192.
 6 TRAFHZD~ Stra~ SOTZTG~ Jukebo~ ARJGQDS1~ d59c4cda-1~ Billie Hol~     177.
 7 TRAMWSA~ Old ~ SOSBXR~ 25 Cou~ AR9HABI1~ aff932c2-e~ Red Foley       193.
 8 TRHRKYP~ Aber~ SONNDZ~ The Pa~ ARNEL2O1~ 882af819-8~ Bukka White     160.
 9 TRYMDFV~ 49 H~ SOMKVQ~ Big Jo~ ARFNKDR1~ bf295ac0-2~ Big Joe Wi~     168.
10 TREAPSO~ Parc~ SOWZXJ~ Fixin'~ ARNEL2O1~ 882af819-8~ Bukka White     160.
# ... with 990 more rows, and 3 more variables: artist_familiarity <dbl>,
#   artist_hotttnesss <dbl>, year <int>
# Manipulate the track metadata
track_metadata_tbl %>%
  # Select columns
  select(title, duration) %>%
  # Mutate columns
  mutate(duration_minutes = duration/60)
# Source:   lazy query [?? x 3]
# Database: spark_connection
   title                      duration duration_minutes
   <chr>                         <dbl>            <dbl>
 1 Jersey Belle Blues             178.             2.97
 2 Get Yourself Together          190.             3.17
 3 Jersey Bull Blues              193.             3.21
 4 High Fever Blues               174.             2.90
 5 Safety Mama                    192.             3.20
 6 Strange Fruit                  177.             2.94
 7 Old Shep                       193.             3.21
 8 Aberdeen Mississippi Blues     160.             2.67
 9 49 Highway Blues               168.             2.80
10 Parchman Farm Blues            160.             2.66
# ... with 990 more rows

```


***

## Summarizing columns

```r
# track_metadata_tbl has been pre-defined
track_metadata_tbl

# Manipulate the track metadata
track_metadata_tbl %>%
  # Select columns
  select(title, duration) %>%
  # Mutate columns
  mutate(duration_minutes = duration/60) %>%
  # Summarize columns
  summarize(mean_duration_minutes = mean(duration_minutes))


```

Output:

```bash
# track_metadata_tbl has been pre-defined
track_metadata_tbl
# Source:   table<track_metadata> [?? x 11]
# Database: spark_connection
   track_id title song_id release artist_id artist_mbid artist_name duration
   <chr>    <chr> <chr>   <chr>   <chr>     <chr>       <chr>          <dbl>
 1 TRDVOZX~ Jers~ SOKMEH~ Backwa~ ARDNQ0R1~ dbfd61ef-f~ Lonnie Joh~     178.
 2 TRDPMEU~ Get ~ SOFCPU~ Ramble~ ARDNQ0R1~ dbfd61ef-f~ Lonnie Joh~     190.
 3 TRJQDNJ~ Jers~ SOKDFL~ Comple~ ARTDUXM1~ c71b4f57-2~ Charley Pa~     193.
 4 TRRRGCS~ High~ SONQUO~ The Pa~ ARNEL2O1~ 882af819-8~ Bukka White     174.
 5 TRISTAT~ Safe~ SODFTC~ The Bl~ ARYYCVC1~ ffa28768-e~ Bessie Smi~     192.
 6 TRAFHZD~ Stra~ SOTZTG~ Jukebo~ ARJGQDS1~ d59c4cda-1~ Billie Hol~     177.
 7 TRAMWSA~ Old ~ SOSBXR~ 25 Cou~ AR9HABI1~ aff932c2-e~ Red Foley       193.
 8 TRHRKYP~ Aber~ SONNDZ~ The Pa~ ARNEL2O1~ 882af819-8~ Bukka White     160.
 9 TRYMDFV~ 49 H~ SOMKVQ~ Big Jo~ ARFNKDR1~ bf295ac0-2~ Big Joe Wi~     168.
10 TREAPSO~ Parc~ SOWZXJ~ Fixin'~ ARNEL2O1~ 882af819-8~ Bukka White     160.
# ... with 990 more rows, and 3 more variables: artist_familiarity <dbl>,
#   artist_hotttnesss <dbl>, year <int>
# Manipulate the track metadata
track_metadata_tbl %>%
  # Select columns
  select(title, duration) %>%
  # Mutate columns
  mutate(duration_minutes = duration/60) %>%
  # Summarize columns
  summarize(mean_duration_minutes = mean(duration_minutes))
# Source:   lazy query [?? x 1]
# Database: spark_connection
  mean_duration_minutes
                  <dbl>
1                  3.75

```

***

*End of Chapter 1*
