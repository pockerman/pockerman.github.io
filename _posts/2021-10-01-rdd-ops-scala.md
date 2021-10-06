---
title: 'Apache Spark: RDD operations with Scala'
date: 2021-10-01
permalink: /posts/2021/10/apache_spark_rdd_ops_scala/
tags:
  - apache-spark
  - scala
  - resilient-distributed-dataset
  - big-data
---

## Overview

TODO

### Acknowledgements

The content of this notebooks is to a large extent edited from [1].

## RDD operations with Scala

A Spark RDD provides two types of operations

- Transformations
- Actions

### Transformations

A transformation operation creates a new RDD from an existing RDD. Moreover, we can apply a chain of
transformations once the data is loaded into memory. Some common transformations are ```filter``` and ```map```

#### Transformations examples

In this section, I will review some common RDD transformations. 

- ```map(function)```: It returns a new data set by operating on each element of the source RDD.

- ```flatMap(function)```: Similar to map, but each item can be mapped to zero, one, or more items.

- ```mapPartitions(function)```: Similar to map, but works on the partition level.

- ```mapPartitionsWithIndex(function)```: Similar to ```mapPartitions```, but provides a function with an Int value to indicate the index position of the partition.

- ```filter(function)```: It returns a new RDD that contains only elements that satisfy the predicate.

- ```union(otherDataset)```: It returns a new data set that contains the elements of the source RDD and the ```otherDataset```  RDD. Note that the participating RDDs should be of the same data type.

- ```intersection(otherDataset)```: It returns a new data set that contains the intersection of elements from the source RDD and the argument RDD.

- ```randomSplit```:

### Actions

Spark transformations are lazy evaluated. What this means that a transformation is applied only when an action is called,
Let's see some examples of actions.


#### Actions examples

- ```collect()```: Returns all the elements of the data set are returned as an array to the driver program.

- ```count()```:  Returns the number of elements in the data set.

- ```reduce(function)```: It returns a data set by aggregating the elements of the RDD it is applied on. The aggregation is done by using  the user provided ```function``` argument. The ```function``` should take two arguments and returns a single argument. Moreover it should be commutative and associative so that it can be operated in parallel. 

- ```first()```: Returns the first element in the data set.

- ```take(n)```: Returns the first ```n``` elements in the data set as an array.

- ```takeOrdered(n)```: 
- ```top(n)```: 
- ```takeSample```:
- ```saveAsTextFile(path)```: Write the elements of the RDD as a text file in the local file system, HDFS, or any another supported storage system.

- ```foreach(function)```: Applies the ```function``` argument on each element in the RDD.
- ```countApprox(n)```: 

## References

1. Subhashini Chellappan, Dharanitharan Ganesan, ```Practical Apache Spark. Using the Scala API```, Apress
