# MySQL�߼����ݹ���mysqldump,mysqlpump,mydumper

[TOC]

## mysqldump

mysql�����߼����ݹ���

����˵��������Ĳ���������mysqldump --help�鿴��

�Ƚ���Ҫ�ļ�������

�٣�--single-transaction

ͨ��������������װ��һ������(Repeatable Read)����ʹ�õ�����������һ��һ���Կ��ա�ֻ�е���ʹ��֧��MVCC�Ĵ洢���棨Ŀǰֻ��InnoDB��ʱ�ſ��Թ������������治�ܱ�֤������һ�µġ������������˨Csingle-transactionѡ��ʱ��Ҫȷ�������ļ���Ч����ȷ�ı����ݺͶ�������־λ�ã�����Ҫ��֤û���������ӻ�ִ��������䣺ALTER TABLE, DROP TABLE, RENAME TABLE, TRUNCATE TABLE����ᵼ��һ���Կ���ʧЧ�����ѡ�������Զ��ر�lock-tables��������mysql5.7.11֮ǰ��--default-parallelism����1��ʱ��ʹ˲�Ҳ���⣬����ʹ��--default-parallelism=0��5.7.11֮������--single-transaction��--default-parallelism�Ļ������⡣

�ڣ�--master-data

���ѡ����԰�binlog��λ�ú��ļ�����ӵ�����У��������1�������ӡ��һ��CHANGE MASTER����������2�������ע��ǰ׺���������ѡ����Զ��򿪨Clock-all-tables������ͬʱ�����˨Csingle-transaction����������£�ȫ�ֶ���ֻ���ڿ�ʼdump��ʱ�����һС��ʱ�䣬��Ҫ�����Ķ��Csingle-transaction�Ĳ��֣������κ�����£�������־�еĲ������ᷢ���ڵ�����׼ȷʱ�̡����ѡ����Զ��رըClock-tables���򿪸ò�����Ҫ��reloadȨ�ޣ����ҷ���������binlog��

�ۣ�--lock-all-tables ��-x

�������п������еı�����ͨ��������dump�Ĺ����г���ȫ�ֶ�����ʵ�ֵġ����Զ��رըCsingle-transaction �� �Clock-tables��

�ܣ�--lock-tables��-l

����ĳ��������ÿ�����б���READ LOCAL������MyISAM������д�룬��Ϊ����ֻ���ָ�������ݿ⣬���ܱ�֤�����ϵ�һ���ԣ���ͬ��ı������ʱ���в�ͬ��״̬���èCskip-lock-tables���رա�

�ݣ�--flush-logs��-F

�ڿ�ʼ����ǰˢ�·���������־�ļ���ע�⣬�����һ���Ե����ܶ����ݿ⣨ʹ��--databases= ��--all-databases ѡ�������ÿ����ʱ���ᴥ����־ˢ�¡������ǵ�ʹ����--lock-all-tables��--master-data��--single-transactionʱ����־ֻ�ᱻˢ��һ�Σ��Ǹ�ʱ�����б��ᱻ��ס�����������ϣ����ĵ�������־ˢ�·�����ͬһ��ȷ����ʱ�̣�����Ҫʹ��--lock-all-tables��--master-data��--single-transaction��� �Cflush-logs��

�ޣ�--opt

�ò���Ĭ�Ͽ�������ʾ�������--add-drop-table --add-locks --create-options --disable-keys --extended-insert --lock-tables --quick --set-charsetѡ�ͨ�� --skip-opt �رա�


## mysqlpump

MySQL5.7֮�����һ�����ݹ��ߣ�mysqlpump������mysqldump��һ��������mysqldump�Ͳ���˵���ˣ����ڿ���mysqlpump����������Щ���������Բ鿴�ٷ��ĵ�������������ʹ������˵����

mysqlpump��mysqldumpһ���������߼����ݣ�������SQL��ʽ���ı����档�߼�������������ݵĺô��ǲ�����undo log�Ĵ�С��ֱ�ӱ������ݼ��ɡ�������Ҫ���ص��ǣ�

- ���б������ݿ�����ݿ��еĶ���ģ��ӿ챸�ݹ��̡�
- ���õĿ������ݿ�����ݿ���󣨱��洢���̣��û��ʻ����ı��ݡ�
- �����û��˺���Ϊ�ʻ�������䣨CREATE USER��GRANT����������ֱ�Ӳ��뵽MySQL��ϵͳ���ݿ⡣
- ���ݳ���ֱ������ѹ����ı����ļ���
- ���ݽ���ָʾ������ֵ����
- ���¼��أ���ԭ�������ļ����Ƚ��������������������������������ά��������
- �ӿ��˻�ԭ�ٶȡ�
- ���ݿ����ų�����ָ�����ݿ⡣

### ����

���󲿷ֲ�����mysqldumpһ��

1��--add-drop-database���ڽ�����֮ǰ��ִ��ɾ�������

DROP DATABASE IF EXISTS `...`;

2��--add-drop-table���ڽ���֮ǰ��ִ��ɾ�������

