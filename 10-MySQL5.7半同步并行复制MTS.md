# MySQL 5.7��ͬ�����и���MTS


## ͬ���첽��ͬ��

��MySQL5.5��ʼ��MySQL�Բ������ʽ֧�ְ�ͬ�����ơ�


�첽���ƣ�Asynchronous replication��

MySQLĬ�ϵĸ��Ƽ����첽�ģ�������ִ����ͻ����ύ������������������������ͻ��ˣ��������Ĵӿ��Ƿ��Ѿ����ղ����������ͻ���һ�����⣬�����crash���ˣ���ʱ�����Ѿ��ύ��������ܲ�û�д������ϣ������ʱ��ǿ�н�������Ϊ�������ܵ��������ϵ����ݲ�������

 

ȫͬ�����ƣ�Fully synchronous replication��

ָ������ִ����һ���������еĴӿⶼִ���˸�����ŷ��ظ��ͻ��ˡ���Ϊ��Ҫ�ȴ����дӿ�ִ�����������ܷ��أ�����ȫͬ�����Ƶ����ܱ�Ȼ���յ����ص�Ӱ�졣

 

��ͬ�����ƣ�Semisynchronous replication��

�����첽���ƺ�ȫͬ������֮�䣬������ִ����ͻ����ύ������������̷��ظ��ͻ��ˣ����ǵȴ�����һ���ӿ���յ���д��relay log�вŷ��ظ��ͻ��ˡ�������첽���ƣ���ͬ��������������ݵİ�ȫ�ԣ�ͬʱ��Ҳ�����һ���̶ȵ��ӳ٣�����ӳ�������һ��TCP/IP������ʱ�䡣���ԣ���ͬ����������ڵ���ʱ��������ʹ�á�

 ![30.png](pic/30.png)

 
## ��ͬ�����Ƶ�Ǳ������

�ͻ��������ڴ洢������ύ���ڵõ��ӿ�ȷ�ϵĹ����У�����崻��ˣ���ʱ�����ܵ����������

 

1.����û���͵��ӿ���


��ʱ���ͻ��˻��յ������ύʧ�ܵ���Ϣ���ͻ��˻������ύ�������µ����ϣ���崻������������������Դӿ��������¼��뵽�����ӽṹ�У��ᷢ�֣��������ڴӿ��б��ύ�����Σ�һ����֮ǰ��Ϊ����ʱ��һ���Ǳ�����ͬ�������ġ�

 

2.�����Ѿ����͵��ӿ���


��ʱ���ӿ��Ѿ��յ���Ӧ���˸����񣬵��ǿͻ�����Ȼ���յ������ύʧ�ܵ���Ϣ�������ύ�������µ����ϡ�

 

## �����ݶ�ʧ�İ�ͬ������


�������Ǳ�����⣬MySQL 5.7������һ���µİ�ͬ��������Loss-Less��ͬ�����ơ�

 

��Ȼ��֮ǰ�İ�ͬ������ͬ��֧�֣�MySQL 5.7.2������һ���µĲ������п���-`rpl_semi_sync_master_wait_point`

rpl_semi_sync_master_wait_point������ȡֵ

 

`AFTER_SYNC` ������µİ�ͬ��������Waiting Slave dump��Storage Commit֮ǰ��

`AFTER_COMMIT` �ϵİ�ͬ������

 

## ����ʵ�������mysql5.7����GTID�����Ӱ�ͬ��MTSģʽ


Ҫ��ʹ�ð�ͬ�����ƣ������������¼���������

1. MySQL 5.5�����ϰ汾

2. ����have_dynamic_loadingΪYES

3. �첽�����Ѿ�����

![31.png](pic/31.png)

### ��װ���

```shell
mysql> install plugin rpl_semi_sync_master soname 'semisync_master.so';
Query OK, 0 rows affected (0.10 sec)

mysql> install plugin rpl_semi_sync_slave soname 'semisync_slave.so';
Query OK, 0 rows affected (0.05 sec)
```

### �����ļ�

```shell
## master
	[mysqld]
	# AB replication
	server-id=1
	log-bin=/var/lib/mysql-log/masterb

	# GTID
	gtid_mode=on
	enforce_gtid_consistency=1

	# crash safe
	sync_binlog=1
	innodb_flush_log_at_trx_commit=1

	# ��ͬ��ģʽ
	rpl_semi_sync_master_enabled=1
	rpl_semi_sync_master_timeout=1000

## slave
	[mysqld]
	# AB replication
	server-id=2

	# open gtid mode
	gtid_mode=on
	enforce_gtid_consistency=1

	# slave crash
	relay_log_recovery=1

	# multisource
	master-info-repository=table
	relay-log-info-repository=table

	# MTS һ��һ�߳�
	slave-parallel-type=logical_clock
	slave-parallel-workers=16

	# ��ͬ��ģʽ
	rpl_semi_sync_slave_enabled=1
```

### ���԰�ͬ�����Ƴ�ʱ���Զ��л����첽ģʽ


```shell
mysql> create database db1;
Query OK, 1 row affected (0.06 sec)

mysql> create table db1.t1 (id int);
Query OK, 0 rows affected (0.26 sec)

mysql> insert into db1.t1 values (1);
Query OK, 1 row affected (0.23 sec)
# ��slave�رպ���ִ�����²���
mysql> insert into db1.t1 values (2);
Query OK, 1 row affected (1.19 sec)

mysql> insert into db1.t1 values (3);
Query OK, 1 row affected (0.36 sec)

mysql> insert into db1.t1 values (4);
Query OK, 1 row affected (0.08 sec)
```

