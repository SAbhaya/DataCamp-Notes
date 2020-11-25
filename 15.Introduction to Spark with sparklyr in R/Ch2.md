# Chapter 2 - Tools of the Trade: Advanced dplyr Usage

## Mother's little helper (1)

```r
# track_metadata_tbl has been pre-defined
track_metadata_tbl

track_metadata_tbl %>%
  # Select columns starting with artist
  select(starts_with("artist"))

track_metadata_tbl %>%
  # Select columns ending with id
  select(ends_with("id"))

```

Output:

```bash
# track_metadata_tbl has been pre-defined
track_metadata_tbl
# Source:   table<track_metadata> [?? x 11]
# Database: spark_connection
   track_id title song_id release artist_id artist_mbid artist_name duration
   <chr>    <chr> <chr>   <chr>   <chr>     <chr>       <chr>          <dbl>
 1 TRSTWXA~ The ~ SOCQMX~ Espana  ARNNU4G1~ 5c176092-c~ 101 Strings     221.
 2 TRQNQZX~ Soul~ SOWULX~ Diary ~ ARJ9DSA1~ 4756395c-5~ John Mayal~     368.
 3 TRTPVEB~ The ~ SOHKYI~ The Bl~ ARQ8CJ61~ 15ab8bb8-7~ Illinois J~     329.
 4 TRZZNFB~ The ~ SOJVHK~ Give M~ ARFAKTH1~ 0174d942-3~ Chairmen O~     193.
 5 TRZAAWN~ Litt~ SORYVB~ Beauti~ ARNMWP51~ 4dca4bb2-2~ Curtis May~     321.
 6 TRRAOAD~ Will~ SODTBY~ Jammys~ ARMDWND1~ 8ee00333-e~ Black Uhuru     179.
 7 TRPGBAO~ Chil~ SOCTEW~ Pure A~ ARBSLZ11~ f940c4dd-f~ Alex de Gr~     162.
 8 TRQLTYD~ Ring~ SONNCT~ Eat To~ ARM7YQQ1~ 4d2956d1-a~ Blondie         210.
 9 TRJNORS~ Chat~ SOGQHH~ Sid Si~ ARBK4PS1~ 637504e3-b~ Sid Vicious     111.
10 TRLSQUC~ Migh~ SOTFGI~ Rockin~ ARRHNLN1~ d86c3c8b-8~ Jelly Roll~     242.
# ... with 990 more rows, and 3 more variables: artist_familiarity <dbl>,
#   artist_hotttnesss <dbl>, year <int>
track_metadata_tbl %>%
  # Select columns starting with artist
  select(starts_with("artist"))
# Source:   lazy query [?? x 5]
# Database: spark_connection
   artist_id   artist_mbid     artist_name    artist_familiar~ artist_hotttnes~
   <chr>       <chr>           <chr>                     <dbl>            <dbl>
 1 ARNNU4G118~ 5c176092-cb4d-~ 101 Strings               0.467            0.369
 2 ARJ9DSA118~ 4756395c-57ed-~ John Mayall &~            0.627            0.405
 3 ARQ8CJ6118~ 15ab8bb8-7348-~ Illinois Jacq~            0.441            0.355
 4 ARFAKTH118~ 0174d942-39da-~ Chairmen Of T~            0.536            0.415
 5 ARNMWP5118~ 4dca4bb2-23ba-~ Curtis Mayfie~            0.787            0.495
 6 ARMDWND118~ 8ee00333-ec2c-~ Black Uhuru               0.707            0.463
 7 ARBSLZ1118~ f940c4dd-f687-~ Alex de Grassi            0.565            0.381
 8 ARM7YQQ118~ 4d2956d1-a3f7-~ Blondie                   0.759            0.551
 9 ARBK4PS118~ 637504e3-be95-~ Sid Vicious               0.621            0.455
10 ARRHNLN118~ d86c3c8b-84d5-~ Jelly Roll Ki~            0.468            0.303
# ... with 990 more rows
track_metadata_tbl %>%
  # Select columns ending with id
  select(ends_with("id"))
# Source:   lazy query [?? x 4]
# Database: spark_connection
   track_id        song_id         artist_id       artist_mbid                 
   <chr>           <chr>           <chr>           <chr>                       
 1 TRSTWXA12903D1~ SOCQMXH12AC468~ ARNNU4G1187FB5~ 5c176092-cb4d-4e05-806b-1e9~
 2 TRQNQZX128F422~ SOWULXB12A6D4F~ ARJ9DSA1187B99~ 4756395c-57ed-4a63-afb2-011~
 3 TRTPVEB128F426~ SOHKYIC12A8AE4~ ARQ8CJ61187FB3~ 15ab8bb8-7348-4377-ab73-b7a~
 4 TRZZNFB128F933~ SOJVHKV12AB018~ ARFAKTH1187B9B~ 0174d942-39da-4dcd-aa48-d0f~
 5 TRZAAWN128F92E~ SORYVBT12AB017~ ARNMWP51187FB3~ 4dca4bb2-23ba-4103-97e6-581~
 6 TRRAOAD12903CE~ SODTBYX12AC3DF~ ARMDWND1187B9A~ 8ee00333-ec2c-439b-a619-ae1~
 7 TRPGBAO128F42B~ SOCTEWS12A8C13~ ARBSLZ11187FB4~ f940c4dd-f687-4c6a-872b-1cc~
 8 TRQLTYD128F146~ SONNCTZ12A6D4F~ ARM7YQQ1187B9A~ 4d2956d1-a3f7-44bb-9a41-675~
 9 TRJNORS128F145~ SOGQHHK12A6D4F~ ARBK4PS1187B9A~ 637504e3-be95-4005-83ee-3eb~
10 TRLSQUC128F422~ SOTFGIG12A6D4F~ ARRHNLN1187FB5~ d86c3c8b-84d5-44ce-80fa-7cf~
# ... with 990 more rows

```

