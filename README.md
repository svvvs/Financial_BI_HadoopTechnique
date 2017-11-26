# Financial_BI_HadoopTechnique
training 20171125

useful links official site

http://hadoop.apache.org

http://hadoop.hbase.org

spark

hbase
redis
http://pan.baidu.com/s/1dEULcLN  pwd:45bz
QQ: 50926571

big data architect performance (forein official sites, CSDN)
data extraction 
data analysis (application: business, algo: math, statistic, python/ML skelearn)
National Big Data white book
jikebang

Questions:
1. hadoop的核心组件hdfs,yarn,MR做为数据平台优势？


#======================================================================================
#                                   d1_hdfs
#======================================================================================
#--------------操作提示-------
#business:practise hadoop 
#author:zhangyuelei
#date:
#提示:请将sysdate换成练习操作日期yyyymmdd,

#yourname更改为你的名字拼音，例如zhangyuelei
#-------------------------------


#创建当前日期的本地文件夹并产生数据文件
mkdir -p /home/hadoop/20171107/
echo '1 zhangyuelei '> ~/$(date +%Y%m%d)/zhangyuelei/zhangyuelei.txt
echo '2 zhangyuelei '>> ~/$(date +%Y%m%d)/zhangyuelei/zhangyuelei.txt
echo '2 zhangyuelei '>> ~/$(date +%Y%m%d)/zhangyuelei/zhangyuelei.txt
#在集群中创建目录
hadoop fs -mkdir -p /20171107/zhangyuelei
#在集群中查看自己创建的目录
hadoop fs -ls /20171014/
#把本地文件传到集群中
hadoop fs -put ~/$(date +%Y%m%d)/zhangyuelei/zhangyuelei.txt  /20171014/zhangyuelei
#把本地目录下（/home/hadoop/datasource/studentdata）相关数据文件传到集群中
hadoop fs -put /home/hadoop/datasource/studentdata/* /20171014/zhangyuelei

#在集群中查看自己上传的文件
hadoop fs -ls /20171014/zhangyuelei

#把集群中文件保存到本地指定目录
hadoop fs -get /20171014/zhangyuelei/zhangyuelei.txt ~/20171014/zhangyuelei/output/
#在本地查看自己下载的文件
ls ~/20171014/zhangyuelei/output

#集群文件检测命令
hadoop fsck /20171114/zhangyixuan/zhangyixuan.txt

#查看集群文件具体的属性信息并打印
hadoop fs  -stat  "%o %r" /20171109/vincentwei/vincentwei.txt
#根据以下提示查看文件的其他属性信息
{HADOOP_HOME}/bin/hadoop fs –stat [format]
下面列出了format的形式：
%b：打印文件大小（目录为0）
%n：打印文件名
%o：打印block size 
%r：打印备份数
%y：打印UTC日期 yyyy-MM-dd HH:mm:ss
%Y：打印自1970年1月1日以来的UTC微秒数
%F：目录打印directory, 文件打印regular file
当使用-stat选项但不指定format时候，只打印文件创建日期，相当于%y：

#查看hdfs的集群状态命令(自己理解下对应的参数含义)：
hadoop dfsadmin -report

#查看hdfs的集群文件命令(自己理解下对应的参数含义)：
hadoop fs -du /20171109/


#归档集群文件到指定集群目录	
hadoop archive -archiveName 	.har -p /20171014/zhangyuelei /20171014/zhangyuelei/output

#拷贝归档文件到集群中自己创建的文件夹下
hadoop distcp /20171014/zhangyuelei/output/zhangyue.har /20171014/zhangyuelei

-----运行mapreduce程序

hadoop jar /home/hadoop/bigdata/hadoop/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.2.jar  wordcount  /20171116/will/will01.txt /20171116/will/output9/

hadoop fs -ls /20171014/zhangyuelei/output2

#======================================================================================
#                                   d1_env
#======================================================================================
把以下配置文件拷贝到
/home/hadoop/20171107/yourname 目录下
通过cat命令查看对应核心配置文件的参数

#Hadoop核心参数配置$HADOOP_HOME/etc/hadoop/
1、master 记录运行辅助namenode的机器列表 
2、slaves 记录运行datanode和tasktracker的机器列表 
3、hadoop-env.sh 记录脚本要用的环境变量，以运行hadoop 
4、core-site.xml hadoop core的配置项，例如hdfs和mapreduce常用的i/o设置等 
5、hdfs-site.xml hadoop守护进程的配置项，包括namenode、辅助namenode和datanode等 
6、mapred-site.xml mapreduce守护进程的配置项，包括jobtracker和tasktracker 
7、hadoop-metrics.properties 控制metrics在hadoop上如何发布的属性 
8、log4j.properties 系统日志文件、namenode审计日志、tasktracker子进程的任务日志的属性 
9 hive-site.xml 进行Hive的核心参数配置 $HIVE_HOME/conf
10 zoo.cfg ZOOKEEPER的配置参数 路径home/hadoop/bigdata/zk/conf
11 hbase-site.xml 进行HBASE的参数配置$HBASE_HOME/conf


#把hdfs集群中指定目录下的文件内容合并下载到本地指定文件内
hadoop fs -getmerge  /20171107/yourname/ /20171107/yourname/merge.txt

#======================================================================================
#                                   d1_Hbase
#======================================================================================
#hbase 进入
hbase shell
#hbase 创建表
create 'tbl_yuelei1108','cf'
#hbase 查看表
list
scan 'tbl_yuelei1108'

#hbase导入表
sqoop import --connect jdbc:mysql://slave02:3306/yinlian --table TBL_TRANS --hbase-table TBL_TRANS --column-family TRANS --hbase-row-key TRANS_DATETIME --hbase-create-table --username hive -password hive -m 1 --split-by SETT_DATE --null-string '\\N' --null-non-string '\\N'

#======================================================================================
#                                   d1_Hive
#======================================================================================
#--------------操作提示-------
#business:practise hadoop 
#author:yuanmin
#date:
#提示:请将sysdate换成练习操作日期yyyymmdd
#yourname或yuanmin更改为你的名字拼音，例如yuanmin
#-------------------------------

----hive 各种表构建方式练习--

----/home/hadoop/datasource/studentdata---
Hive的建表
1 Hive 创建内部表
use huifeng1018;
show tables;

CREATE TABLE yourname_stud_info (
  stud_code string ,
  stud_name string ,
  stud_sex string COMMENT '性别',
  birthday date ,
  log_date date ,
  orig_addr string ,
  lev_date date ,
  college_code string  COMMENT '学院编码',
  college_name string ,
  state string 
) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' STORED AS TEXTFILE;

#查看表结构
show create table yourname_stud_info;
#加载传到hdfs的数据到表
load data inpath '/20171118/yourname/stud_info.csv' into table huifeng1018.yourname_stud_info;
#查看对应表数据
select * from huifeng1018.yourname_stud_info;
2 外部表
drop table if exists yuanmin_external_stud_info;
create external table yuanmin_external_stud_info (
stud_code string ,
  stud_name string ,
  stud_sex string COMMENT '性别',
  birthday date ,
  log_date date ,
  orig_addr string ,
  lev_date date ,
  college_code string  COMMENT '学院编码',
  college_name string ,
  state string 
)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' STORED AS TEXTFILE
LOCATION '/20171118/yourname/yuanmin_external_stud_info';
#拷贝数据到对应外部表目录
hadoop fs -cp /20171118/yourname/stud_info.csv /20171118/yourname/yuanmin_external_stud_info/
#查看对应表数据
select * from huifeng1018.yuanmin_external_stud_info;

3 退出hive进入shell客户端用sqoop命令创建表并导入数据
#创建与mysql数据库中testdb.stud_info结构一样的hive表
sqoop create-hive-table --connect jdbc:mysql://slave02:3306/testdb --table stud_info --username hive --password hive --hive-table huifeng1018.yuanmin_stud_info_sqoo
#导入mysql数据库中testdb.stud_info数据到hive表
sqoop import  --connect jdbc:mysql://slave02:3306/testdb --table stud_info --username hive --password hive --hive-import --hive-table huifeng1018.yuanmin_stud_info_sqoo -m 1 --target-dir /20171118/yourname/temp3
#查看对应表数据
select * from huifeng1018.yuanmin_stud_info_sqoo;
ODL层表：
drop table if exists  stg_yuanmin_s_stud_info;
create table  stg_yuanmin_s_stud_info as
select
concat(college_code ,stud_code) as stud_id,
stud_code   ,               
stud_name   ,               
stud_sex    ,               
birthday    ,               
log_date    ,               
orig_addr   ,               
lev_date    ,               
college_code,               
college_name,               
state                                                                                                       
from huifeng1018.yuanmin_stud_info_sqoo;
#查看对应表数据
select * from stg_yuanmin_s_stud_info;

IDL层 数仓-维度层表
create table huifeng1018.yuanmin_dim_address_info (id int,city string,people_m double,gdp double,per_gdp double,per_gdp_f double) row format delimited fields terminated by ',' stored as textfile;
#加载数据进hive,学员环境数据文件路径在/home/hadoop/data/stg_city.csv
load data local inpath '/home/hadoop/yuelei/stg_city.csv' into table huifeng1018.yuanmin_dim_address_info;



IDL层 数仓事实表
create table  yuanmin_dw_stud_info as select 
a.stud_id,a.stud_code,             
a.stud_name ,         
a.stud_sex ,               
a.birthday,               
a.log_date    ,               
a.orig_addr   ,               
a.lev_date    ,               
a.college_code,a.college_name,a.state,b.id,b.people_m,b.gdp,b.per_gdp from yuanmin_s_stud_info a left outer join yuanmin_dim_address_info b on a.orig_addr=b.city;

ADL层
Create table yuanmin_dm_stud_info (
Stud_id string,
Name string,
City string,
City_desc string,
Birthday string,
Star string,
Star_desc string)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' STORED AS TEXTFILE;

drop table if exists yuanmin_rpt_college_code_sum;
Create table yuanmin_rpt_college_code_sum
 as select college_code,college_name,count(1) 
 from yuanmin_dw_stud_info group by college_code, college_name;

#HIVE查询语句优化
select * from yuanmin_stud_info_sqoo;
set hive.exec.parallel=true ，默认为false
set hive.exec.parallel.thread.number=8
select * from yuanmin_stud_info_sqoo;



#参考城市维度表yuanmin_dim_address_info理解Hive的维度模型设计思想，
在Hive数据库的huifeng1018库中自己整理出一张生日星座性格特点的维度表
yourname_dim_xingzuo_info，必含的字段有（出生月日，星座，星座性格特点）,并加载对应数据进该维度表。
#根据数据创建对应的维度表结构如下
use huifeng1018;
CREATE TABLE yuanmin_dim_xingzuo_info_stg(
  name string ,
  shengri string ,
  start_month int ,
  end_month int ,
  start_date int ,
  end_date int ,
  xingge string ,
  guanlianxingzuo string ,
  gouchengyuansu string ,
  yanse string  ,
  yingwen string 
) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' STORED AS TEXTFILE;
#加载数据进入表结构
load data local inpath '/home/hadoop/data/xingzuo.txt' overwrite into table yuanmin_dim_xingzuo_info_stg
#清洗掉含Null的数据，创建维度表语句如下：
create table yuanmin_dim_xingzuo_info as 
select * from yuanmin_dim_xingzuo_info_stg
where xingge is not NULL;

