# 为了修正mysql UTF+8 的 Datatime 列 在sink的devoke中多了8小时的bug -- benjamin
1. MySqlSnapshotSplitReadTask.srcDbTimeZone 时区要使用 serverTimezone
2. MySqlConverter里面Timestamp不要转为LocalDateTime (这步不是必需的.如果能保证cdc运行在UTC环境下,时间也不会有误差)
    为了实现这点,使用了以 PFMySqlValueConverters 复写 PFMySqlValueConverters 的方式
3. flink-sql-connector-mysql-cdc 的版本要改为2.2.3
    注意要保证 flink-cdc-base 和 flink-connector-debezium 的版本号是2.2.1(不要2.2-SNAPSHOT)
4. 