***

## Mother's little helper (2)

```r
# track_metadata_tbl has been pre-defined
track_metadata_tbl

track_metadata_tbl %>%
  # Select columns containing ti
  select(contains("ti"))

track_metadata_tbl %>%
  # Select columns matching ti.?t
  select(matches("ti.?t"))
  
```

Output:

```bash
# track_metadata_tbl has been pre-defined
track_metadata_tbl
# Source:   table<track_metadata> [?? x 11]
# Database: spark_connection
   track_id title song_id release artist_id artist_mbid artist_name duration
   <chr>    <chr> <chr>   <chr>   <chr>     <chr>       <chr>          <dbl>
 1 TRSTWXA~ The ~ SOCQMX~ Espana  ARNNU4G1~ 5c176092-c~ 101 Strings     221.
 2 TRQNQZX~ Soul~ SOWULX~ Diary ~ ARJ9DSA1~ 4756395c-5~ John Mayal~     368.
 3 TRTPVEB~ The ~ SOHKYI~ The Bl~ ARQ8CJ61~ 15ab8bb8-7~ Illinois J~     329.
 4 TRZZNFB~ The ~ SOJVHK~ Give M~ ARFAKTH1~ 0174d942-3~ Chairmen O~     193.
 5 TRZAAWN~ Litt~ SORYVB~ Beauti~ ARNMWP51~ 4dca4bb2-2~ Curtis May~     321.
 6 TRRAOAD~ Will~ SODTBY~ Jammys~ ARMDWND1~ 8ee00333-e~ Black Uhuru     179.
 7 TRPGBAO~ Chil~ SOCTEW~ Pure A~ ARBSLZ11~ f940c4dd-f~ Alex de Gr~     162.
 8 TRQLTYD~ Ring~ SONNCT~ Eat To~ ARM7YQQ1~ 4d2956d1-a~ Blondie         210.
 9 TRJNORS~ Chat~ SOGQHH~ Sid Si~ ARBK4PS1~ 637504e3-b~ Sid Vicious     111.
10 TRLSQUC~ Migh~ SOTFGI~ Rockin~ ARRHNLN1~ d86c3c8b-8~ Jelly Roll~     242.
# ... with 990 more rows, and 3 more variables: artist_familiarity <dbl>,
#   artist_hotttnesss <dbl>, year <int>
track_metadata_tbl %>%
  # Select columns containing ti
  select(contains("ti"))
# Source:   lazy query [?? x 7]
# Database: spark_connection
   title artist_id artist_mbid artist_name duration artist_familiar~
   <chr> <chr>     <chr>       <chr>          <dbl>            <dbl>
 1 The ~ ARNNU4G1~ 5c176092-c~ 101 Strings     221.            0.467
 2 Soul~ ARJ9DSA1~ 4756395c-5~ John Mayal~     368.            0.627
 3 The ~ ARQ8CJ61~ 15ab8bb8-7~ Illinois J~     329.            0.441
 4 The ~ ARFAKTH1~ 0174d942-3~ Chairmen O~     193.            0.536
 5 Litt~ ARNMWP51~ 4dca4bb2-2~ Curtis May~     321.            0.787
 6 Will~ ARMDWND1~ 8ee00333-e~ Black Uhuru     179.            0.707
 7 Chil~ ARBSLZ11~ f940c4dd-f~ Alex de Gr~     162.            0.565
 8 Ring~ ARM7YQQ1~ 4d2956d1-a~ Blondie         210.            0.759
 9 Chat~ ARBK4PS1~ 637504e3-b~ Sid Vicious     111.            0.621
10 Migh~ ARRHNLN1~ d86c3c8b-8~ Jelly Roll~     242.            0.468
# ... with 990 more rows, and 1 more variable: artist_hotttnesss <dbl>
track_metadata_tbl %>%
  # Select columns matching ti.?t
  select(matches("ti.?t"))
# Source:   lazy query [?? x 6]
# Database: spark_connection
   title   artist_id  artist_mbid artist_name artist_familiar~ artist_hotttnes~
   <chr>   <chr>      <chr>       <chr>                  <dbl>            <dbl>
 1 The Br~ ARNNU4G11~ 5c176092-c~ 101 Strings            0.467            0.369
 2 Soul O~ ARJ9DSA11~ 4756395c-5~ John Mayal~            0.627            0.405
 3 The Ga~ ARQ8CJ611~ 15ab8bb8-7~ Illinois J~            0.441            0.355
 4 The Tw~ ARFAKTH11~ 0174d942-3~ Chairmen O~            0.536            0.415
 5 Little~ ARNMWP511~ 4dca4bb2-2~ Curtis May~            0.787            0.495
 6 Willow~ ARMDWND11~ 8ee00333-e~ Black Uhuru            0.707            0.463
 7 Childr~ ARBSLZ111~ f940c4dd-f~ Alex de Gr~            0.565            0.381
 8 Ring O~ ARM7YQQ11~ 4d2956d1-a~ Blondie                0.759            0.551
 9 Chatte~ ARBK4PS11~ 637504e3-b~ Sid Vicious            0.621            0.455
10 Mighty~ ARRHNLN11~ d86c3c8b-8~ Jelly Roll~            0.468            0.303
# ... with 990 more rows

```

