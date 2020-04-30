# Introduction to Bash Scripting
## From Command-Line to Bash Script

## Extracting scores with shell

There is a file in either the start_dir/first_dir, start_dir/second_dir or start_dir/third_dir directory called soccer_scores.csv. It has columns Year,Winner,Winner Goals for outcomes of a soccer league.

cd into the correct directory and use cat and grep to find who was the winner in 1959. You could also just ls from the top directory if you like!


```bash
repl:~/start_dir/second_dir$ grep '1959' soccer_scores.csv
1959,Dunav,2

```

or `cat soccer_scores.csv | grep 1959'`


***

# Searching a book with shell

Count the number of lines in the book that contain either the character 'Sydney Carton' or 'Charles Darnay'.

```bash
repl:~$ cat two_cities.txt | grep -E "Sydney Carton|Charles Darnay" | wc -l
77

```
Or:

```bash
cat two_cities.txt | egrep 'Sydney Carton|Charles Darnay' | wc -l
```

## A simple Bash script

```bash
#!/usr.bash

# Concatenate the file
cat server_log_with_todays_date.txt


# Now save and run!

```

Output:

```bash

repl:~/workspace$ cd /home/repl/workspace
repl:~/workspace$ bash script.sh
2019-01-01 | server request | windows | ping-2.000
2019-02-01 | server request | mac | ping-2.000
2019-02-01 | server request | windows | ping-12.000
2019-02-01 | server request | linux | ping-6.000repl:~/workspacrepl:~/workspace$ 

```

***

## Shell pipelines to Bash scripts

```bash

#!/usr/bash

# Create a single-line pipe
cat soccer_scores.csv | cut -d "," -f 2 | tail -n +2 | sort | uniq -c

# Now save and run!

```

Output:

```bash

repl:~/workspace$ bash script.sh
     13 Arda
      8 Beroe
      9 Botev
      8 Cherno
     17 Dunav
     15 Etar
      4 Levski
      1 Lokomotiv
repl:~/workspace$ 


```

***

## Extract and edit using Bash scripts





