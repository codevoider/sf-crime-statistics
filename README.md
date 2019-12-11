# SF Crime Statistics with Spark Streaming Project

## Overview

In this project, you will be provided with a real-world dataset, extracted from Kaggle, on San Francisco crime incidents, and you will provide statistical analyses of the data using Apache Spark Structured Streaming. You will draw on the skills and knowledge you've learned in this course to create a Kafka server to produce data, and ingest data through Spark Structured Streaming.

## Requirements

* Java 1.8.x
* Scala 2.11.x
* Spark 2.4.x
* Kafka
* Python 3.6 or above

## How to use the application

### Start kafka

1. Zookeeper:

`/usr/bin/zookeeper-server-start config/zookeeper.properties`

2. Kafka server:

`/usr/bin/kafka-server-start config/server.properties`

### Start pumping data

1. Insert data into topic:

`python kafka_server.py`

2. Kafka consumer:

`kafka-console-consumer --from-beginning --bootstrap-server localhost:9092`

### Trigger spark job

`spark-submit --packages org.apache.spark:spark-sql-kafka-0-10_2.11:2.4.4 --master local[*] data_stream.py`


### Kafka consumer output data

![kafka data](https://github.com/xiaoyifan/sf-crime-statistics/blob/master/data.png)

### Count result

![spark count](https://github.com/xiaoyifan/sf-crime-statistics/blob/master/result.png)

## Q&A:

**1. How did changing values on the SparkSession property parameters affect the throughput and latency of the data?**

The performance evaluation metric we could refer to is `processedRowsPerSecond`, higher the number, better performance we have.


**2. What were the 2-3 most efficient SparkSession property key/value pairs? Through testing multiple variations on values, how can you tell these were the most optimal?**
To make the performance better, we should maximize the `processedRowsPerSecond` as mentioned above. the most pairs are: 
- spark.default.parallelism
- spark.sql.shuffle.partitions

to pick the parallelism, the ideal number would be `3 * CPU core`. and for partitions, asssuming we are using HDFS, typical partition size is 64MB. then the number is total data size/64MB.