***

## Selecting unique rows

```r
# track_metadata_tbl has been pre-defined
track_metadata_tbl

track_metadata_tbl %>%
  # Only return rows with distinct artist_name
  distinct(artist_name)

```

Output:

```bash
# track_metadata_tbl has been pre-defined
track_metadata_tbl
# Source:   table<track_metadata> [?? x 11]
# Database: spark_connection
   track_id title song_id release artist_id artist_mbid artist_name duration
   <chr>    <chr> <chr>   <chr>   <chr>     <chr>       <chr>          <dbl>
 1 TRSTWXA~ The ~ SOCQMX~ Espana  ARNNU4G1~ 5c176092-c~ 101 Strings     221.
 2 TRQNQZX~ Soul~ SOWULX~ Diary ~ ARJ9DSA1~ 4756395c-5~ John Mayal~     368.
 3 TRTPVEB~ The ~ SOHKYI~ The Bl~ ARQ8CJ61~ 15ab8bb8-7~ Illinois J~     329.
 4 TRZZNFB~ The ~ SOJVHK~ Give M~ ARFAKTH1~ 0174d942-3~ Chairmen O~     193.
 5 TRZAAWN~ Litt~ SORYVB~ Beauti~ ARNMWP51~ 4dca4bb2-2~ Curtis May~     321.
 6 TRRAOAD~ Will~ SODTBY~ Jammys~ ARMDWND1~ 8ee00333-e~ Black Uhuru     179.
 7 TRPGBAO~ Chil~ SOCTEW~ Pure A~ ARBSLZ11~ f940c4dd-f~ Alex de Gr~     162.
 8 TRQLTYD~ Ring~ SONNCT~ Eat To~ ARM7YQQ1~ 4d2956d1-a~ Blondie         210.
 9 TRJNORS~ Chat~ SOGQHH~ Sid Si~ ARBK4PS1~ 637504e3-b~ Sid Vicious     111.
10 TRLSQUC~ Migh~ SOTFGI~ Rockin~ ARRHNLN1~ d86c3c8b-8~ Jelly Roll~     242.
# ... with 990 more rows, and 3 more variables: artist_familiarity <dbl>,
#   artist_hotttnesss <dbl>, year <int>
track_metadata_tbl %>%
  # Only return rows with distinct artist_name
  distinct(artist_name)
# Source:   lazy query [?? x 1]
# Database: spark_connection
   artist_name       
   <chr>             
 1 101 Strings       
 2 Light Of The World
 3 Raul Seixas       
 4 Be Bop Deluxe     
 5 Eugenio Finardi   
 6 Cerrone           
 7 Tower Of Power    
 8 Killing Joke      
 9 Dobie Gray        
10 Gil Scott-Heron   
# ... with 749 more rows

```
***