DROP TABLE IF EXISTS `...`.`...`;

3��--add-drop-user����CREATE USER���֮ǰ����DROP USER��ע�⣺���������Ҫ��--usersһ��ʹ�ã����߲���Ч��

DROP USER 'backup'@'192.168.123.%';

4��--add-locks�����ݱ�ʱ��ʹ��LOCK TABLES��UNLOCK TABLES��ע�⣺���������֧�ֲ��б��ݣ���Ҫ�رղ��б��ݹ��ܣ�--default-parallelism=0 

```shell
LOCK TABLES `...`.`...` WRITE;
...
UNLOCK TABLES;
```

5��--all-databases���������п⣬-A��

6��--bind-address��ָ��ͨ���ĸ�����ӿ�������Mysql��������һ̨�����������ж��IP������ֹͬһ��������ȥӰ��ҵ��

7��--complete-insert��dump�����������е�����insert��䡣

8��--compress�� ѹ���ͻ��˺ͷ�������������е����ݣ�-C��

9��--compress-output��Ĭ�ϲ�ѹ�������Ŀǰ����ʹ�õ�ѹ���㷨��LZ4��ZLIB��

```shell
shell> mysqlpump --compress-output=LZ4 > dump.lz4
shell> lz4_decompress dump.lz4 dump.txt

shell> mysqlpump --compress-output=ZLIB > dump.zlib
shell> zlib_decompress dump.zlib dump.txt
```

10��--databases���ֶ�ָ��Ҫ���ݵĿ⣬֧�ֶ�����ݿ⣬�ÿո�ָ���-B��

11��--default-character-set��ָ�����ݵ��ַ�����

12��--default-parallelism��ָ�������߳�����Ĭ����2��������ó�0����ʾ��ʹ�ò��б��ݡ�ע�⣺ÿ���̵߳ı��ݲ����ǣ���create table��������������������������create tableʱ����������д�����ݣ����������������

13��--defer-table-indexes���ӳٴ���������ֱ���������ݶ�������֮���ٴ���������Ĭ�Ͽ��������ر�����mysqldumpһ�����ȴ���һ����������������ٵ������ݣ���Ϊ�ڼ��ػ�ԭ���ݵ�ʱ��Ҫά�����������Ŀ���������Ч�ʱȽϵ͡��ر�ʹ�ò�����--skip--defer-table-indexes��

14��--events���������ݿ���¼���Ĭ�Ͽ������ر�ʹ��--skip-events������

15��--exclude-databases�������ų��ò���ָ�������ݿ⣬����ö��ŷָ������ƵĻ���--exclude-events��--exclude-routines��--exclude-tables��--exclude-triggers��--exclude-users��

```shell
mysqlpump --exclude-databases=mysql,sys    #���ݹ���mysql��sys���ݿ�

mysqlpump --exclude-tables=rr,tt   #���ݹ����������ݿ���rr��tt��

mysqlpump -B test --exclude-tables=tmp_ifulltext,tt #���ݹ���test���е�rr��tt��
```

ע�⣺Ҫ��ֻ�������ݿ���˺ţ���Ҫ��Ӳ���--users��������Ҫ���˵����е����ݿ⣬�磺

mysqlpump --users --exclude-databases=sys,mysql,db1,db2 --exclude-users=dba,backup  #���ݳ�dba��backup�������˺š�

16��--include-databases��ָ���������ݿ⣬����ö��ŷָ������ƵĻ���--include-events��--include-routines��--include-tables��--include-triggers��--include-users�����·���ʹ��ͬ15��

17��--insert-ignore��������insert ignore������insert��䡣

18��--log-error-file�����ݳ��ֵ�warnings��erros��Ϣ�����һ��ָ�����ļ���

19��--max-allowed-packet������ʱ����client/serverֱ��ͨ�ŵ����buffer���Ĵ�С��

20��--net-buffer-length������ʱ����client/serverͨ�ŵĳ�ʼbuffer��С�����������в�������ʱ��mysqlpump �����е�N���ֽڳ���

21��--no-create-db�����ݲ�дCREATE DATABASE��䡣Ҫ�Ǳ��ݶ���⣬��Ҫʹ�ò���-B����ʹ��-B��ʱ������create database��䣬�ò�����������create database ��䡣

22��--no-create-info�����ݲ�д������䣬�������ݱ�ṹ��ֻ�������ݣ�-t��

23��--hex-blob�� ����binary�ֶε�ʱ��ʹ��ʮ�����Ƽ���������Ӱ����ֶ�������BINARY��VARBINARY��BLOB��BIT��

24��--host ������ָ�������ݿ��ַ��-h��

25��--parallel-schemas=[N:]db_list��ָ�����б��ݵĿ⣬������ö��ŷָ������ָ����N����ʹ��N���̵߳ĵض��У����N��ָ�������� --default-parallelism��ȷ��N��ֵ���������ö��--parallel-schemas��

