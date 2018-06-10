---
layout: post
title:  redis系列 02 - redis配置
tag: redis
date: 2018-01-10 14:48:20 +09:00
---

```
                _._
           _.-``__ ''-._
      _.-``    `.  `_.  ''-._           Redis 4.0.6 (00000000/0) 64 bit
  .-`` .-```.  ```\/    _.,_ ''-._
 (    '      ,       .-`  | `,    )     Running in standalone mode
 |`-._`-...-` __...-.``-._|'` _.-'|     Port: 6379
 |    `-._   `._    /     _.-'    |     PID: 43582
  `-._    `-._  `-./  _.-'    _.-'
 |`-._`-._    `-.__.-'    _.-'_.-'|
 |    `-._`-._        _.-'_.-'    |           http://redis.io
  `-._    `-._`-.__.-'_.-'    _.-'
 |`-._`-._    `-.__.-'    _.-'_.-'|
 |    `-._`-._        _.-'_.-'    |
  `-._    `-._`-.__.-'_.-'    _.-'
      `-._    `-.__.-'    _.-'
          `-._        _.-'
              `-.__.-'
```


## redis 配置

* 连接本地的`redis`客户端

```shell
redis-cli
127.0.0.1:6379> 
```

* 查询`redis`某个配置 CONFIG_SETTING_NAME

```shell
127.0.0.1:6379> CONFIG GET bind
1) "bind"
2) "127.0.0.1 ::1"
```

* 查询 `redis`下所有的配置信息

