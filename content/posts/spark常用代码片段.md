---
title: spark常用代码片段
hideSummary: True
hideMeta: True
---



# 1. beeline从hive拉数据到本地

```bash 
beeline --outputformat=csv2 -e "SELECT * FROM xxxx" > /data/xxxx.csv

```

# 2. pyspark从hive读数据的操作


```python
import findspark
findspark.init()

from pyspark.sql import SparkSession

ss = SparkSession.builder\\
.appName("xxx_app")\\
.config("spark.executor.memory", "20g")\\
.config("spark.driver.memory", "20g")\\
.config("spark.memory.offHeap.enabled",True)\\
.config("spark.memory.offHeap.size","56g")\\
.config("spark.driver.maxResultSize", '60g')\\
.config("spark.sql.execution.arrow.enabled", "true")\\
.config('spark.dynamicAllocation.enabled','true')\\
.enableHiveSupport()\\
.getOrCreate()
hive_read_sql = "SELECT * FROM XXX"
df = ss.sql(hive_read_sql)

```
