
# mpstat
print standard output activities for each available processor
```
# -P ALL  Indicate all processors
# 1 interval every 1s

mpstat -P ALL 1
```

# pidstat
monitoring individual tasks currently being managed by the Linux kernel
```
pidstat 1
```

# atop
advance top

# iftop
display bandwidth usage on an interface
```
# -o 10s  Sort by second column (10s traffic average)
# -n  don't do hostname lookups

iftop -o 10s -n
```