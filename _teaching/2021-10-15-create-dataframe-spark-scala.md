---
title: 'Apache Spark. Create a  DataFrame with Scala'
date: 2021-10-15
permalink: /teaching/2021/10/spark_df/
tags:
  - spark
  - scala
  - big-data
  - data-engineering
  - data-analysis
---

## Overview

In this post, we will review how to create a ```DataFrame``` in Spark using the Scala API.

##  Create a  DataFrame with Scala

The Spark ```DataFrame``` is inspired by the equivalent pandas ```DataFrame```. A ```DataFrame``` is Spark is like a distributed in-memory table with named columns and schemas [1]. Similar to an RDD, a ```DataFrame``` is also immutable and Spark keeps a lineage of all the transformations. Thus, when we add or change the names and types of the columns, a new ```DataFrame``` is actually created.

```
package train.spark

import org.apache.spark.sql.SparkSession

object CreateDataFrame {

  
  def main(args: Array[String]) {
    
    val spark = SparkSession
    .builder()
    .appName("Spark DataFrame Demo")
    .getOrCreate()
    
    val csvFile = "/home/alex/qi3/learn_scala/scripts/spark/data/train.csv" 
    val df = spark.read.csv(csvFile)
    
    // print the schema
    df.printSchema()
   
  }
}
```

The output is 

```
21/10/15 15:22:10 WARN Utils: Your hostname, LT-2R0620-101 resolves to a loopback address: 127.0.1.1; using 192.168.0.71 instead (on interface wlp58s0)
21/10/15 15:22:10 WARN Utils: Set SPARK_LOCAL_IP if you need to bind to another address
WARNING: An illegal reflective access operation has occurred
WARNING: Illegal reflective access by org.apache.spark.unsafe.Platform (file:/home/alex/MySoftware/spark-3.0.1-bin-hadoop2.7/jars/spark-unsafe_2.12-3.0.1.jar) to constructor java.nio.DirectByteBuffer(long,int)
WARNING: Please consider reporting this to the maintainers of org.apache.spark.unsafe.Platform
WARNING: Use --illegal-access=warn to enable warnings of further illegal reflective access operations
WARNING: All illegal access operations will be denied in a future release
21/10/15 15:22:11 WARN NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
root
 |-- _c0: string (nullable = true)
 |-- _c1: string (nullable = true)
 |-- _c2: string (nullable = true)
 |-- _c3: string (nullable = true)
 |-- _c4: string (nullable = true)


```

### Specify a schema

```
package train.spark

import org.apache.spark.sql.SparkSession
import org.apache.spark.sql.SQLContext
import org.apache.spark.sql.types._

import org.apache.spark.SparkContext
import org.apache.spark.SparkContext._
import org.apache.spark.SparkConf


object CreateDataFrame {

  
  def main(args: Array[String]) {
  
    val csvFile = "/home/alex/qi3/learn_scala/scripts/spark/data/train.csv" 
    val appName: String = "Spark DataFrame Demo"
    
    val conf = new SparkConf().setAppName(appName)
    val sc = new SparkContext(conf)
    val sqlContext = new SQLContext(sc)
    
    val customSchema = StructType(Array(
			StructField("mu-1", DoubleType, false),
  			StructField("mu-2", DoubleType, false),
	  		StructField("label", IntegerType, false)))
    
    // specify a schema
    val df_schema = sqlContext.read.format("csv")
    			  .option("delimiter",",")
    			  .schema(customSchema)
    			  .load(csvFile)
    			  
    df_schema.printSchema()	
    
    df_schema.groupBy("label").count().show()
    df_schema.show(5)
   
  }
}
```

```
root
 |-- mu-1: double (nullable = true)
 |-- mu-2: double (nullable = true)
 |-- label: integer (nullable = true)

+-----+-----+
|label|count|
+-----+-----+
| null|    1|
|    1|  185|
|    3|  185|
|    4|  185|
|    2|  185|
|    0|  185|
+-----+-----+

+-----+-----+-----+
| mu-1| mu-2|label|
+-----+-----+-----+
| null| null| null|
|22.91|28.54|    0|
|17.26|30.72|    0|
|17.05|31.08|    0|
|24.05|26.27|    0|
+-----+-----+-----+
only showing top 5 rows

```

## References

1.  Jules S. Damji, Brooke Wenig, Tathagata Das, Denny Lee, ```Learning Spark. Lighting-fast data analytics```, O'Reilly, 2nd Edition.


```python

```
