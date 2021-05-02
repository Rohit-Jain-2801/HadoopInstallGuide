# HadoopInstallGuide
Apache Hadoop Components Installation Guide on Windows

<br/>

## Apache Hadoop Installation
* Download [Java JDK 8 (v1.8.0_291)](https://www.oracle.com/in/java/technologies/javase/javase-jdk8-downloads.html).
* Download [Hadoop Binary (v3.3.0)](https://hadoop.apache.org/releases.html) Latest Version.
* Create new folder named `Hadoop` in the directory where we want to keep all things related to Hadoop & extract hadoop binary in it.
* Setting Environment Path Variables:
    + Set Variable as `JAVA_HOME` & Value as `<Java Root Path>`.
    + Set Variable as `HADOOP_HOME` & Value as `<Hadoop Root Path>`.
    + Add following paths to `Path` Variable:
        - `<Java Bin Path>`
        - `<Hadoop Bin Path>`
        - `<Hadoop Sbin Path>`
* Check if Java is installed properly by running following commands:
    + `javac`
    + `java -version`
* Make new folder named `data` in root directory of Hadoop followed by:
    + Making new folder named `datanode` inside `data` folder.
    + Making new folder named `namenode` inside `data` folder.
* Make changes in 4 hadoop files located in `etc/hadoop/`:
    + `core-site.xml`
    ```
    <configuration>
        <property>
            <name>fs.defaultFS</name>
            <value>hdfs://localhost:9000</value>
            <!-- <value>hdfs://0.0.0.0:19000</value> -->
        </property>

        <property>
            <name>hadoop.tmp.dir</name>
            <value>file:///E:/Rohit/Hadoop/hadoop/tmp/hadoop-${user.name}<value>
        </property>
    </configuration>
    ```
    + `mapred-site.xml`
    ```
    <configuration>
        <property>
            <name>mapreduce.framework.name</name>
            <value>yarn</value>
        </property>
    </configuration>
    ```
    + `yarn-site.xml`
    ```
    <configuration>
        <property>
            <name>yarn.nodemanager.aux-services</name>
            <value>mapreduce_shuffle</value>
        </property>
        
        <property>
            <name>yarn.nodemanager.auxservices.mapreduce.shuffle.class</name>  
            <value>org.apache.hadoop.mapred.ShuffleHandler</value>
        </property>
    </configuration>
    ```
    + `hdfs-site.xml`
    ```
    <configuration>
        <property>
            <name>dfs.replication</name>
            <value>1</value>
        </property>

        <property>
            <name>dfs.namenode.name.dir</name>
            <value>file:///E:/Rohit/Hadoop/hadoop/data/namenode</value>
        </property>

        <property>
            <name>dfs.datanode.data.dir</name>
            <value>file:///E:/Rohit/Hadoop/hadoop/data/datanode</value>
        </property>
    </configuration>
    ```
* Download [files](https://github.com/kontext-tech/winutils) for support of Windows & add it to `bin` folder.
* Start Hadoop by opening `Terminal` as `Administrator` & by running following command:
    + `start-all.cmd` (or `start-dfs.cmd` & `start-yarn.cmd`)
* `Command` to check all the Hadoop daemons like `DataNode`, `NameNode`, `NodeManager` & `ResourceManager`:
    + `jps` (Java Virtual Machine Process Status Tool)
* To access Web-UI, open browser & go to:
    + `localhost:9870`: NameNode Information
    + `localhost:9864`: DataNode Information
    + `localhost:8088`: Resource Manager (YARN)
* Stop Hadoop by running following command:
    + `stop-all.cmd` (or `stop-dfs.cmd` & `stop-yarn.cmd`)

<br/>

## Apache HBase Installation
* Download [HBase Binary (v2.3.5)](https://hbase.apache.org/downloads.html) Latest Version.
* Preferably extract HBase in the same directory where Hadoop is residing.
* Make new folders named `hbase` & `zookeeper` in root directory of HBase.
* Open `hbase.cmd` file placed in `<hbase bin>` folder &
    + Search for `java_arguments` as variable.
    + Remove `%HEAP_SETTINGS% ` from the RHS.
* Open `hbase-env.cmd` file placed in `<hbase conf>` folder & add following lines:
```
set JAVA_HOME=%JAVA_HOME%
set HBASE_CLASSPATH=%HBASE_HOME%\lib\client-facing-thirdparty\*
set HBASE_HEAPSIZE=8000
set HBASE_OPTS="-XX:+UseConcMarkSweepGC" "-Djava.net.preferIPv4Stack=true"
set SERVER_GC_OPTS="-verbose:gc" "-XX:+PrintGCDetails" "-XX:+PrintGCDateStamps" %HBASE_GC_OPTS%
set HBASE_USE_GC_LOGFILE=true

set HBASE_JMX_BASE="-Dcom.sun.management.jmxremote.ssl=false" "-Dcom.sun.management.jmxremote.authenticate=false"

set HBASE_MASTER_OPTS=%HBASE_JMX_BASE% "-Dcom.sun.management.jmxremote.port=10101"
set HBASE_REGIONSERVER_OPTS=%HBASE_JMX_BASE% "-Dcom.sun.management.jmxremote.port=10102"
set HBASE_THRIFT_OPTS=%HBASE_JMX_BASE% "-Dcom.sun.management.jmxremote.port=10103"
set HBASE_ZOOKEEPER_OPTS=%HBASE_JMX_BASE% "-Dcom.sun.management.jmxremote.port=10104"
set HBASE_REGIONSERVERS=%HBASE_HOME%\conf\regionservers
set HBASE_LOG_DIR=%HBASE_HOME%\logs
set HBASE_IDENT_STRING=%USERNAME%
set HBASE_MANAGES_ZK=true
```
* Open `hbase-site.xml` file placed in `<hbase conf>` folder & add following lines inside `<configuration>` tag:
```
<property>
    <name>hbase.rootdir</name>
    <value>file:///E:/Rohit/Hadoop/HBase/hbase</value>
</property>
<property>
    <name>hbase.zookeeper.property.dataDir</name>
    <value>/E:/Rohit/Hadoop/HBase/zookeeper</value>
</property>
<property>
    <name>hbase.zookeeper.quorum</name>
    <value>localhost</value>
</property>
```
* Setting Environment Path Variables:
    + Set Variable as `HBASE_HOME` & Value as `<HBase Root Path>`.
    + Set Variable as `HBASE_BIN_PATH` & Value as `<HBase Bin Path>`.
    + Add `<HBase Bin Path>` path to `Path` Variable.
* Start HBase by opening `Terminal` as `Administrator` & by running following commands:
    + `start-all.cmd` (Hadoop)
    + `start-hbase.cmd` (HBase)
* To interact with HBase, run following command: `hbase shell`.
* Start HBase by running following command: `stop-hbase.cmd`.

<br/>

## Apache Hive Installation
* Download Relational Database - [Apache Derby Binary (v10.14.2.0)](https://db.apache.org/derby/derby_downloads.html) Latest Version to create its Metastore (where all metadata will be stored).
* Preferably extract Derby in the same directory where Hadoop is residing.
* Download [Cygwin (v3.2.0)](https://www.cygwin.com/) Latest Version & Install it.
* Download [Hive Binary (v3.1.2)](https://hive.apache.org/downloads.html) Latest Version.
* Preferably extract Hive in the same directory where Hadoop is residing.
* Setting Environment Path Variables:
    + Set Variable as `HIVE_HOME` & Value as `<Hive Root Path>`.
    + Set Variable as `DERBY_HOME` & Value as `<Dirby Root Path>`.
    + Set Variable as `HIVE_LIB` & Value as `<Hive Lib Path>`.
    + Set Variable as `HIVE_BIN` & Value as `<Hive Bin Path>`.
    + Set Variable as `HADOOP_USER_CLASSPATH_FIRST` & Value as `true`.
    + Add following paths to `Path` Variable:
        - `<Dirby Bin Path>`
        - `<Hive Bin Path>`
* Copy files from `Derby Lib` folder to `Hive Lib` folder.
* Create a new file named `hive-site.xml` in `<hive conf>` folder & add following lines:
```
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>

<configuration>
	<property>
		<name>javax.jdo.option.ConnectionURL</name> 
		<value>jdbc:derby://localhost:1527/metastore_db;create=true</value> 
		<description>JDBC connect string for a JDBC metastore</description>
	</property>
	<property> 
		<name>javax.jdo.option.ConnectionDriverName</name> 
		<value>org.apache.derby.jdbc.ClientDriver</value> 
		<description>Driver class name for a JDBC metastore</description>
	</property>
	<property> 
		<name>hive.server2.enable.doAs</name> 
		<description>Enable user impersonation for HiveServer2</description>
		<value>true</value>
	</property>
	<property>
		<name>hive.server2.authentication</name> 
		<value>NONE</value>
		<description> Client authentication types. NONE: no authentication check LDAP: LDAP/AD based authentication KERBEROS: Kerberos/GSSAPI authentication CUSTOM: Custom authentication provider (Use with property hive.server2.custom.authentication.class) </description>
	</property>
	<property>
		<name>datanucleus.autoCreateTables</name>
		<value>True</value>
	</property>
</configuration>
```
* Download extra [cmd files](https://svn.apache.org/repos/asf/hive/trunk/bin/) for Windows support from [this](https://github.com/HadiFadl/Hive-cmd) & replace in Hive bin directory along with sub-directories.
* Replace the Hive `guava-19.0.jar` stored in `Hive Lib` with Hadoop’s `guava-27.0-jre.jar` found in `hadoop\share\hadoop\hdfs\lib`.
* Make new directories in following locations as:
    + `E:\cygdrive`
    + `C:\cygdrive`
* Open the `Terminal` as `Administrator` and execute the following commands for symbolic links:
    + `mklink /J  E:\cygdrive\e\ E:\`
    + `mklink /J  C:\cygdrive\c\ C:\`
* Start Derby by opening `Terminal` as `Administrator` & by running following command: `StartNetworkServer -h 0.0.0.0`
* Open Cygwin utility and execute the following command: `cygstart ~/.bashrc` & add following lines:
```
export HADOOP_HOME='/cygdrive/e/Rohit/Hadoop/hadoop'
export PATH=$PATH:$HADOOP_HOME/bin
export HIVE_HOME='/cygdrive/e/Rohit/Hadoop/hive'
export PATH=$PATH:$HIVE_HOME/bin
export HADOOP_CLASSPATH=$HADOOP_CLASSPATH:$HIVE_HOME/lib/*.jar
```
* Comment `2` lines in file `hive-schema-3.1.0.derby.sql` in `hive\scripts\metastore\upgrade\derby` folder containing:
    + Line 1: `CREATE FUNCTION "APP"."NUCLEUS_ASCII" ...`
    + Line 2: `CREATE FUNCTION "APP"."NUCLEUS_MATCHES" ...`
* Inside Cygwin utility, goto hive-bin by `cd $HIVE_HOME/bin` & run command: `schematool -dbType derby -initSchema` for `Initializing Hive Metastore`.
* Start Hive by opening `Terminal` as `Administrator` & by running following commands:
    + `start-all.cmd` (Hadoop)
    + `hadoop dfsadmin -safemode leave` (Disabling SafeMode of Hadoop)
    + `hive --service hiveserver2 start` (HiveServer2 service)
    + `hive` (Apache Hive)

<br/>

## Apache Pig Installation
* Download [Pig Binary (v0.17.0)](https://pig.apache.org/releases.html) Latest Version.
* **Note:** The `Apache Pig v0.17.0` supports `Hadoop 2.x` versions & it is facing some compatibility issues with `Hadoop 3.x`.
* Preferably extract Pig in the same directory where Hadoop is residing.
* Setting Environment Path Variables:
    + Set Variable as `PIG_HOME` & Value as `<Pig Root Path>`.
    + Add following path to `Path` Variable: `<Pig Bin Path>`
* Make change of `HADOOP_BIN_PATH` from `%HADOOP_HOME%\bin` to `%HADOOP_HOME%\libexec` in `pig.cmd` file located in `Pig Bin` folder.
* To check if pig is installed properly, run command: `pig -version`.
* The `PigLatin` statements can be run in two ways:
    + `Local`: All scripts are executed on a single machine without requiring Hadoop. `(command: pig -x local)`
    + `MapReduce`: Scripts are executed on a Hadoop cluster `(command: pig -x MapReduce)`

<br/>

**Note:**
* All the customs paths mentioned above has to be configured according your own system.
* All the above installation steps are collected from various source available on internet. I have just cumulated them together here.
* This guide may not be updated for later versions or other components of Apache.
* If there is any issues, please contact through [Email](mailto:rohitrocks2801@gmail.com) or If you want to contribute, create a pull request.
