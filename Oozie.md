# Oozie使用

使用到的软件

- hadoop 0.20.205.0
- oozie  3.2.0
- extjs  2.2
- maven  3.0.4

## 下载及编译
	wget http://mirrors.gigenet.com/apache/incubator/oozie/stable/oozie-3.2.0-incubating.tar.gz

悲催的是java写的东东居然还需要下载下来编译， 所以需要有maven3的环境

配置maven3很简单，下载zip包后解压到某目录下，然后配置两个环境变量

	export M3_HOME={you maven folder}
	export Path=$Path:$M3_HOME/bin

到{Oozie home}/bin目录下运行

	mkdistro.sh

如果失败的话，从失败的地方重新运行可以用命令：      

	mkdistro.sh -ff


## 安装

## 配置
### 配置Hadoop
{hadoop home}/conf/core-site.xml 加入以下内容

	<property>
	  <name>hadoop.proxyuser.hadoop.hosts</name>
	  <value>{your server}</value>
	</property>
	<property>
	  <name>hadoop.proxyuser.hadoop.groups</name>
	  <value>hadoop</value>
	</property>

***Note:***此处的value为运行job所在的机器名和hadoop所在的group名。不能用*

别忘记copy配置文件到slave, 重启启动Hadoop

### 修改及配置Oozie

#### 修改{OOZIE_HOME}/conf/oozie-site.xml

	<property>
		<name>oozie.services.ext</name>
		<value>
			org.apache.oozie.service.HadoopAccessorService
		</value>
		<description>
		To add/replace services defined in 'oozie.services' with custom implementations.Class names must be separated by commas.
		</description>
	</property>



如果你暂时不配置数据库的话，需要让Oozie自动创建数据库，所以要修改下面内容：将false改为true

		<property>
		    <name>oozie.service.JPAService.create.db.schema</name>
		    <value>true</value>
		    <description>
		        Creates Oozie DB.

		        If set to true, it creates the DB schema if it does not exist. If the DB schema exists is a NOP.
		        If set to false, it does not create the DB schema. If the DB schema does not exist it fails start up.
		    </description>
		</property>

否则你会遇到以下问题 

	Caused by: org.apache.openjpa.lib.jdbc.ReportingSQLException: Schema 'SA' does not exist

#### 修改{OOZIE_HOME}/conf/oozie-env.sh

	export OOZIE_HTTP_HOSTNAME={your host}
	export OOZIE_HTTP_PORT={your Oozie port}


#### Setup Oozie

	{OOZIE_HOME}/bin/oozie-setup.sh -hadoop 0.20.200 {HADOOP_HOME} -extjs {/PATH/TO}/ext-2.2.zip

### 启动Oozie

	$ {OOZIE_HOME}/bin/oozie-start.sh

当然第一次启动你希望看看启动的时候的一些信息， 你可以用

	$ {OOZIE_HOME}/bin/oozie-run.sh	



### Run Jobs



hadoop is not allowed to impersonate hadoop