```shell
mysqlpump --parallel-schemas=4:vs,aa --parallel-schemas=3:pt   #4���̱߳���vs��aa��3���̱߳���pt��ͨ��show processlist ���Կ�����7���̡߳�

mysqlpump --parallel-schemas=vs,abc --parallel-schemas=pt  #Ĭ��2���̣߳���2���̱߳���vs��abc��2���̱߳���pt
####��ȻҪ��Ӳ��IO������Ļ��������ٿ������̺߳����ݿ���в��б���
```

26��--password��������Ҫ�����롣

27��--port ���������ݿ�Ķ˿ڡ�

28��--protocol={TCP|SOCKET|PIPE|MEMORY}��ָ�����ӷ�������Э�顣

29��--replace�����ݳ���replace into��䡣

30��--routines�����ݳ��������洢���̺ͺ�����Ĭ�Ͽ�������Ҫ�� mysql.proc���в鿴Ȩ�ޡ����ɵ��ļ��л����CREATE PROCEDURE �� CREATE FUNCTION��������ڻָ����ر�����Ҫ��--skip-routines������

31��--triggers�����ݳ���������������Ĭ�Ͽ�����ʹ��--skip-triggers���رա�

31��--set-charset�������ļ���дSET NAMES default_character_set ��������˲�Ĭ�Ͽ����� -- skip-set-charset���ô˲����������ڱ����ļ�����д��set names...

32��--single-transaction���ò�����������뼶�����ó�Repeatable Read������dump֮ǰ����start transaction ��������ˡ�����ʹ��innodbʱ�����ã���Ϊ�ڷ���start transactionʱ����֤���ڲ������κ�Ӧ���µ�һ����״̬����myisam��memory�ȷ���������ǻ�ı�״̬�ģ���ʹ�ô˲ε�ʱ��Ҫȷ��û������������ʹ��ALTER TABLE��CREATE TABLE��DROP TABLE��RENAME TABLE��TRUNCATE TABLE����䣬�������ֲ���ȷ�����ݻ���ʧ�ܡ�--add-locks�ʹ˲λ��⣬��mysql5.7.11֮ǰ��--default-parallelism����1��ʱ��ʹ˲�Ҳ���⣬����ʹ��--default-parallelism=0��5.7.11֮������--single-transaction��--default-parallelism�Ļ������⡣

33��--skip-definer��������Щ������ͼ�ʹ洢�����õ��� DEFINER �� SQL SECURITY ��䣬�ָ���ʱ�򣬻�ʹ��Ĭ��ֵ��������ڻ�ԭ��ʱ�򿴵�û��DEFINER����ʱ���˺Ŷ�����

34��--skip-dump-rows��ֻ���ݱ�ṹ�����������ݣ�-d��ע�⣺mysqldump֧��--no-data��mysqlpump��֧��--no-data

35��--socket���������ӵ�localhost��Unixʹ���׽����ļ�����Windows���������ܵ�������ʹ�ã�-S��

36��--ssl��--ssl������Ҫ��ȥ������--ssl-modeȡ��������ssl��صı��ݣ��뿴�ٷ��ĵ���

37��--tz-utc������ʱ���ڱ����ļ�����ǰ�������SET TIME_ZONE='+00:00'��ע�⣺�����ԭ�ķ���������ͬһ��ʱ�����һ�ԭ���е�����timestamp�ֶΣ��ᵼ�»�ԭ�����Ľ����һ�¡�Ĭ�Ͽ����ò������� --skip-tz-utc���رղ�����

38��--user������ʱ����û�����-u��

39��--users���������ݿ��û������ݵ���ʽ��CREATE USER...��GRANT...��ֻ�������ݿ��˺ſ���ͨ���������

mysqlpump --exclude-databases=% --users    #���˵��������ݿ�

40��--watch-progress��������ʾ���ȵ���ɣ������������к��������󡣸ò���Ĭ�Ͽ�������--skip-watch-progress���رա�

### ʹ��˵��

mysqlpump�ļܹ�����ͼ��ʾ��

![](pic/71.png)

mysqlpump֧�ֻ��ڿ�ͱ�Ĳ��е�����mysqlpump�Ĳ��е������ܵļܹ�Ϊ������+�̣߳������ж�����У�--parallel-schemas������ÿ���������ж���̣߳�N��������һ�����п��԰�1�����߶�����ݿ⣨���ŷָ�����mysqlpump�ı����ǻ��ڱ��еģ�����ÿ�ű�ĵ���ֻ���ǵ����̵߳ģ�������и����������ĳ�����ݿ���һ�ű�ǳ��󣬿��ܴ󲿷ֵ�ʱ�䶼�������������ı������棬���б��ݵ�Ч�����ܾͲ����ԡ������������mydumper������chunk�ķ�ʽ������������mydumper֧��һ�ű����߳���chunk�ķ�ʽ�������������������mysqldump�������˺ܴ��������������²�����mysqlpump��mysqldump�ı���Ч�ʡ� 

