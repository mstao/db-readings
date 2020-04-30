MySQL常见的日志类型包括：

- 错误日志（error log）
- 二进制日志（binlog）
- 慢查询日志（slow query log）
- 查询日志（log）

# 错误日志

错误日志文件对MySQL的启动、运行、关闭过程进行了记录。MySQL DBA在遇到问题时应该首先查看该文件以便定位问题。该文件不仅记录了所有的错误信息，也记录一些警告信息或正确的信息。通过`SHOW VARIABLES LIKE '%log_error%'`来查看错误的路径：如下所示：

Variable_name      |Value              |
-------------------|-------------------|
binlog_error_action|ABORT_SERVER       |
log_error          |/var/log/mysqld.log|
log_error_verbosity|3                  |

# 慢查询日志

https://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html

查看慢查询的日志是否开启，以及日志文件位置：

```
SHOW VARIABLES LIKE '%slow_query_log%'
```

Variable_name      |Value                            |
-------------------|---------------------------------|
slow_query_log     |OFF                              |
slow_query_log_file|/var/lib/mysql/localhost-slow.log|


慢查询日志默认是关闭的，通过如下命令将其开启：

```
SET GLOBAL slow_query_log=ON
```

一个查询消耗多长时间被定义为慢查询呢？MySQL是通过**long_query_time**这个参数来控制的：

```
SHOW VARIABLES LIKE '%long_query_time%'
```

Variable_name  |Value    |
---------------|---------|
long_query_time|10.000000|

long_query_time单位是秒，默认十秒，可以通过以下命令进行设置：

```
SET GLOBAL long_query_time=1
```

如果有SQL查询时间超过设置的阈值，就会记录到慢查询日志里面，我们可以用 `tail -f xx-slow.log`来查看慢查询日志信息，比如：

```
$ tail -f db2-slow.log 
# Query_time: 1.290560  Lock_time: 0.001066 Rows_sent: 0  Rows_examined: 77
use iwmsfacility;
SET timestamp=1588212896;
SELECT DISTINCT r.*
FROM iwmsreceivebill AS r
WHERE (r.companyUuid = '0000001') AND (r.dcUuid = '000000110000029') AND (EXISTS (SELECT 1
FROM iwmsreceivebillitem AS i
WHERE (r.uuid = i.billUuid) AND (i.articleCode IN ('q'))))
ORDER BY r.billNumber DESC
LIMIT 61;
```

除此之外，如果查询没有使用索引，那么被记录到慢查询的日志概率性较大，MySQL 提供了 `log_queries_not_using_indexes`参数来控制是否将没有使用索引的查询记录到慢查询日志里面：

```
SHOW VARIABLES LIKE '%log_queries_not_using_indexes%'
```

Variable_name                |Value|
-----------------------------|-----|
log_queries_not_using_indexes|OFF  |

`log_queries_not_using_indexes`参数默认是关闭的，我们可以将其打开，

```
SET GLOBAL log_queries_not_using_indexes=ON
```

如果`log_queries_not_using_indexes`打开了，MySQL提供了`log_throttle_queries_not_using_indexes`参数来控制每分钟最好写入多少条这样的记录到慢查询日志：

```
SHOW VARIABLES LIKE '%log_throttle_queries_not_using_indexes%'
```

Variable_name                         |Value|
--------------------------------------|-----|
log_throttle_queries_not_using_indexes|0    |

默认是0，代表没有限制。

# 二进制文件