����ӻ���ص����첽ģʽ����Ҫ����slave������Ĭ�ϻ����첽���ơ�


### ��������Ƿ������ڰ�ͬ������ģʽ��

* `show status like 'rpl_semi_sync_master_status';`
* `show status like 'rpl_semi_sync_slave_status';`


### ��������

```shell
mysql> show variables like '%Rpl%';
+-------------------------------------------+------------+
| Variable_name                             | Value      |
+-------------------------------------------+------------+
| rpl_semi_sync_master_enabled              | ON         |
| rpl_semi_sync_master_timeout              | 10000      |
| rpl_semi_sync_master_trace_level          | 32         |
| rpl_semi_sync_master_wait_for_slave_count | 1          |
| rpl_semi_sync_master_wait_no_slave        | ON         |
| rpl_semi_sync_master_wait_point           | AFTER_SYNC |
| rpl_stop_slave_timeout                    | 31536000   |
+-------------------------------------------+------------+
7 rows in set (0.30 sec)
```

`rpl_semi_sync_master_wait_for_slave_count`

MySQL 5.7.3����ģ��ñ�����������Ҫ�ȴ����ٸ�slaveӦ�𣬲��ܷ��ظ��ͻ��ˣ�Ĭ��Ϊ1��

 

`rpl_semi_sync_master_wait_no_slave`

ON

Ĭ��ֵ����״̬����Rpl_semi_sync_master_clients�е�ֵС��rpl_semi_sync_master_wait_for_slave_countʱ��Rpl_semi_sync_master_status������ʾΪON��

OFF

��״̬����Rpl_semi_sync_master_clients�е�ֵ��rpl_semi_sync_master_wait_for_slave_countʱ��Rpl_semi_sync_master_status������ʾΪOFF�����첽���ơ�

˵��ֱ��һ�㣬����ҵļܹ���1��2�ӣ�2���Ӷ������˰�ͬ�����ƣ������õ���rpl_semi_sync_master_wait_for_slave_count=2���������һ���ҵ��ˣ�����rpl_semi_sync_master_wait_no_slave����ΪON���������ʱ��ʾ����Ȼ�ǰ�ͬ�����ƣ����rpl_semi_sync_master_wait_no_slave����ΪOFF��������̱���첽���ơ�

 

### ״̬����

```shell
mysql> show status like '%Rpl_semi%';
+--------------------------------------------+-------+
| Variable_name                              | Value |
+--------------------------------------------+-------+
| Rpl_semi_sync_master_clients               | 1     |
| Rpl_semi_sync_master_net_avg_wait_time     | 0     |
| Rpl_semi_sync_master_net_wait_time         | 0     |
| Rpl_semi_sync_master_net_waits             | 6     |
| Rpl_semi_sync_master_no_times              | 1     |
| Rpl_semi_sync_master_no_tx                 | 1     |
| Rpl_semi_sync_master_status                | ON    |
| Rpl_semi_sync_master_timefunc_failures     | 0     |
| Rpl_semi_sync_master_tx_avg_wait_time      | 1120  |
| Rpl_semi_sync_master_tx_wait_time          | 4483  |
| Rpl_semi_sync_master_tx_waits              | 4     |
| Rpl_semi_sync_master_wait_pos_backtraverse | 0     |
| Rpl_semi_sync_master_wait_sessions         | 0     |
| Rpl_semi_sync_master_yes_tx                | 4     |
+--------------------------------------------+-------+
14 rows in set (0.00 sec)
```
 

����״̬�����У��Ƚ���Ҫ�������¼���

 

`Rpl_semi_sync_master_clients`

��ǰ��ͬ�����ƴӵĸ����������һ����ӵļܹ������������첽���ƴӵĸ�����

 

`Rpl_semi_sync_master_no_tx`

The number of commits that were not acknowledged successfully by a slave.

���嵽����Ĳ����У�ָ����insert into test.test values(2)�������

 

`Rpl_semi_sync_master_yes_tx`

The number of commits that were acknowledged successfully by a slave.



 

## �ܽ�

1. ��һ����ӵļܹ��У����Ҫ������ͬ�����ƣ�����Ҫ�����еĴӶ��ǰ�ͬ�����ơ�

2. MySQL 5.7����������˰�ͬ�����Ƶ����ܡ�

    5.6�汾�İ�ͬ�����ƣ�dump thread �е������ݲ�ͬ����ʮ��Ƶ�������񣺴���binlog ��slave ������Ҫ�ȴ�slave������Ϣ�����������������Ǵ��еģ�dump thread ����ȴ� slave ����֮��Żᴫ����һ�� events ����dump thread ��Ȼ��Ϊ������ͬ��������ܵ�ƿ�����ڸ߲���ҵ�񳡾��£������Ļ��ƻ�Ӱ�����ݿ������TPS ��

    5.7�汾�İ�ͬ�������У�������һ�� ack collector thread ��ר�����ڽ���slave �ķ�����Ϣ������master ���������̶߳�������������ͬʱ����binlog ��slave ���ͽ���slave�ķ����� 