```shell
#mysqlpumpѹ������vs���ݿ� ���������̱߳��ݣ�����ʱ�䣺222s
mysqlpump -uzjy -p -h192.168.123.70 --single-transaction --default-character-set=utf8 --compress-output=LZ4 --default-parallelism=3 -B vs > /home/zhoujy/vs_db.sql.lz4

#mysqldump����ѹ��vs���ݿ� �����̱߳��ݣ�����ʱ�䣺900s��gzip��ѹ���ʱ�LZ4�ĸ�
mysqldump -uzjy -p -h192.168.123.70 --default-character-set=utf8 -P3306 --skip-opt --add-drop-table --create-options  --quick --extended-insert --single-transaction -B vs | gzip > /home/zhoujy/vs.sql.gz

#mydumper����vs���ݿ� ���������̱߳��ݣ�����ʱ�䣺300s��gzip��ѹ���ʱ�LZ4�ĸ�
mydumper -u zjy -p  -h 192.168.123.70 -P 3306 -t 3 -c -l 3600 -s 10000000 -B vs -o /home/zhoujy/vs/

#mydumper����vs���ݿ⣬��������̱߳��ݣ����ҿ�����һ�ű����߳���chunk�ķ�ʽ����������-r������ʱ�䣺180s
mydumper -u zjy -p  -h 192.168.123.70 -P 3306 -t 5 -c -r 300000 -l 3600 -s 10000000 -B vs -o /home/zhoujy/vs/
```

�����濴����mysqlpump�ı���Ч�������ģ�mydumper��֮��mysqldump��������IO���������£����ö��߳̾ͱ��õ��̱߳��ݡ�����mysqlpump��֧�ֶ����ݿ�Ĳ��б��ݣ���mydumperҪô����һ���⣬Ҫô�ͱ������п⡣�������Oracle�ٷ������߼����ݹ���mysqlpump��ƪ���µĲ��Խ��Ҳ˵����mysqlpump��mysqldump�Ĳ��Ժá�����ʵ�������ͬ�����Ը������ٶ�����ֻ�ǲο������׿������ٸ����б��ݵ��̣߳����������IO�ĳ������������÷�����ֻ���б������񣬿���������Ƶ������ô��̡�

�ܽ᣺

mysqldump��mysqlpump��ʹ�÷������󲿷�һ�£����������ֹ��߱������ݿ����Ҫ�ھ���Ļ����²�������ѡ����Щʱ������������ݸ��ã�xtrabackup������֮������Ҫ���в��ԣ�����پ���ʹ�����ֱ��ݹ��߽��б��ݡ�

--

## mydumper

mydumper��&myloader�������ڶ�MySQL���ݿ���ж��̱߳��ݺͻָ��Ŀ�Դ (GNU GPLv3)���ߡ�������Ա��Ҫ����MySQL��Facebook��SkySQL��˾��Ŀǰ��Percona��˾������ά������PerconaRemote DBA��Ŀ����Ҫ��ɲ��֣�������Percona XtraDB Cluster�С�mydumper�ĵ�һ��0.1������2010.3.26�����°汾0.9.1������2015.11.06��

mydumper��һ�ֱ�MySQL�ٷ�mysqldump������ı��ݹ��ߣ���Ҫ�����ڶ��̺߳ͱ����ļ����淽ʽ�ϡ���MySQL 5.7�汾�У��ٷ�������һ���µı��ݹ���mysqlpump��Ҳ�Ƕ��̵߳ģ���ʵ�ַ�ʽ���˶�Ŀһ�µĸо������ź���������Ϊ����Ĳ��С���mydumper�ܹ�ʵ�ּ�¼����Ĳ��б��ݣ��䱸�ݿ�������̺߳Ͷ�������߳����.

����һ��������ָ������ĳ��ʱ��㣬�����������뵼����Binlog�ļ���Ϣ��ƥ�䣬��������˶��ű�����ݣ���Щ��ͬ��֮������ݶ���ͬһ��ʱ�������ݡ�

### mydumper����ԭ��

��mydumper���б��ݵ�ʱ����һ�����߳��Լ���������߳���ɡ������̵߳������ǣ�

1. �������ݿ�
2. FLUSH TABLES WITH READ LOCK ����ҳˢ�µ����̲����ֻ����
3. START TRANSACTION /!40108 WITH CONSISTENT SNAPSHOT / �������ﲢ��ȡһ���Կ���
4. SHOW MASTER STATUS ���binlog��Ϣ
5. �������̲߳��������ݿ�
6. Ϊ���̷߳�������push��������
7. UNLOCK TABLES / FTWRL / �ͷ���

���̵߳���Ҫ�����ǣ�

1. �������ݿ�
2. SET SESSION TRANSACTION ISOLATION LEVEL REPEATABLE
3. START TRANSACTION /!40108 WITH CONSISTENT SNAPSHOT /
4. �Ӷ�����pop����ִ��

���������̵߳����̵Ĺ�ϵ��ͼ

![](pic/72.png)

��ͼ�п��Կ��������߳��ͷ����������߳̿�������֮�������Ǳ�֤���̻߳�õ�����һ��Ϊһ�������ݵĹؼ���

