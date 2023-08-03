# Repartition and Coalesce

Apache Spark, the powerful framework for big data processing, has a secret to its efficiency: data partitioning. Spark divides the data into smaller chunks called partitions and performs computations on these partitions in parallel. This parallel processing enables Spark to crunch vast amounts of data swiftly.

Data partitioning in Spark happens automatically, but there are times when manual adjustment becomes necessary for optimal performance. Understanding when and how to adjust partitioning ensures your Spark computations run efficiently and smoothly, handling even the most challenging data processing tasks.

## Repartition

The repartition method is used to increase or decrease the number of partitions in the data frame

```
# Import the necessary libraries
from pyspark.sql import SparkSession
from pyspark.sql.functions import col

# Create a SparkSession
spark = SparkSession.builder.appName("RepartitionAndCoalesceExample").getOrCreate()

# Sample data for the DataFrame
data = [("Alice", 28), ("Bob", 22), ("Charlie", 35), ("Diana", 40), ("Eva", 30)]

# Create a DataFrame
df = spark.createDataFrame(data, ["Name", "Age"])

# Let's check the default number of partitions
print("Default number of partitions:", df.rdd.getNumPartitions())

# Repartition the DataFrame into 4 partitions
df_repartitioned = df.repartition(4)

# Check the number of partitions after repartitioning
print("Number of partitions after repartitioning:", df_repartitioned.rdd.getNumPartitions())

# Sample output after repartitioning:
# Default number of partitions: 1
# Number of partitions after repartitioning: 4

```

## Coalesce

The coalesce method is used to reduce the number of partitions in a data frame.

```
# Coalesce the DataFrame into 2 partitions
df_coalesced = df.coalesce(2)

# Check the number of partitions after coalescing
print("Number of partitions after coalescing:", df_coalesced.rdd.getNumPartitions())

# Sample output after coalescing:
# Number of partitions after coalescing: 2

```
Even if you try to increase the number of partitions with coalesce, it won't work !

```
# Coalesce the DataFrame into 4 partitions
df_coalesced = df.coalesce(4)

# Check the number of partitions after coalescing
print("Number of partitions after coalescing:", df_coalesced.rdd.getNumPartitions())

# Sample output after coalescing:
# Number of partitions after coalescing: 1

```

The repartition does a full shuffle of the data and creates equal-sized partitions of data. coalesce combine existing partitions to avoid a full shuffle