## Common people

```r

# track_metadata_tbl has been pre-defined
track_metadata_tbl

track_metadata_tbl %>%
  # Count the artist_name values
  count(artist_name, sort = TRUE) %>%
  # Restrict to top 20
  top_n(20)
  
```

Output:

```bash
# track_metadata_tbl has been pre-defined
track_metadata_tbl
# Source:   table<track_metadata> [?? x 11]
# Database: spark_connection
   track_id title song_id release artist_id artist_mbid artist_name duration
   <chr>    <chr> <chr>   <chr>   <chr>     <chr>       <chr>          <dbl>
 1 TRSTWXA~ The ~ SOCQMX~ Espana  ARNNU4G1~ 5c176092-c~ 101 Strings     221.
 2 TRQNQZX~ Soul~ SOWULX~ Diary ~ ARJ9DSA1~ 4756395c-5~ John Mayal~     368.
 3 TRTPVEB~ The ~ SOHKYI~ The Bl~ ARQ8CJ61~ 15ab8bb8-7~ Illinois J~     329.
 4 TRZZNFB~ The ~ SOJVHK~ Give M~ ARFAKTH1~ 0174d942-3~ Chairmen O~     193.
 5 TRZAAWN~ Litt~ SORYVB~ Beauti~ ARNMWP51~ 4dca4bb2-2~ Curtis May~     321.
 6 TRRAOAD~ Will~ SODTBY~ Jammys~ ARMDWND1~ 8ee00333-e~ Black Uhuru     179.
 7 TRPGBAO~ Chil~ SOCTEW~ Pure A~ ARBSLZ11~ f940c4dd-f~ Alex de Gr~     162.
 8 TRQLTYD~ Ring~ SONNCT~ Eat To~ ARM7YQQ1~ 4d2956d1-a~ Blondie         210.
 9 TRJNORS~ Chat~ SOGQHH~ Sid Si~ ARBK4PS1~ 637504e3-b~ Sid Vicious     111.
10 TRLSQUC~ Migh~ SOTFGI~ Rockin~ ARRHNLN1~ d86c3c8b-8~ Jelly Roll~     242.
# ... with 990 more rows, and 3 more variables: artist_familiarity <dbl>,
#   artist_hotttnesss <dbl>, year <int>
track_metadata_tbl %>%
  # Count the artist_name values
  count(artist_name, sort = TRUE) %>%
  # Restrict to top 20
  top_n(20)
Selecting by  n
# Source:     lazy query [?? x 2]
# Database:   spark_connection
# Ordered by: desc(n)
   artist_name                     n
   <chr>                       <dbl>
 1 Bukka White                    20
 2 Merle Travis                   13
 3 Skip James                     12
 4 Tampa Red                      12
 5 Billie Holiday                  9
 6 Sleepy John Estes               9
 7 Bud Powell                      9
 8 "Arthur \"Big Boy\" Crudup"     8
 9 Big Joe Williams                7
10 Lonnie Johnson                  6
11 Harry Belafonte                 6
12 Charley Patton                  6
13 Leon Payne                      6
14 Bing Crosby                     5
15 Red Foley                       5
16 Tom Lehrer                      5
17 Thelonious Monk                 4
18 Martin Denny                    4
19 Gene Autry                      4
20 Percy Mayfield                  4

```