���߳������ӵ����ݿ������ͨ��Flush tables with read lock(FTWRL) ��������ҳˢ�µ����̣�����ȡһ��ȫ�ֵ�ֻ��������������Ա�֤�����ͷ�֮ǰ�����߳̿�����������һ�µġ�Ȼ������ͨ�� Start Transaction with consistent snapshot ����һ�����ն������ͨ�� show master status��ȡbinlogλ����Ϣ��

Ȼ�󴴽����dump��������̲߳�Ϊ���������

���߳��ڴ������̺߳�ͨ��һ���첽��Ϣ���� ready �ȴ����߳�׼����ϡ� ���߳��ڴ���������������MySQL���ݿ�����ӣ�Ȼ�����õ�ǰ������뼶��ΪRepeatable Read��

�������֮��ʼ���ն������������һϵ�в���֮�����̲߳Ż�ͨ��ready���и��������Լ���׼����ϡ����̵߳ȴ�ȫ�����߳�׼����Ͽ���һ���Զ�Snapshot�����Ż��ͷ�ȫ��ֻ������Unlock Table����

���ֻ��Innodb����ôֻ���ڴ�������׶λ�����������������MyIsam�������������MVCC���ܵı���ô����Щ��ĵ����������֮ǰ���������Щ����м�����Mydumper����ά����һ�� non_innodb_table �б��ڴ�������׶λ�����Ϊ��Innodb��������ͬʱ��ά����һ��ȫ�ֵ�unlock_table�����Լ�һ��ԭ�Ӽ����� non_innodb_table_counter , ���߳�ÿ���һ����Innodb�������㽫 non_innodb_table_counter ��һ�����non_innodb_table_counter ֵΪ0 ��ͨ���� unlock_table ����pushһ����Ϣ�ķ�ʽ֪ͨ���߳�����˷�Innodb��ĵ����������ִ�� unlock table������

mydumper֧�ּ�¼����Ĳ����������ڼ�¼����ĵ���ʱ�����߳�������������ʱ���Ա���в�֣�Ϊ���һ���ּ�¼����һ������������һ���ô����ǵ���ĳ�����ر���ʱ����Ծ����ܵ����ö��̲߳�������ĳ���߳��ڵ���һ�����������̴߳��ڿ���״̬���ڷָ�ʱ������ѡȡ������PRIMARY KEY����Ϊ�ָ����ݣ����û���������������Ψһ����(UNIQUE KEY)�������ϳ��Զ�ʧ�ܺ���ѡȡһ�����ֶȱȽϸߵ��ֶ���Ϊ��¼���ֵ�����(ͨ�� show index ������е�cardinality��ֵȷ��)��

���ֵķ�ʽ�Ƚϱ�����ֱ��ͨ�� select min(filed),max(filed) from table ��û����ֶε�ȡֵ��Χ��ͨ�� explain select filed from table ��ȡ�ֶμ�¼��������Ȼ��ͨ��һ��ȷ���Ĳ������ÿһ���������ִ��ʱ��where���������ּ��㷽ʽֻ֧���������͵��ֶΡ�

���Ͼ���mydumper�Ĳ�����ȡһ�������ݵķ�ʽ����ؼ�����������Innodb���MVCC���ܣ�����ͨ�����ն����ֻ�������񴴽��׶β���Ҫ������

### mydumper ����

```shell
01
-B, --database              Ҫ���ݵ����ݿ⣬��ָ���򱸷����п�
02.
-T, --tables-list           ��Ҫ���ݵı������ö��Ÿ���
03.
-o, --outputdir             �����ļ������Ŀ¼
04.
-s, --statement-size        ���ɵ�insert�����ֽ�����Ĭ��1000000
05.
-r, --rows                  �����зֿ�ʱ��ָ���Ŀ�������ָ�����ѡ���ر� --chunk-filesize
06.
-F, --chunk-filesize        ������С�ֿ�ʱ��ָ���Ŀ��С����λ�� MB
07.
-c, --compress              ѹ������ļ�
08.
-e, --build-empty-files     ����������ǿգ����ǲ���һ�����ļ���Ĭ����������ֻ�б�ṹ�ļ���
09.
-x, --regex                 ��ͬ������ʽƥ�� 'db.table'
10.
-i, --ignore-engines        ���ԵĴ洢���棬�ö���ָ�
11.
-m, --no-schemas            �����ݱ�ṹ
12.
-k, --no-locks              ��ʹ����ʱ����ֻ������ʹ�����ѡ���������ݲ�һ��
13.
--less-locking              ���ٶ�InnoDB�����ʩ��ʱ�䣨����ģʽ�Ļ���������⣩
14.
-l, --long-query-guard      �趨�������ݵĳ���ѯ��ʱʱ�䣬��λ���룬Ĭ����60�루��ʱ��Ĭ��mydumper�����˳���
15.
--kill-long-queries         ɱ������ѯ (���˳�)
16.
-b, --binlogs               ����binlog
17.
-D, --daemon                �����ػ�����ģʽ���ػ�����ģʽ��ĳ���������϶����ݿ���б���
18.
-I, --snapshot-interval     dump���ռ��ʱ�䣬Ĭ��60s����Ҫ��daemonģʽ��
19.
-L, --logfile               ʹ�õ���־�ļ���(mydumper����������־), Ĭ��ʹ�ñ�׼���
20.
--tz-utc                    ��ʱ����ʹ�õ�ѡ���������
21.
--skip-tz-utc               ͬ��
22.
--use-savepoints            ʹ��savepoints�����ٲɼ�metadata����ɵ���ʱ�䣬��Ҫ SUPER Ȩ��
23.
--success-on-1146           Not increment error count and Warning instead of Critical in case of table doesn't exist
24.
-h, --host                  ���ӵ�������
25.
-u, --user                  ������ʹ�õ��û�
26.
-p, --pass<a href="http://www.it165.net/edu/ebg/" target="_blank"class="keylink">word</a>              ����
27.
-P, --port                  �˿�
28.
-S, --socket                ʹ��socketͨ��ʱ��socket�ļ�
29.
-t, --threads               �����ı����߳�����Ĭ����4
30.
-C, --compress-protocol     ѹ����mysqlͨ�ŵ�����
31.
-V, --version               ��ʾ�汾��
32.
-v, --verbose               �����Ϣģʽ, 0 = silent, 1 = errors, 2 = warnings, 3= info, Ĭ��Ϊ 2
```