```shell
127.0.0.1:6379> CONFIG GET *
  1) "dbfilename" # 本地持久化数据库文件名，默认值为dump.rdb
  2) "dump.rdb"
  3) "requirepass" # 设置Redis连接密码，如果配置了连接密码，客户端在连接Redis时需要通过AUTH <password>命令提供密码，默认关闭 requirepass foobared
  4) ""
  5) "masterauth" # 当master服务设置了密码保护时，slav服务连接master的密码 masterauth <master-password>
  6) ""
  7) "cluster-announce-ip"
  8) ""
  9) "unixsocket"
 10) ""
 11) "logfile"# 日志记录方式，默认为标准输出，如果配置Redis为守护进程方式运行，而这里又配置为日志记录方式为标准输出，则日志将会发送给/dev/null
 12) ""
 13) "pidfile" # 当Redis以守护进程方式运行时，Redis默认会把pid写入/var/run/redis.pid文件，可以通过pidfile指定
 14) "/var/run/redis_6379.pid"
 15) "slave-announce-ip"
 16) ""
 17) "maxmemory" # 指定Redis最大内存限制，Redis在启动时会把数据加载到内存中，达到最大内存后，Redis会先尝试清除已到期或即将到期的Key，当此方法处理 后，仍然到达最大内存设置，将无法再进行写入操作，但仍然可以进行读取操作。Redis新的vm机制，会把Key存放内存，Value会存放在swap区
 18) "0"
 19) "maxmemory-samples" # Redis默认的灰选择3个样本进行检测，你可以通过maxmemory-samples进行设置
 20) "5"
 21) "lfu-log-factor"
 22) "10"
 23) "lfu-decay-time"
 24) "1"
 25) "timeout" # 当 客户端闲置多长时间后关闭连接，如果指定为0，表示关闭该功能
 26) "0"
 27) "active-defrag-threshold-lower"
 28) "10"
 29) "active-defrag-threshold-upper"
 30) "100"
 31) "active-defrag-ignore-bytes"
 32) "104857600"
 33) "active-defrag-cycle-min"
 34) "25"
 35) "active-defrag-cycle-max"
 36) "75"
 37) "auto-aof-rewrite-percentage"
 38) "100"
 39) "auto-aof-rewrite-min-size"
 40) "67108864"
 41) "hash-max-ziplist-entries"
 42) "512"
 43) "hash-max-ziplist-value"
 44) "64"
 45) "list-max-ziplist-size"
 46) "-2"
 47) "list-compress-depth"
 48) "0"
 49) "set-max-intset-entries"
 50) "512"
 51) "zset-max-ziplist-entries"
 52) "128"
 53) "zset-max-ziplist-value"
 54) "64"
 55) "hll-sparse-max-bytes"
 56) "3000"
 57) "lua-time-limit"
 58) "5000"
 59) "slowlog-log-slower-than" # 记录超过特定执行时间的命令。执行时间不包括I/O计算比如连接客户端，返回结果等，只是命令执行时间 # 可以通过两个参数设置slow log：一个是告诉Redis执行超过多少时间被记录的参数slowlog-log-slower-than(微妙)
 60) "10000"
 61) "latency-monitor-threshold"
 62) "0"
 63) "slowlog-max-len" # # 另一个是slow log 的长度。当一个新命令被记录的时候最早的命令将被从队列中移除
 64) "128"
 65) "port" # 指定Redis监听端口，默认端口为6379
 66) "6379"
 67) "cluster-announce-port"
 68) "0"
 69) "cluster-announce-bus-port"
 70) "0"
 71) "tcp-backlog"
 72) "511"
 73) "databases" # 设置数据库的数量，默认数据库为0，可以使用SELECT <dbid>命令在连接上指定数据库id
 74) "16"
 75) "repl-ping-slave-period" # 从库会按照一个时间间隔向主库发送PINGs.可以通过repl-ping-slave-period设置这个时间间隔，默认是10秒
 76) "10"
 77) "repl-timeout" # 设置主库批量数据传输时间或者ping回复时间间隔，默认值是60秒
 78) "60"
 79) "repl-backlog-size"
 80) "1048576"
 81) "repl-backlog-ttl"
 82) "3600"
 83) "maxclients" # 设置同一时间最大客户端连接数，默认无限制，Redis可以同时打开的客户端连接数为Redis进程可以打开的最大文件描述符数，如果设置 maxclients 0，表示不作限制。当客户端连接数到达限制时，Redis会关闭新的连接并向客户端返回max number of clients reached错误信息 maxclients 128
 84) "10000"
 85) "watchdog-period"
 86) "0"
 87) "slave-priority"
 88) "100"
 89) "slave-announce-port"
 90) "0"
 91) "min-slaves-to-write"
 92) "0"
 93) "min-slaves-max-lag"
 94) "10"
 95) "hz"
 96) "10"
 97) "cluster-node-timeout"
 98) "15000"
 99) "cluster-migration-barrier"
100) "1"
101) "cluster-slave-validity-factor"
102) "10"
103) "repl-diskless-sync-delay"
104) "5"
105) "tcp-keepalive"
106) "300"
107) "cluster-require-full-coverage"
108) "yes"
109) "no-appendfsync-on-rewrite" #AOF策略设置为always或者everysec时，后台处理进程(后台保存或者AOF日志重写)会执行大量的I/O操作 # 在某些Linux配置中会阻止过长的fsync()请求。注意现在没有任何修复，即使fsync在另外一个线程进行处理 # 为了减缓这个问题，可以设置下面这个参数no-appendfsync-on-rewrite
110) "no"
111) "slave-serve-stale-data"
112) "yes"
113) "slave-read-only"
114) "yes"
115) "stop-writes-on-bgsave-error"
116) "yes"
117) "daemonize" # Redis默认不是以守护进程的方式运行，可以通过该配置项修改，使用yes启用守护进程
118) "no"
119) "rdbcompression" # 指定存储至本地数据库时是否压缩数据，默认为yes，Redis采用LZF压缩，如果为了节省CPU时间，可以关闭该选项，但会导致数据库文件变的巨大
120) "yes"
121) "rdbchecksum"
122) "yes"
123) "activerehashing"# 指定是否激活重置哈希，默认为开启
124) "yes"
125) "activedefrag"
126) "no"
127) "protected-mode"
128) "yes"
129) "repl-disable-tcp-nodelay"
130) "no"
131) "repl-diskless-sync"
132) "no"
133) "aof-rewrite-incremental-fsync"
134) "yes"
135) "aof-load-truncated"
136) "yes"
137) "aof-use-rdb-preamble"
138) "no"
139) "lazyfree-lazy-eviction"
140) "no"
141) "lazyfree-lazy-expire"
142) "no"
143) "lazyfree-lazy-server-del"
144) "no"
145) "slave-lazy-flush"
146) "no"
147) "maxmemory-policy" # 当内存达到最大值的时候Redis会选择删除哪些数据？有五种方式可供选择; volatile-lru -> 利用LRU算法移除设置过过期时间的key (LRU:最近使用 Least Recently Used ) # allkeys-lru -> 利用LRU算法移除任何key # volatile-random -> 移除设置过过期时间的随机key # allkeys->random -> remove a random key, any key # volatile-ttl -> 移除即将过期的key(minor TTL) # noeviction -> 不移除任何可以，只是返回一个写错误
148) "noeviction"
149) "loglevel" # 指定日志记录级别，Redis总共支持四个级别：debug、verbose、notice、warning，默认为verbose
150) "notice"
151) "supervised"
152) "no"
153) "appendfsync"# 指定更新日志条件，共有3个可选值： no：表示等操作系统进行数据缓存同步到磁盘（快）,always：表示每次更新操作后手动调用fsync()将数据写到磁盘（慢，安全）, everysec：表示每秒同步一次（折衷，默认值）
154) "everysec"
155) "syslog-facility"
156) "local0"
157) "appendonly" # 指定是否在每次更新操作后进行日志记录，Redis在默认情况下是异步的把数据写入磁盘，如果不开启，可能会在断电时导致一段时间内的数据丢失。因为 redis本身同步数据文件是按上面save条件来同步的，所以有的数据会在一段时间内只存在于内存中。默认为no
158) "no"
159) "dir" # 指定本地数据库存放目录
160) "/usr/local/var/db/redis"
161) "save" # 指定在多长时间内，有多少次更新操作，就将数据同步到数据文件，可以多个条件配合 save <seconds> <changes> ;  Redis默认配置文件中提供了三个条件: save 900 1, save 300 10 , save 60 10000; 分别表示900秒（15分钟）内有1个更改，300秒（5分钟）内有10个更改以及60秒内有10000个更改。
162) "900 1 300 10 60 10000"
163) "client-output-buffer-limit"
164) "normal 0 0 0 slave 268435456 67108864 60 pubsub 33554432 8388608 60"
165) "unixsocketperm"
166) "0"
167) "slaveof" # 设置当本机为slav服务时，设置master服务的IP地址及端口，在Redis启动时，它会自动从master进行数据同步, slaveof <masterip> <masterport>
168) ""
169) "notify-keyspace-events"
170) ""
171) "bind" #  绑定的主机地址
172) "127.0.0.1 ::1"
```

## 编辑配置信息

* 直接编辑 `redis.conf`文件 

![redis-config](http://p3q1ykanf.bkt.clouddn.com/201806/redis-config.png)


* 通过 `CONFIG set`命令更新

格式
```shell
127.0.0.1:6379> CONFIG SET CONFIG_SETTING_NAME NEW_CONFIG_VALUE
```


### 相关链接

* [Redis 配置文件 redis.conf 项目详解](http://yijiebuyi.com/blog/bc2b3d3e010bf87ba55267f95ab3aa71.html)
* [Redis 配置 RUNOOB.COM](http://www.runoob.com/redis/redis-conf.html)