***

## Collecting data back from Spark

```r
# track_metadata_tbl has been pre-defined
track_metadata_tbl

results <- track_metadata_tbl %>%
  # Filter where artist familiarity is greater than 0.9
  filter(artist_familiarity  > 0.9)

# Examine the class of the results
class(results)

# Collect your results
collected <- results %>%
 collect()

# Examine the class of the collected results
class(collected)


```


Output:

```bash
# track_metadata_tbl has been pre-defined
track_metadata_tbl
# Source:   table<track_metadata> [?? x 11]
# Database: spark_connection
   track_id title song_id release artist_id artist_mbid artist_name duration
   <chr>    <chr> <chr>   <chr>   <chr>     <chr>       <chr>          <dbl>
 1 TRSTWXA~ The ~ SOCQMX~ Espana  ARNNU4G1~ 5c176092-c~ 101 Strings     221.
 2 TRQNQZX~ Soul~ SOWULX~ Diary ~ ARJ9DSA1~ 4756395c-5~ John Mayal~     368.
 3 TRTPVEB~ The ~ SOHKYI~ The Bl~ ARQ8CJ61~ 15ab8bb8-7~ Illinois J~     329.
 4 TRZZNFB~ The ~ SOJVHK~ Give M~ ARFAKTH1~ 0174d942-3~ Chairmen O~     193.
 5 TRZAAWN~ Litt~ SORYVB~ Beauti~ ARNMWP51~ 4dca4bb2-2~ Curtis May~     321.
 6 TRRAOAD~ Will~ SODTBY~ Jammys~ ARMDWND1~ 8ee00333-e~ Black Uhuru     179.
 7 TRPGBAO~ Chil~ SOCTEW~ Pure A~ ARBSLZ11~ f940c4dd-f~ Alex de Gr~     162.
 8 TRQLTYD~ Ring~ SONNCT~ Eat To~ ARM7YQQ1~ 4d2956d1-a~ Blondie         210.
 9 TRJNORS~ Chat~ SOGQHH~ Sid Si~ ARBK4PS1~ 637504e3-b~ Sid Vicious     111.
10 TRLSQUC~ Migh~ SOTFGI~ Rockin~ ARRHNLN1~ d86c3c8b-8~ Jelly Roll~     242.
# ... with 990 more rows, and 3 more variables: artist_familiarity <dbl>,
#   artist_hotttnesss <dbl>, year <int>
results <- track_metadata_tbl %>%
  # Filter where artist familiarity is greater than 0.9
  filter(artist_familiarity  > 0.9)
# Examine the class of the results
class(results)
[1] "tbl_spark" "tbl_sql"   "tbl_lazy"  "tbl"      
# Collect your results
collected <- results %>%
 collect()
# Examine the class of the collected results
class(collected)
[1] "tbl_df"     "tbl"        "data.frame"

```