### myloader ����

```shell
01
-d, --directory                   �����ļ����ļ���
02.
-q, --queries-per-transaction     ÿ������ִ�еĲ�ѯ������Ĭ����1000
03.
-o, --overwrite-tables            ���Ҫ�ָ��ı���ڣ�����drop���ñ�ʹ�øò�������Ҫ����ʱ��Ҫ���ݱ�ṹ
04.
-B, --database                    ��Ҫ��ԭ�����ݿ�
05.
-e, --enable-binlog               ���û�ԭ���ݵĶ�������־
06.
-h, --host                        ����
07.
-u, --user                        ��ԭ���û�
08.
-p, --pass<a href="http://www.it165.net/edu/ebg/" target="_blank"class="keylink">word</a>                    ����
09.
-P, --port                        �˿�
10.
-S, --socket                      socket�ļ�
11.
-t, --threads                     ��ԭ��ʹ�õ��߳�����Ĭ����4
12.
-C, --compress-protocol           ѹ��Э��
13.
-V, --version                     ��ʾ�汾
14.
-v, --verbose                     ���ģʽ, 0 = silent, 1 = errors, 2 = warnings,3 = info, Ĭ��Ϊ2
```

### ʹ�ð���

```shell
# ����game�⵽/backup/01�ļ����У���ѹ�������ļ�
mydumper -u root -p ### -h localhost -B game -c -o /backup/01

# �����������ݿ⣬�����ݶ�������־�ļ���������/backup/02�ļ���

mydumper -u root -p ### -h localhost -o /backup/02

# ����game.tb_player���Ҳ����ݱ�ṹ��������/backup/03�ļ���

mydumper -u root -p ### -h localhost -T tb_player -m -o /backup/03

# ��ԭ

mysqlload -u root -p ### -h localhost -B game -d /backup/02

mydumper��less lockingģʽ

mydumperʹ��--less-locking���Լ������ȴ�ʱ�䣬��ʱmydumper��ִ�л��ƴ���Ϊ
```

## mysqldump,mysqlpump,mydumper�Ա�

|�����	|IP��ַ|���|
|:--|:--|:--|
|mastera|172.25.0.11|mysql server 5.7.17|
|workstation|172.25.0.10|python python-matplotlib|


