# Geting general redis instance
## info
several blocks of information and statistics about the server
## monitor
to see a constant stream of every command processed by the Redis server

# Cache debug
Total Keys
```
127.0.0.1:6389> DBSIZE
# (integer) 343585
# The total count of keys are 342112
```
Hit Ratio
```
127.0.0.1:6389> info
# Find
# keyspace_hits:12141853
# keyspace_misses:36914043
# Then then cache hit rate is: 12141853/(12141853+36914043) = 24.75% (very bad)
```
Big Keys
```
$ /usr/local/bin/redis-cli -h 127.0.0.1 -p 6389 –bigkeys
# If the keys is too big, consume too much space, may need to redesign about it.
```
Show All the keys (Not really need when the keys are huge)
```
$ /usr/local/bin/redis-cli -h 127.0.0.1 -p 6389 --scan --pattern '*'
# we can see all the keys detail
```
Check Key TTL(expire time)
```
127.0.0.1:6389> TTL mykey
# If the key has the TTL set, will remove from the Redis cache when time up. Could cause the missing ache when search
127.0.0.1:6389> info
# Find
# expired_keys:6856
# means we have 6856 keys has expired since Redis start
```
Slow Log
```
127.0.0.1:6389> SLOWLOG GET 10
# Get 10 slow cache access, for example:
# 1) (integer) 381  # Unique ID
# 2) (integer) 1493366877 # Unix timestamp, convert to human time is about 07:50:19:285
# 3) (integer) 38021 # Execution time in microseconds, is 0.038021 seconds
# 4) 1) "DEL"
#    2) "Dashboard-SearchResultsData:\xac\xed\x00\x05sr\x003com.equilar.international.model.dashboard.P4PSearch':
#       \x96Ro\xa6\x10\x1b\x02\x00\x1eL\x00\nbeginIndext\x00\x13Ljava/lang/Int... (2056 more bytes)"
# In this case, use 0.038021 seconds, it is bad when response time > 10 ms
```
Evicted Key
```
127.0.0.1:6389> info
# Find
# evicted_keys:27764495
# means we have 27764495 keys evicted due to the cache hit the memory limit, replace by Redis algorithm(LRU)
# Find
# used_memory_peak:4307511872
# used_memory_peak_human:4.01G
# we use max 4.01G memory, that's the our Redis total memory, need to increase the psychical memory for Redis.
```

# Memory check
## memory usage
tells how much memory is currently being used by a single key.
```
127.0.0.1:6379> memory usage <keyofsomething>
```
## memory stats
general understanding of how your Redis server is using memory
```
127.0.0.1:6379> memory stats
```
* `peak.allocated`: The peak number of bytes consumed by Redis
* `total.allocated`: The total number of bytes allocated by Redis
* `startup.allocated`: The initial number of bytes consumed by Redis at startup
* `replication.backlog`: The size of the replication backlog, in bytes
* `clients.slaves`: The total size of all replica overheads (the output and query buffers and connection contexts)
* `clients.normal`: The total size of all client overheads
* `aof.buffer`: The total size of the current and rewrite append-only file buffers
* `db.0`: The overheads of the main and expiry dictionaries for each database in use on the server, reported in bytes
* `overhead.total`: The sum of all overheads used to manage Redis’s keyspace
* `keys.count`: The total number of keys stored in all the databases on the server
* `keys.bytes-per-key`: The ratio of the server’s net memory usage and keys.count
* `dataset.bytes`: The size of the dataset, in bytes
* `dataset.percentage`: The percentage of Redis’s net memory usage taken by dataset.bytes
* `peak.percentage`: The percentage of peak.allocated taken out of total.allocated
* `fragmentation`: The ratio of the amount of memory currently in use divided by the physical memory Redis is actually using
## memory malloc-stats
provides an internal statistics report from jemalloc, the memory allocator used by Redis on Linux systems:
```
127.0.0.1:6379> memory malloc-stats
```
## memory doctor
This feature will output any memory consumption issues that it can find and suggest potential solutions.

## Evicted Key
```
127.0.0.1:6389> info
# Find
# used_memory:3538196240 -> used memory in bytes
# used_memory_human:3.30G -> same as used_memory
# used_memory_rss:3792228352 -> actually memory used in bytes
# used_memory_peak:4307511872 -> max memory usage in bytes, if redis use more memory than it's config, will start to swap the physical memory, which will cause performance go down
# used_memory_peak_human:4.01G -> same as used_memory_peek
# mem_fragmentation_ratio:1.07 -> = used_memory_rss/used_memory,
# >1.5 means too much fragment memory, Redis waste too much memory,
# <1.0 means some cache missing, which should not happened
```
# Redis Client Connection Check
## Client list
```
127.0.0.1:6389> client list
# For Example:
# id=231876 addr=10.1.28.134:49916 fd=21 name= age=428055 idle=428034 flags=N db=0 sub=0 psub=0 multi=-1 qbuf=0 qbuf-free=0 obl=0 oll=0 omem=0 events=r cmd=get
# id: unique client id, increase by 1 when has new connect. Reset to 1 when Redis reboot
# addr: client ip
# fd: description mark for socket, same as the FD in lsof
# name: client name
# age: client live time (seconds)
# idle: client idle time (seconds)
# flages: client type, has 13 in total. Common type: N(normal client), M(master), S(Slave), O(monitor)
# db: the database series number client use
# sub/psub: subscription channel/mode
# multi: how mand commands has run in current transaction
# qbuf: query buffer bytes (important)
# qbuf-free: query buffer free bytes (important)
# omem: changeable output buffer memory bytes (important)
# events: event file description (r/w)
# cmd: last command client run, not include parameter
# 
# client connection will consume the Redis memory, so make sure qbuf + qbuf-free and omem are in the reasonable range
```
```
127.0.0.1:6389> info
# Find
# blocked_clients = 0
# If any client was blocked(BLPOP, BRPOP, BRPOPLPUSH), that's not right

```

# Statistic
## linux iftop
can view live network input/output to the Redis Server
```
$ iftop -o 10s -n
```
# Tools
[Redis-stat](https://github.com/junegunn/redis-stat)
[Redis-live](https://github.com/nkrode/RedisLive)
