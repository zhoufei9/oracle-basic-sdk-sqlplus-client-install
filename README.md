# Oracle database client installation（basic/sdk/sqlplus）

安装client客户端<br/>
1.要远程连接oracle，先下载下面三个文件，注意版本最好一致。 <br/>
不同版本，或者不同操作系统的，请到官方网站下载，里面有详细说明。 <br/>
我的php版本为5.6.30，要连接的oracle服务器是 11g R2，操作系统版本CentOS 6.9 x86_64 <br/>
尽量根据oracle版本下载对应的客户端安装包<br/>
instantclient-sdk-linux.x64-11.1.0.2.0.zip  (连接oracle必选)<br/>
instantclient-basic-linux.x64-11.1.0.2.0.zip (连接oracle必选)<br/>
instantclient-sqlplus-linux.x64-11.1.0.2.0.zip (需要安装目标机器在命令行访问oracle时选)<br/><br/>

2.先创建三个客户端的安装目录,但配置环境变量时，需要一致<br/>
mkdir -p /home/oracle<br/>
mkdir -p /home/oracle/sdk <br/>
mkdir -p /home/oracle/network/admin (监听器和网络环境)<br/><br/>

3.解压到安装目录 <br/>
把版本sqlplus、basic解压后的目录下所有文件搬到安装目录<br/>
unzip instantclient-sqlplus-linux.x64-11.1.0.2.0.zip -d /home/oracle/<br/>
unzip instantclient-basic-linux.x64-11.1.0.2.0.zip   -d /home/oracle/<br/>
改名lib <br/>
mv /home/oracle/instantclient_11_2  /home/oracle/lib<br/>
sdk解压<br/>
unzip instantclient-sdk-linux.x64-11.1.0.2.0.zip   -d /home/oracle/sdk<br/><br/>

4.配置环境变量<br/>
vi /etc/profile<br/>
加入 <br/>
export ORACLE_HOME=/home/oracle<br/>
export LD_LIBRARY_PATH=$ORACLE_HOME/lib<br/>
export PATH=$ORACLE_HOME/bin:$PATH <br/>
保存并退出。<br/>
source /etc/profile              //使配置文件立刻生效 <br/>
echo $ORACLE_HOME                //查看一下配置的环境变量是否成功 <br/><br/>

5.配置监听器和网络环境。 <br/>
  复制oracle服务端的tnsnames.ora文件，放到/home/oracle/network/admin/目录下，并且改成如下内容，注意host和port、SERVICE_NAME：<br/>
  或者创建监听文件 tnsnames.ora<br/>
  #vim /home/oracle/network/admin/tnsnames.ora<br/>
	ORCL =<br/>
	  (DESCRIPTION =<br/>
		(ADDRESS = (PROTOCOL = TCP)(HOST = 192.168.23.12)(PORT = 1521))<br/>
		(CONNECT_DATA =<br/>
		  (SERVER = DEDICATED)<br/>
		  (SERVICE_NAME = orcl)<br/>
		)<br/>
	  )<br/>
 注意：服务端的listener.ora文件 需要把localhost改为它的ip或者它的全名。 <br/><br/>
  
  
6.测试<br/>
$LD_LIBRARY_PATH/sqlplus username/password@192.168.23.12/orcl<br/><br/>
出现如下表示连接成功
SQL*Plus: Release 11.2.0.4.0 Production on Wed Jun 7 10:50:20 2017<br/>
Copyright (c) 1982, 2013, Oracle.  All rights reserved.<br/>
Connected to:<br/>
Oracle Database 11g Enterprise Edition Release 11.2.0.1.0 - 64bit Production<br/>
With the Partitioning, OLAP, Data Mining and Real Application Testing options<br/>
SQL> 