```shell
[root@mastera mysql5.7]# ls
mysql-5.7.17-1.el7.x86_64.rpm-bundle.tar                 mysql-community-libs-compat-5.7.17-1.el7.x86_64.rpm
mysql-community-client-5.7.17-1.el7.x86_64.rpm           mysql-community-minimal-debuginfo-5.7.17-1.el7.x86_64.rpm
mysql-community-common-5.7.17-1.el7.x86_64.rpm           mysql-community-server-5.7.17-1.el7.x86_64.rpm
mysql-community-devel-5.7.17-1.el7.x86_64.rpm            mysql-community-server-minimal-5.7.17-1.el7.x86_64.rpm
mysql-community-embedded-5.7.17-1.el7.x86_64.rpm         mysql-community-test-5.7.17-1.el7.x86_64.rpm
mysql-community-embedded-compat-5.7.17-1.el7.x86_64.rpm  sysbench-master
mysql-community-embedded-devel-5.7.17-1.el7.x86_64.rpm   sysbench-master.zip
mysql-community-libs-5.7.17-1.el7.x86_64.rpm
[root@mastera mysql5.7]# rpm -ivh mysql-community-client-5.7.17-1.el7.x86_64.rpm mysql-community-common-5.7.17-1.el7.x86_64.rpm mysql-community-devel-5.7.17-1.el7.x86_64.rpm mysql-community-server-5.7.17-1.el7.x86_64.rpm mysql-community-libs-5.7.17-1.el7.x86_64.rpm 
warning: mysql-community-client-5.7.17-1.el7.x86_64.rpm: Header V3 DSA/SHA1 Signature, key ID 5072e1f5: NOKEY
Preparing...                          ################################# [100%]
Updating / installing...
   1:mysql-community-common-5.7.17-1.e################################# [ 20%]
   2:mysql-community-libs-5.7.17-1.el7################################# [ 40%]
   3:mysql-community-client-5.7.17-1.e################################# [ 60%]
   4:mysql-community-server-5.7.17-1.e################################# [ 80%]
   5:mysql-community-devel-5.7.17-1.el################################# [100%]
[root@mastera mysql5.7]# systemctl start mysqld
[root@mastera mysql5.7]# grep password /var/log/mysqld.log
2017-02-09T08:32:17.515190Z 1 [Note] A temporary password is generated for root@localhost: p!hHXls+U6Po
[root@mastera mysql5.7]# mysqladmin -uroot -p'p!hHXls+U6Po' password '(Uploo00king)'
mysqladmin: [Warning] Using a password on the command line interface can be insecure.
Warning: Since password will be sent to server in plain text, use ssl connection to ensure password safety.
[root@mastera mysql5.7]# mysql -uroot -p'(Uploo00king)'
mysql: [Warning] Using a password on the command line interface can be insecure.
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 4
Server version: 5.7.17 MySQL Community Server (GPL)

Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> create database db1;
Query OK, 1 row affected (0.00 sec)

mysql> use db1;
Database changed

mysql> create table t1 (id int primary key auto_increment,num int);
Query OK, 0 rows affected (0.06 sec)

mysql> create table t2 like t1;
Query OK, 0 rows affected (0.24 sec)

mysql> create table t3 like t1;
Query OK, 0 rows affected (0.04 sec)

mysql> create table t4 like t1;
Query OK, 0 rows affected (0.34 sec)

mysql> delimiter //

mysql> create procedure booboo_insert (in tb_name varchar(20)) begin declare i int; declare j int; declare _sql varchar(100); set i=1; set j=10000; set _sql=concat('insert into ',tb_name,' set num=',i); set @sql=_sql;start transaction; prepare s2 from @sql; while i<=j do execute s2; set i=i+1;end while; deallocate prepare s2; commit; end//
Query OK, 0 rows affected (0.01 sec)

mysql> call booboo_insert('t1')//
Query OK, 0 rows affected (0.71 sec)

mysql> call booboo_insert('t2')//
Query OK, 0 rows affected (0.71 sec)

mysql> call booboo_insert('t3')//
Query OK, 0 rows affected (0.68 sec)

mysql> call booboo_insert('t4')//
Query OK, 0 rows affected (0.83 sec)

mysql> show table status;
+------+--------+---------+------------+-------+----------------+-------------+-----------------+--------------+-----------+----------------+---------------------+---------------------+------------+-------------------+----------+----------------+---------+
| Name | Engine | Version | Row_format | Rows  | Avg_row_length | Data_length | Max_data_length | Index_length | Data_free | Auto_increment | Create_time         | Update_time         | Check_time | Collation         | Checksum | Create_options | Comment |
+------+--------+---------+------------+-------+----------------+-------------+-----------------+--------------+-----------+----------------+---------------------+---------------------+------------+-------------------+----------+----------------+---------+
| t1   | InnoDB |      10 | Dynamic    | 10000 |             32 |      327680 |               0 |            0 |         0 |          10002 | 2017-02-09 16:40:53 | 2017-02-09 16:43:39 | NULL       | latin1_swedish_ci |     NULL |                |         |
| t2   | InnoDB |      10 | Dynamic    | 10000 |             32 |      327680 |               0 |            0 |         0 |          10001 | 2017-02-09 16:40:58 | 2017-02-09 16:43:59 | NULL       | latin1_swedish_ci |     NULL |                |         |
| t3   | InnoDB |      10 | Dynamic    | 10000 |             32 |      327680 |               0 |            0 |         0 |          10001 | 2017-02-09 16:41:00 | 2017-02-09 16:44:02 | NULL       | latin1_swedish_ci |     NULL |                |         |
| t4   | InnoDB |      10 | Dynamic    | 10000 |             32 |      327680 |               0 |            0 |         0 |          10001 | 2017-02-09 16:41:03 | 2017-02-09 16:44:05 | NULL       | latin1_swedish_ci |     NULL |                |         |
+------+--------+---------+------------+-------+----------------+-------------+-----------------+--------------+-----------+----------------+---------------------+---------------------+------------+-------------------+----------+----------------+---------+
4 rows in set (0.00 sec)

mysql> exit
Bye

[root@mastera mysql5.7]# du -h /var/lib/mysql
12M	/var/lib/mysql/mysql
1.1M	/var/lib/mysql/performance_schema
676K	/var/lib/mysql/sys
1.6M	/var/lib/mysql/db1
135M	/var/lib/mysql

```

