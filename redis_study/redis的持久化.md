# Redis的持久化

**配置文件都是对redis服务器的配置,可以配置是否要进行持久化或者其他的操作**

## RDB

 **在指定时间间隔内将内存中的数据存储到磁盘中去,恢复时将文件恢复到内存中去**

### 1. 间隔时间:可以在redis的配置文件中进行配置


	    - save 900 1:在15min中至少一个键的值被更改  
	    - save 300 10:在5min中至少10个键的值被修改                                                                                                                                                                
	    - save 60 10000:在60秒内有10000个键被修改
		- 如果将所有save都注释掉或者是save "",将会关掉这个功能
		- 可以通过lastsave命令获取最后一次成功执行快照的时间
		- redis-cli config set save " ":关掉在间隔时间自动保存dump文件的功能

### 2. 生成dump.rdb文件的情况

	- 超过redis配置文件所设置的时间将会自动生成该文件
	- 执行FLUSHALL命令,也会产生dump.rdb文件
	- 当执行shutdown命令,停掉服务器时,同样会生成dump.rdb文件
	- 执行sava命令或者bgsave命令

### 3. 生成dump.rdb文件的方式

- Redis会创建一个子进程来进行持久化,这个子进程会存在主进程所有的数据,并且使用这个子进程来进行持  
	  久化的操作,并且在sava过程中只进行保存,并不会写入,所有的写入操作都会被阻塞
- BGSAVE 命令执行之后立即返回 OK ，然后 Redis fork 出一个新子进程，原来的 Redis 进程(父进程)继续处理客户端请求，而子进程则负责将数据保存到磁盘，然后退出

### 4. stop-writes-on-bgsave-error命令

- 当执行持久化文件到磁盘出错时,如果上面在配置文件设置该项为yea时,那么当出错时数据将无法存储

### 5. 如何对redis进行恢复

- dump.rdb这个文件名是Redis配置文件中声明的默认Dump的文件名,而这个文件是会存储到当前的目录(redis-cli和redis-server执行的当前目录),当数据进行恢复时,直接将dump文件移动到这个目录下,redis在启动服务器时会默认读取该文件

### 6. 优势

- 适合大规模的数据恢复
- 对数据的完整性和一致性要求不高

  ### 7. 劣势
- 在一定时间间隔内做一次备份,如果redis意外down,就会丢失最后一次快照后的所有修改
- fork进程时,所使用的内存将会膨胀两倍

### 7. dump文件出错

	使用命令redis-check-rdb命令对文件进行修复

## AOF

**redis持久化的另一种形式**

### 1. 简介

	以日志的形式来记录每个写操作，将Redis执行过的所有写指令记录下来(读操作不记录)，
	只许追加文件但不可以改写文件，redis启动之初会读取该文件重新构建数据，换言之，redis
	重启的话就根据日志文件的内容将写指令从前到后执行一次以完成数据的恢复工作

### 2. redis开启AOF进行持久化

- 修改配置文件redis.conf,将no修改为yes
	
			appendonly no

- redis默认生成的append only file的文件名,配置文件中的设置

		appendfilename "appendonly.aof"

- redis中aof同步的时间间隔

		 appendfsync always:每一次操作都会进行同步
		 appendfsync everysec:每秒钟同步一次
		 appendfsync no:从不同步

- redis中aof文件存储的内容:redis所有进行的写操作

- redis持久化中rdb和aof同时开启时,redis会读取aof配置文件,而不会读取rdb文件

- 当aof文件出错时,可以使用redis-check-aof --fix命令对其进行修复

### 3. rewrite操作

**当aof文件超过一定的阀值,将会触发重写机制,对AOF文件进行压缩**

1. 触发重新机制的阀值,在redis.conf配置文件下的内容
	
		 Specify a percentage of zero in order to disable the automatic AOF
	
	rewrite feature.
	
		 auto-aof-rewrite-percentage 100
	 auto-aof-rewrite-min-size 64mb
		
		- 当文件大小为最近一次rewrite后文件大小的两倍,如果还没有rewrite,那么这个大小便为刚启动时aof  
		文件的大小,并且需要这个文件大小超过min-size的大小,这是为了防止此时aof文件大小已经是上次大小   
		的两倍但还是比较小的情况


2. aof的rewrite机制是如何实现?

		AOF文件持续增长而过大时，会fork出一条新进程来将文件重写(也是先写临时文件最后再rename)，
		遍历新进程的内存中数据，每条记录有一条的Set语句。重写aof文件的操作，并没有读取旧的aof文件，
		而是将整个内存中的数据库内容用命令的方式重写了一个新的aof文件，这点和快照有点类似




​	

​	





​	
