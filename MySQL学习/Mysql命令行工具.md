## 慢日志查询

set long_query_time = 0
是将慢查询日志的阈值设置为 0，表示这个线程接下来的语句都会被记录入慢查询日志中；

## 可以用下面介绍的方法，来确定一个排序语句是否使用了临时文件


	/* 打开optimizer_trace，只对本线程有效 */
	SET optimizer_trace='enabled=on'; 
	
	/* @a保存Innodb_rows_read的初始值 */
	select VARIABLE_VALUE into @a from  performance_schema.session_status where variable_name = 'Innodb_rows_read';
	
	/* 执行语句 */
	select city, name,age from t where city='杭州' order by name limit 1000; 
	
	/* 查看 OPTIMIZER_TRACE 输出 */
	SELECT * FROM `information_schema`.`OPTIMIZER_TRACE`\G
	
	/* @b保存Innodb_rows_read的当前值 */
	select VARIABLE_VALUE into @b from performance_schema.session_status where variable_name = 'Innodb_rows_read';
	
	/* 计算Innodb_rows_read差值 */
	select @b-@a;