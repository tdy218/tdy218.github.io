---
layout: post
title:  "Linux操作系统监控oom-killer事件的一种方法"
date:   2023-07-20 16:09:51 +0800
comments: true
categories: [linux]

---

```shell
root# vi /etc/rsyslog.d/30-oom-killer.conf

#module(load="omprog")

if ($msg contains ['Killed process', 'Out of memory']) then {
    /root/oom-killer.log
    ^/path/to/script.sh
    #action(type="omprog"
    #   binary="/path/to/script.sh param1 param2"
    #   template="RSYSLOG_TraditionalFileFormat")
    stop
}

root# systemctl restart rsyslog
```

这样当系统发生 oom-killer 事件的时候，会实时写日志到 /root/oom-killer.log 文件并执行 /path/to/script.sh 脚本。

/root/oom-killer.log 文件中记录着 if 条件匹配到的两条日志：

```log
Jul 20 16:06:01 iZbp1cqjat6f9k19qkvfxsZ kernel: Out of memory: Kill process 25196 (java) score 669 or sacrifice child
Jul 20 16:06:01 iZbp1cqjat6f9k19qkvfxsZ kernel: Killed process 25196 (java), UID 0, total-vm:3330072kB, anon-rss:1296092kB, file-rss:0kB
, shmem-rss:0kB
```

/path/to/script.sh 脚本的 $1 变量就是 $msg 的内容。

