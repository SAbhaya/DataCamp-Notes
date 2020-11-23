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