�洢����

�ô洢���̿���ѭ������10000�����ݣ����ҿ��Դ�����������������

```shell
create procedure booboo_insert (in tb_name varchar(20)) 
begin 
declare i int; 
declare j int; 
declare _sql varchar(100); 
set i=1; set j=10000; 
set _sql=concat('insert into ',tb_name,' set num=',i); 
set @sql=_sql;
start transaction; 
prepare s2 from @sql; 
while i<=j do 
execute s2; 
set i=i+1;
end while; 
deallocate prepare s2; 
commit; 
end//
```

�ֶ��������ĸ����Ա����ǿ��Կ���db1��ĺ�С��1.6M������������һ���������Ƚ�Сʱ��mysqldump��mysqlpump������

```shell
[root@mastera mysql5.7]# time mysqlpump -uroot -p'(Uploo00king)' db1 --default-parallelism=4 > /tmp/mysql.pump4.sql
mysqlpump: [Warning] Using a password on the command line interface can be insecure.
Dump progress: 0/1 tables, 250/10000 rows
Dump completed in 3016 milliseconds

real	0m3.166s
user	0m0.219s
sys	0m0.475s
[root@mastera mysql5.7]# time mysqlpump -uroot -p'(Uploo00king)' db1  > /tmp/mysql.pump2.sql
mysqlpump: [Warning] Using a password on the command line interface can be insecure.
Dump progress: 0/1 tables, 250/10000 rows
Dump completed in 3703 milliseconds

real	0m3.871s
user	0m0.307s
sys	0m0.606s
[root@mastera mysql5.7]# time mysqldump -uroot -p'(Uploo00king)' db1 > /tmp/mysql.dump.sql
mysqldump: [Warning] Using a password on the command line interface can be insecure.

real	0m0.411s
user	0m0.024s
sys	0m0.116s
```

�ӽ�������������ǳ�С��ʱ��û��Ҫ���и��ƣ�mysqlpump��������

������������һ���������Ƚϴ��ʱ��

����sbtest�⣬ʹ��sysbenchѹ�⹤��׼��4��1000000�еı�

```shell
mysql> show table status;
+---------+--------+---------+------------+--------+----------------+-------------+-----------------+--------------+-----------+----------------+---------------------+---------------------+------------+-------------------+----------+------------------+---------+
| Name    | Engine | Version | Row_format | Rows   | Avg_row_length | Data_length | Max_data_length | Index_length | Data_free | Auto_increment | Create_time         | Update_time         | Check_time | Collation         | Checksum | Create_options   | Comment |
+---------+--------+---------+------------+--------+----------------+-------------+-----------------+--------------+-----------+----------------+---------------------+---------------------+------------+-------------------+----------+------------------+---------+
| sbtest1 | InnoDB |      10 | Dynamic    | 986776 |            221 |   218841088 |               0 |            0 |   4194304 |        1000001 | 2017-02-09 17:03:29 | 2017-02-09 17:03:18 | NULL       | latin1_swedish_ci |     NULL | max_rows=1000000 |         |
| sbtest2 | InnoDB |      10 | Dynamic    | 986020 |            234 |   231456768 |               0 |            0 |   6291456 |           NULL | 2017-02-09 17:06:22 | 2017-02-09 17:07:14 | NULL       | latin1_swedish_ci |     NULL |                  |         |
| sbtest3 | InnoDB |      10 | Dynamic    | 986020 |            234 |   231456768 |               0 |            0 |   6291456 |           NULL | 2017-02-09 17:07:25 | 2017-02-09 17:08:11 | NULL       | latin1_swedish_ci |     NULL |                  |         |
| sbtest4 | InnoDB |      10 | Dynamic    | 986559 |            225 |   222019584 |               0 |            0 |   6291456 |           NULL | 2017-02-09 17:08:24 | 2017-02-09 17:09:15 | NULL       | latin1_swedish_ci |     NULL |                  |         |
+---------+--------+---------+------------+--------+----------------+-------------+-----------------+--------------+-----------+----------------+---------------------+---------------------+------------+-------------------+----------+------------------+---------+
4 rows in set (0.00 sec)

[root@mastera sysbench-master]# du -h /var/lib/mysql
12M	/var/lib/mysql/mysql
1.1M	/var/lib/mysql/performance_schema
676K	/var/lib/mysql/sys
1.6M	/var/lib/mysql/db1
937M	/var/lib/mysql/sbtest
1.2G	/var/lib/mysql
```

������ǰ���ݿ�����ݴﵽ��11G��������������

```shell
[root@mastera ~]# time mysqldump -uroot -p'(Uploo00king)' sbtest --single-transaction > /tmp/mysqldump.sbtest.sql
[root@mastera ~]# time mysqlpump -uroot -p'(Uploo00king)' sbtest --single-transaction --default-parallelism=2 > /tmp/mysqlpump.sbtest.sql
```