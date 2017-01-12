# MySQL 5.7���и���MTS

* MySQL 5.5 ֻ��һ��sql�߳̽�������
* MySQL 5.6 һ��һ�߳� 
* MySQL 5.7 һ��һ�߳�

![32.png](pic/32.png)

MySQL 5.7�ſɳ�Ϊ�����Ĳ��и��ƣ���������Ϊ��Ҫ��ԭ�����slave�������Ļط���������һ�µļ�master������������ô����ִ�е�slave�Ͼ��������в��лطš�

MTS: Prepared transactions slave parallel applier 

�ò��и��Ƶ�˼����������MariaDB��Kristain�����������MariaDB 10�г��֣����źܶ�ѡ��MariaDB��С�����Ϊ���صĹ���֮һ���ǲ��и��ơ�

MySQL 5.7���и��Ƶ�˼����׶���һ���Ա�֮��һ�����ύ�������ǿ��Բ��лطţ���Ϊ��Щ�����ѽ��뵽�����prepare�׶Σ���˵������֮��û���κγ�ͻ������Ͳ������ύ����


## ��������ģʽ`slave-parallel-type`

Ϊ�˼���MySQL 5.6���ڿ�Ĳ��и��ƣ�5.7�������µı���`slave-parallel-type`����������õ�ֵ�У�

* DATABASE��Ĭ��ֵ�����ڿ�Ĳ��и��Ʒ�ʽ
* LOGICAL_CLOCK���������ύ�Ĳ��и��Ʒ�ʽ


֧�ֲ��и��Ƶ�GTID

last_committed

sequence_number

���и������������
master_info_repository

## ���������߳���`slave_parallel_workers`

* slave_parallel_workers����Ϊ0����MySQL 5.7�˻�Ϊԭ���̸߳���
* slave_parallel_workers����Ϊ1����SQL�̹߳���ת��Ϊcoordinator�̣߳�����ֻ��1��worker�߳̽��лطţ�Ҳ�ǵ��̸߳��ơ�Ȼ��������������ȴ����һЩ��������Ϊ����һ��coordinator
�̵߳�ת�������slave_parallel_workers=1�����ܷ�����0��Ҫ��

## Enhanced Multi-Threaded Slave�����ܽ�

# slave
slave-parallel-type=LOGICAL_CLOCK
slave-parallel-workers=16
master_info_repository=TABLE
relay_log_info_repository=TABLE
relay_log_recovery=ON

## ����ʵ������� MySQL 5.7 ����GTID��MTS���и���

![29.png](pic/29.png)


### �����װ

|host|ip|software|
|:--|:--|:--|
|masterb|	172.25.0.12|	mysql-community-server-5.7|
|slaveb|	172.25.0.14|	mysql-community-server-5.7|


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
```

## �ܽ�

MySQL 5.7�Ƴ���Enhanced Multi-Threaded Slave���������MySQL������ʮ��ĸ����ӳ�����
