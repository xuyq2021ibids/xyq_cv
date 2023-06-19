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

# 多窗口管理

一种方式是用nhup，还有一种是[tmux](https://www.ruanyifeng.com/blog/2019/10/tmux.html)

Ctrl+b d：分离当前会话。
Ctrl+b s：列出所有会话。
Ctrl+b $：重命名当前会话。
## 新建会话和列出会话
```bash
$ tmux new -s <session-name>
$ tmux ls
```

## 切换会话
```bash
# 使用会话编号
$ tmux switch -t 0
# 使用会话名称
$ tmux switch -t <session-name>
```
## 杀掉会话
```bash
# 使用会话编号
$ tmux kill-session -t 0
# 使用会话名称
$ tmux kill-session -t <session-name>
```