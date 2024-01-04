# bigdata

Installing of Hadoop Single Node Cluster Configuration. 
1.Downloading the software Required 
2.Untar the software 
3.Bashrc configurations 
4.Hadoop Configuration File 
5.Share public key to localhost 
6.Formatting the name node 
7.Starting Hadoop Daemons 
8.Checking the working hadoop Daemons 
 
Step-1 
Hadoop 1.2.0-bin.tar.gz 
Jdk 7u67-linux-i586.tar.gz 
Link for JDK http://www.oracle.com/technetwork/java/javase/downloads/index.html Link of Hadoop : http://mirror.fibergrid.in/apache/hadoop/common/hadoop-1.2.0/ 
 
Step-2 
Open the terminal window 
Type the command : cd Desktop 
Type ls command 
tar  –zxvf  jdk-7u67-linux-i586.tar.gz 
tar –zxvf  hadoop-1.2.0-bin.tar.gz 
 
Step-3 
Open the terminal type the command 
sudo  gedit   ~/.bashrc 
 
export JAVA_HOME=/home/user/Desktop/jdk1.7.0_67 export PATH=$PATH:$JAVA_HOME/bin 
export HADOOP_HOME=/home/user/Desktop/hadoop-1.2.0 export PATH=$PATH:$HADOOP_HOME/bin 
 
Open new terminal and check for java version and Hadoop version. 
 
Step-4 
1. Core-site.xml 
(Configuration setting for hadoop core, such as I/O setting that are common to HDFS and Mapreduce) 2.Hdfs-Site.xml 
 (Configuration setting for HDFS daemons the name node, Secondary name node and data node and Replication factor as well) 
3.	Mapred-site.xml 
(configuration setting for MapReduce daemons: the job tracker and the task tracker) 
4.	Hadoop-Env_sh 
(Environment variables that are used in the scripts to run Hadoop) 
 
 
•	Open the new terminal window 
•	Go to the hadoop folder 
•	cd Desktop 
•	cd Hadoop (Press tab) for path 
•	cd conf 
•	Type ls 
•	All the above mentioned files are listed 
 
 
 
 
Go to terminal and type 
 
sudo gedit core-site.xml 
<configuration> 
<property> 
<name>fs.default.name</name> 
<value>hdfs://localhost:9000</value> 
</property> 
</configuration> 
Ip address :127.0.0.0 , 198.1.27.0 
Port no: 80,  
 
Go to terminal and type sudo gedit hdfs-site.xml 
<configuration> 
<property> 
<name>dfs.replication</name> 
<value>1</value> 
<name>dfs.name.dir</name> 
    <value>/home/user/Desktop/hadoop-1.2.0/name/data</value> 
</property> 
</configuration> 
 
Go to terminal and type sudo gedit mapred-site.xml 
<configuration> 
<property> 
<name>mapred.job.tracker</name> 
<value>hdfs://localhost:9001</value> 
</property> 
</configuration> 
 
Go to terminal and type sudo gedit hadoop-env.sh 
export JAVA_HOME=/home/user/Desktop/jdk1.7.0_67 
 
 
Step-5 
 
sudo apt-get install ssh ssh-keygen -t rsa  -P  "" 
                               Sharing the public key with host 
 
ssh-copy-id   –i   ~/.ssh/id_rsa.pub user@ubuntuvm and check ssh localhost (It should not ask password) 
 
Step-6 
$   hadoop namenode –format 
 
format a new distributed file system 
Process creates an empty file system for creating the storage directories Step7&8
Open the terminal window and type 
$ start-all.sh and type  
$ jps 
 
Hadoop dfs ls/ //deprecated 
Hadoop dfs –mkdir krisnhna/ 	//directory created 
Hadoop dfs –mkdir/Krishna/abc/ 
Now check if directory is there 
Hadoop dfs –ls/Krishna 
Found 1 item 
 
Copy any file n paste in desktop 
Hadoop dfs put  
 
 
user@ubuntuvm:~$ start-all.sh 
Warning: $HADOOP_HOME is deprecated. starting namenode, logging to /home/user/Downloads/hadoop-1.2.0/libexec/../logs/hadoop-user-namenodeubuntuvm.out localhost: datanode running as process 2978.  localhost: secondarynamenode running as process 3123.  jobtracker running as process 3204. localhost: tasktracker running as process 3342. 
user@ubuntuvm:~$ jps 
4020 Jps 
3342 TaskTracker 
3204 JobTracker 
3123 SecondaryNameNode 
3606 NameNode 
2978 DataNode 
 
 
Hadoop commands: 
Hadoop commands and linux commands almost same.  
before run this hdfs commands please download sample datasets from these links and save in work folder datasets  folder 
https://www.briandunning.com/sample-data/us-500.zip //extract this file and place in work/datasets folder 
 
 
hdfs dfs -mkdir  /datasets hdfs dfs -mkdir  /titanicdata hdfs dfs -mkdir  /otherdataset 
 
  
hdfs dfs -ls /Titanicdata 
//If you want to display recursively inside that folder, use -lsr or -ls -R 
 
 
 
 
 
MV also useful to rename the dataset let example 
 
 
 
 
 
 
 	9) hdfs dfs -rmr <path> 
Removes the file or directory identified by path. Recursively deletes any child entries 
 
 
Now local data is deleted 
 
12) 	 hdfs dfs -get [-crc] <src> <localDest> 
Copies the file or directory in HDFS identified by src to the local file system path identified by localDest. 
 
 
 
Word Count Program  
Wordcount.java  
 
import org.apache.hadoop.conf.Configured; import org.apache.hadoop.fs.Path; import org.apache.hadoop.io.IntWritable; import org.apache.hadoop.io.Text; import org.apache.hadoop.mapred.FileInputFormat; import org.apache.hadoop.mapred.FileOutputFormat; import org.apache.hadoop.mapred.JobClient; import org.apache.hadoop.mapred.JobConf; import org.apache.hadoop.util.Tool; 
import org.apache.hadoop.util.ToolRunner; 
 
public class wordcount extends Configured implements Tool { 
 
 	@Override 
 	public int run(String[] args) throws Exception { 
 	 
 	if(args.length<2) 
 	{ 
 	 	System.out.println("Plz Give Input Output Directory Correctly"); 
 	 	return -1; 
 	} 
 	 	 
 	JobConf conf = new JobConf(wordcount.class); 
 	 
 	FileInputFormat.setInputPaths(conf,new Path(args[0])); 
 	FileOutputFormat.setOutputPath(conf, new Path(args[1])); 
 	 
 	conf.setMapperClass(wordmapper.class); 
 	conf.setReducerClass(wordreducer.class); 
 	 
 	conf.setMapOutputKeyClass(Text.class);  	conf.setMapOutputValueClass(IntWritable.class);  	conf.setOutputKeyClass(Text.class); 
 	conf.setOutputValueClass(IntWritable.class); 
 
 	JobClient.runJob(conf);  	return 0; 
 	} 
 
 	public static void main(String args[]) throws Exception 
 	{ 
 	 	int exitcode = ToolRunner.run(new wordcount(), args); 
 	 	System.exit(exitcode); 
 	} 
 
} 
 
 
 
 
 
wordmapper.java 
import java.io.IOException; 
 
import org.apache.hadoop.io.IntWritable; import org.apache.hadoop.io.LongWritable; import org.apache.hadoop.io.Text; 
import org.apache.hadoop.mapred.MapReduceBase; import org.apache.hadoop.mapred.Mapper; import org.apache.hadoop.mapred.OutputCollector; import org.apache.hadoop.mapred.Reporter; 
 
public class wordmapper extends MapReduceBase implements Mapper<LongWritable,Text,Text,IntWritable> 
{ 
 	public void map(LongWritable key, Text value, 
 	 	 	OutputCollector<Text, IntWritable> output, Reporter r) 
 	 	 	throws IOException  
 	{ 
 	 	String s =value.toString(); 
 	 	for(String word:s.split(" ")) 
 	 	{ 
 	 	 	if(word.length()>0) 
 	 	 	{ 
 	 	 	 	output.collect(new Text(word), new IntWritable(1)); 
 	 	 	} 
 	 	} 
 	} 
} 
 
wordreducer.java 
 
import java.io.IOException; 
import java.util.Iterator; 
 
import org.apache.hadoop.io.IntWritable; import org.apache.hadoop.io.Text; 
import org.apache.hadoop.mapred.MapReduceBase; import org.apache.hadoop.mapred.OutputCollector; import org.apache.hadoop.mapred.Reducer; 
import org.apache.hadoop.mapred.Reporter; 
 
public class wordreducer extends MapReduceBase implements Reducer<Text,IntWritable,Text,IntWritable> 
{ 
 	public void reduce(Text key, Iterator<IntWritable> values, 
 	 	 	OutputCollector<Text, IntWritable> output, Reporter r)  	 	 	throws IOException { 	  	int count=0;  	while(values.hasNext()) 
 	{ 
 	 	IntWritable i= values.next(); 
 	 	count+= i.get(); 
 	} 
 	output.collect(key, new IntWritable(count)); 
 	} 
} 
1.	Add Hadoop Java jar file 
           File – new – Java Project – Name:wc1 – finish 
           Wc1 – right Click – Build Path – configure BD – Libraries – Add external jars – Select Desktop –            Hadoop_java_jars – ctrl A – ok – ok  
 
2.	Create 3 classes 
 
•	Right Click On Project(wc1) – new – class – Name:wordcountMain – method – public SVM – finish 
•	Right Click On Project(wc1) – new – class – Name:wcreducer – method – inherited…. • 	Right Click On Project(wc1) – new – class – Name:wcmapper – method – inherited…. 
 
3.	Create Jar Files 
 
Right Click On Project(wc1) – Export – jar Files – Path – Browse – Select directory (desktop) – press ok  Give File Name: WC.jar – ok – finish 
 
4.	To upload input file from local system to hdfs directory: 
 
1.	Open The Terminal  and type 
2.	hadoop dfs -mkdir /WCA 
Name node is in safe mode. 
3.	hadoop dfsadmin -safemode leave 
4.	hadoop dfs –put /home/training/Desktop/words1.txt /WCA/w1 
5.	hadoop jar /home/training/Desktop/wcA.jar wordcountMain /WCA/w1 /wordcountout 
To Check The Output – 
Open Firefox ad Browse and type  
Hadoop Adminstration 
Browse the file System 
 
 
 
 
 
 
 
 
 
 
 
Stock Analysis MaxClosePrice.java 
import org.apache.hadoop.fs.Path; import org.apache.hadoop.conf.Configured; import org.apache.hadoop.io.FloatWritable; import org.apache.hadoop.io.Text; import org.apache.hadoop.mapred.FileInputFormat; import org.apache.hadoop.mapred.FileOutputFormat; import org.apache.hadoop.mapred.JobClient; import org.apache.hadoop.mapred.JobConf; import org.apache.hadoop.util.Tool; 
import org.apache.hadoop.util.ToolRunner; 
 
public class MaxClosePrice extends Configured implements Tool { 
 	 
 	public int run(String[] args) throws Exception { 
 	 	if(args.length<2) 
 	 	{ 
 	 	 	System.out.println("Plz Give Input Output Directory Correctly"); 
 	 	 	return -1; 
 	 	} 
 	 	JobConf conf = new JobConf(MaxClosePrice.class); 
 	 	FileInputFormat.setInputPaths(conf,new Path(args[0]));  	 	FileOutputFormat.setOutputPath(conf, new Path(args[1]));  	 	conf.setMapperClass(MaxClosePriceMapper.class);  	 	conf.setReducerClass(MaxClosePriceReducer.class);  	 	conf.setMapOutputKeyClass(Text.class);  	 	conf.setMapOutputValueClass(FloatWritable.class);  	 	conf.setOutputKeyClass(Text.class); 
 	 	conf.setOutputValueClass(FloatWritable.class); 
 	 	JobClient.runJob(conf); 
 	 	return 0; 
 	 	} 
 	 
 	public static void main(String[] args) throws Exception { 
 	 	int exitcode = ToolRunner.run(new MaxClosePrice(), args); 
 	 	System.exit(exitcode); 
 	 	} 
} 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
MaxClosePriceReducer.java 
 
/** 
 * MaxClosePriceReducer.java 
 */ 
 import java.io.IOException; 
 import java.util.Iterator; 
  
 import org.apache.hadoop.io.FloatWritable;  import org.apache.hadoop.io.Text;  import org.apache.hadoop.mapred.MapReduceBase;  import org.apache.hadoop.mapred.OutputCollector;  import org.apache.hadoop.mapred.Reducer; 
 import org.apache.hadoop.mapred.Reporter; 
 
 public class MaxClosePriceReducer extends MapReduceBase implements 
Reducer<Text,FloatWritable,Text,FloatWritable> 
 { 
 
 	 @Override 
 	 public void reduce(Text key, Iterator<FloatWritable> values,  
 	 	 	 OutputCollector<Text, FloatWritable> output, Reporter r) 
 	 	 	 throws IOException { 
 
 	 	 float maxClosePrice = 0;  
 	 	  
 	 	 //Iterate all and calculate maximum  	 	 while (values.hasNext()) {  	 	 	 FloatWritable i = values.next(); 
 	 	 	 maxClosePrice = Math.max(maxClosePrice, i.get()); 
 	 	 } 
 	 	  
 	 	 //Write output 
 	 	 output.collect(key, new FloatWritable(maxClosePrice)); 
 	 } 
 } 
 
 
 
 
 
 
 
 
 
  
 
 
MaxClosePriceMapper.java 
 
/** 
 * MaxClosePriceMapper.java 
 */ 
 
import java.io.IOException; 
 
import org.apache.hadoop.io.FloatWritable; import org.apache.hadoop.io.LongWritable; import org.apache.hadoop.io.Text; import org.apache.hadoop.mapred.MapReduceBase; import org.apache.hadoop.mapred.OutputCollector; import org.apache.hadoop.mapred.Reporter; 
import org.apache.hadoop.mapred.Mapper; 
 
public class MaxClosePriceMapper extends MapReduceBase implements Mapper<LongWritable, Text, Text, FloatWritable> 
{ 
 
 	@Override 
 	public void map(LongWritable key, Text value,  
 	 	 	OutputCollector<Text, FloatWritable> output, Reporter r) 
 	 	 	 	 	throws IOException { 
 
 	 	String line = value.toString(); 
 	 	String[] items = line.split(","); 
 	 	 
 	 	String stock = items[1]; 
 	 	Float closePrice = Float.parseFloat(items[6]); 
 	 	 
 	 	output.collect(new Text(stock), new FloatWritable(closePrice)); 
 	 	 
 	} 
} 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
Log Analysis 
 
Logmapper.java 
import java.io.IOException; 
 
import org.apache.hadoop.io.IntWritable; import org.apache.hadoop.io.LongWritable; import org.apache.hadoop.io.Text; import org.apache.hadoop.mapred.MapReduceBase; import org.apache.hadoop.mapred.Mapper; import org.apache.hadoop.mapred.OutputCollector; import org.apache.hadoop.mapred.Reporter; 
 
public class LogMapper extends MapReduceBase implements Mapper<LongWritable,Text,Text,IntWritable> 
{ 
 	public void map(LongWritable key, Text value, 
 	 	 	OutputCollector<Text, IntWritable> output, Reporter r) 
 	 	 	throws IOException { 
 	 	 
 	 	String[] s = value.toString().split(" "); 
 	 	String ip = s[0]; 
 	 	 
 	 	output.collect(new Text(ip), new IntWritable(1)); 
 	} 
 
} 
 
Logreducer 
 
import java.io.IOException; import java.util.Iterator; import org.apache.hadoop.io.IntWritable; import org.apache.hadoop.io.Text; import org.apache.hadoop.mapred.MapReduceBase; import org.apache.hadoop.mapred.OutputCollector; import org.apache.hadoop.mapred.Reducer; 
import org.apache.hadoop.mapred.Reporter; 
 
public class LogReducer extends MapReduceBase implements Reducer<Text,IntWritable,Text,IntWritable> 
{ 
 
 	 
 	public void reduce(Text key, Iterator<IntWritable> values, 
 	 	 	OutputCollector<Text, IntWritable> output, Reporter r)  	 	 	throws IOException { 
 	 	 
 	int count=0;  	while(values.hasNext()) 
 	{ 
 	 	IntWritable i= values.next(); 
 	 	count+= i.get(); 
 	} 
 	output.collect(key, new IntWritable(count)); 
 	 	 
 	} 
 
}
Processlogs 
 
import java.io.*; import org.apache.hadoop.conf.Configured; import org.apache.hadoop.fs.FileSystem; import org.apache.hadoop.fs.Path; import org.apache.hadoop.io.IntWritable; import org.apache.hadoop.io.Text; import org.apache.hadoop.mapred.FileInputFormat; import org.apache.hadoop.mapred.FileOutputFormat; import org.apache.hadoop.mapred.JobClient; import org.apache.hadoop.mapred.JobConf; import org.apache.hadoop.util.Tool; 
import org.apache.hadoop.util.ToolRunner; 
 
public class ProcessLogs extends Configured implements Tool { 
 
 	@Override 
 	public int run(String[] args) throws Exception { 
 	 
 	if(args.length<2) 
 	{ 
 	 	System.out.println("Plz Give Input Output Directory Correctly"); 
 	 	return -1; 
 	} 
 	 	 
 	JobConf conf = new JobConf(ProcessLogs.class); 
 	FileInputFormat.setInputPaths(conf,new Path(args[0]));  	FileOutputFormat.setOutputPath(conf, new Path(args[1])); 
 	conf.setMapperClass(LogMapper.class);  	conf.setReducerClass(LogReducer.class);  	conf.setMapOutputKeyClass(Text.class);  	conf.setMapOutputValueClass(IntWritable.class);  	conf.setOutputKeyClass(Text.class);  	conf.setOutputValueClass(IntWritable.class); 
 	JobClient.runJob(conf); 
 	return 0; 
 	} 
 
 	public static void main(String args[]) throws Exception 
 	{ 
 	 	int exitcode = ToolRunner.run(new ProcessLogs(), args); 
 	 	System.exit(exitcode); 
 	} 
 
} 
 
WebLog 
96.7.8.17 - - [24/Apr/2011:04:20:11 -0400] "GET /cat.jpg HTTP/1.1" 200 12433  96.7.1.14 - - [24/Apr/2011:04:20:11 -0400] "GET /cat.jpg HTTP/1.1" 200 12433  
96.7.2.14 - - [24/Apr/2011:04:20:11 -0400] "GET /cat.jpg HTTP/1.1" 200 12433  
192.168.1.1 - - [24/Apr/2011:04:20:11 -0400] "GET /cat.jpg HTTP/1.1" 200 12433  
96.7.4.16 - - [24/Apr/2011:04:20:11 -0400] "GET /cat.jpg HTTP/1.1" 200 12433  
91.75.5.14 - - [24/Apr/2011:04:20:11 -0400] "GET /cat.jpg HTTP/1.1" 200 12433  
98.21.6.14 - - [24/Apr/2011:04:20:11 -0400] "GET /cat.jpg HTTP/1.1" 200 12433  
162.15.16.1 - - [24/Apr/2011:04:20:11 -0400] "GET /cat.jpg HTTP/1.1" 200 12433  
8.8.8.8  - - [24/Apr/2011:04:20:11 -0400] "GET /cat.jpg HTTP/1.1" 200 12433  Temp by States 
 
MaxClosePrice.java 
 
import org.apache.hadoop.fs.Path; import org.apache.hadoop.conf.Configured; import org.apache.hadoop.io.FloatWritable; import org.apache.hadoop.io.Text; import org.apache.hadoop.mapred.FileInputFormat; import org.apache.hadoop.mapred.FileOutputFormat; import org.apache.hadoop.mapred.JobClient; import org.apache.hadoop.mapred.JobConf; import org.apache.hadoop.util.Tool; 
import org.apache.hadoop.util.ToolRunner; 
 
public class maxcloseprice extends Configured implements Tool { 
 	 
 	public int run(String[] args) throws Exception { 
 	 	if(args.length<2) 
 	 	{ 
 	 	 	System.out.println("Plz Give Input Output Directory Correctly"); 
 	 	 	return -1; 
 	 	} 
 	 	JobConf conf = new JobConf(maxcloseprice.class); 
 	 	FileInputFormat.setInputPaths(conf,new Path(args[0]));  	 	FileOutputFormat.setOutputPath(conf, new Path(args[1]));  	 	conf.setMapperClass(maxclosepricemapper.class);  	 	conf.setReducerClass(maxclosepricereducer.class);  	 	conf.setMapOutputKeyClass(Text.class);  	 	conf.setMapOutputValueClass(FloatWritable.class);  	 	conf.setOutputKeyClass(Text.class); 
 	 	conf.setOutputValueClass(FloatWritable.class); 
 	 	JobClient.runJob(conf); 
 	 	return 0; 
 	 	} 
 	 
 	public static void main(String[] args) throws Exception { 
 	 	int exitcode = ToolRunner.run(new maxcloseprice(), args); 
 	 	System.exit(exitcode); 
 	 	} 
} 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
MaxClosePriceMapper.java 
 
import java.io.IOException; import org.apache.hadoop.io.FloatWritable; import org.apache.hadoop.io.LongWritable; import org.apache.hadoop.io.Text; import org.apache.hadoop.mapred.MapReduceBase; import org.apache.hadoop.mapred.OutputCollector; import org.apache.hadoop.mapred.Reporter; 
import org.apache.hadoop.mapred.Mapper; 
 
public class maxclosepricemapper extends MapReduceBase implements Mapper<LongWritable, Text, Text, FloatWritable> 
{ 	 	 	 	 	 	 	 	 	 	                      	 
 	@Override 
 	public void map(LongWritable key, Text value,  
 	 	 	OutputCollector<Text, FloatWritable> output, Reporter r) 
 	 	 	 	 	throws IOException { 
 
 	 	String line = value.toString(); 
 	 	String[] items = line.split(","); 
 	 	 
 	 	String stock = items[3]; 
 	 	if(!items[1].isEmpty() && !items[1].equals(null))  
 	 	{ 
 	 	 	Float closePrice = Float.parseFloat(items[1]); 
 	 	    output.collect(new Text(stock), new FloatWritable(closePrice)); 
 	 	} 
} 
} 
 
MaxClosePriceReducer.java 
 
import java.io.IOException;  import java.util.Iterator; 
 import org.apache.hadoop.io.FloatWritable;  import org.apache.hadoop.io.Text; 
 import org.apache.hadoop.mapred.MapReduceBase;  import org.apache.hadoop.mapred.OutputCollector;  import org.apache.hadoop.mapred.Reducer; 
import org.apache.hadoop.mapred.Reporter; 
 
 public class maxclosepricereducer extends MapReduceBase implements Reducer<Text,FloatWritable,Text,FloatWritable> 
 { 
 	 @Override 
 	 public void reduce(Text key, Iterator<FloatWritable> values,  
 	 	 	 OutputCollector<Text, FloatWritable> output, Reporter r) 
 	 	 	 throws IOException { 
          
   float maxClosePrice=Float.MAX_VALUE;      while (values.hasNext()) {     FloatWritable i = values.next(); 
 	 	 	 maxClosePrice = Math.min(maxClosePrice, i.get()); 
 	 	 } 
 	 	 output.collect(key, new FloatWritable(maxClosePrice)); 
 }  } 
Hive 1---DDL, Create,datatypes (SQL), string,int,varchar,date 
 
open terminal and type hive 
>hive 
 
 
1.	To display the existing databases: 
show databases; 
 
2.	To create a new database: (Database name is "demo") create database demo; create database student_163; 
 
3.	To select a required database: (Selected database is "demo") use demo; 
 
4.	To create a table in the selected database: (Table name is "student") 
 
create table student(name string,rollno string,age int)row format delimited fields terminated by ','; 
 
5.	To insert data into table "student" from a file "Student1" that is stored in local file system: 
 
load data local inpath '/home/training/Desktop/Student1' into table student; select * from student; converted to mapreduce jobs  
 
6.	To insert some more data into table "student" from a file "Student2" that is stored in local file system: 
load data local inpath '/home/training/Desktop/Student2' into table student; 
 
7.	To insert some more data into table "student" from a file "Student3" that is stored in local file system: (Student 3 has some field types different from what the schema is defined) 
load data local inpath '/home/training/Desktop/Student3' into table student; 
 
8.	To display metadata about the table: 
describe student; 
 
9.	To display detailed metadata about the table: 
describe extended student; 
 
10.	To insert data into table "student" from a file "Student4" that is stored in hdfs: 
   First load the data from local file sytem into hdfs. This can be done using the following command: 
     
   $ cd desktop 
   $ hadoop dfs -put stu4 /student4.txt 
   Then load the data from hdfs into hive table:  
   load data inpath '/student4.txt' into table student; 
Note: once loaded the file "Student4" will be moved from its path into given hive table "student" 
 
11.	To display the contents of table "student": 
select count(*) from student; 
 
12.	To display the name "Akash" in uppercase: 
select upper(name) from student where name='Akash'; 
 
Execute some more select with aggregate functions 
13.	To create external table "studextern" in the path "/hivedata":      create external table studextern( 
     name string,                           rollno string,                         age int                           
     )row format delimited                  fields terminated by ','          
     location '/hivedata';   
 
14.	To insert data into external table "studextern":     hadoop dfs -put Student1 /hivedata 
    Note: Just load the file into "/hivedata". All the contents of the file will be inserted into the     table "studextern" 
 
15.	To display the contents of table "studextern"      select * from studextern; 
 
Note: Internal table will be in "/user/hive/warehouse" 
Note: External table will be in the path where the data is stored. 
 
16.	To remove internal table "student": 
    drop table student; 
 
17.	To remove external table "studextern" 
    drop table studextern; 
 
18.	To remove database 
    drop database demo; 
 
Note: When external table is dropped its schema is dropped but data still exists. Can be checked in browser. Note: When internal table is dropped its schema and also data is deleted. 
 
Student1: ravi krishna,2016cse100,25 
mohan,2016cse095,22 mahesh,2016cse102,23 
 
student2: Rajesh,2016cse100,25 
Abhishek,2016cse095,22 
Manish,2016cse102,23 
 
Student3: jagadish,2016cse100,hello 
Akash,2016cse095,hai 
Manjunath,2016cse102,23 
 
Student4: 
jagadish,2016cse100,30 Akash,2016cse095,44 
Manjunath,2016cse102,23 
 
 
 
 
 
 
 
 
Hive 2---Partitions, Buckets 
 
1.	create database part; 
 
2.	use part; 
 
Static Partitioning 
3.	create table student (name string, rollno int, percentage float) 
    partitioned by (state string, city string)                       
    row format delimited                                                 fields terminated by ',';   
 
4.	load data local inpath '/home/training/Desktop/Maharastra' into table student partition(state='maharastra', city='mumbai'); 
 
5.	select * from student; 
 
6.	load data local inpath '/home/training/Desktop/karnataka' into table student partition(state='karnataka', city='Bengaluru'); 
 
7.	select * from student; 
 
8.	select * from student limit 6; 
 
9.	select * from student where state='maharastra'; 
 
Dynamic partioning 
Note: By default dynamic partioning will be disabled. We need to enable it using the followng command: 
10.	set hive.exec.dynamic.partition=true; 
11.	set hive.exec.dynamic.partition.mode=nonstrict; 
 
12.	create table stu(name string, rollno int, percentage float, state string, city string) row format delimited fields terminated by ',';  
 
13.	load data local inpath '/home/training/Desktop/Result1' into table stu; 
 
14.	create table stud_part (name string, rollno int, percentage float) 
    partitioned by (state string, city string)                       
    row format delimited                                                 fields terminated by ',';   
 
15.	insert overwrite table stud_part 
partition (state, city) 
select name,rollno, percentage 
,state, city from stu; 
 
16.	select * from stud_part where city='tumkur'; 
 
 
 
 
 
 
 
Bucketing note: To enable bucketing first execute the following command in hive prompt: 
hive> set hive.enforce.bucketing=true;    
 
1.	create database buck; 
 
2.	use buck; 
 
3.	create table employee(id int, fname string, lname string) row format delimited fields terminated by ',';  
 
4.	load data local inpath '/home/training/Desktop/emp' into table employee; 
 
5.	create table buck_table(id int, fname string, lname string) clustered by (id) into 5 buckets row format delimited fields terminated by ','; 
 
6.	insert overwrite table buck_table select * from employee; 
 
Emp: 19,avinash,chaudary 
20,sai,naidu 
33,rajanish,krishna 
14,mahesh,rao, 
58,rajesh,naik 
67,himesh,nair 
72,himanshu,patel 
28,tittu,charkravarthy 
90,lalitha,patil 
10,yash,kariappa 
11,anmol,patel 
12,jyotsana,desh 88,akshay,kumar 
14,akshay,Khanna 
 
Karnataka: ravi,100,82 abhishek,400,50 ganesh,125,84 
bhavesh,425,78 
 
Maharashtra: rajesh,300,79 ramesh,150,72 bindu,325,66 
dhruv,175,55 
 
Result1: 
ravi,100,82,karnataka,bengaluru mohan,200,80,Punjab,amritsar rajesh,300,79,Maharastra,nagpur ramesh,150,72,Maharastra,Mumbai manish,250,45,Punjab,chandigarh abhishek,400,50,karnataka,tumkur akash,350,45,Punjab,amritsar raju,450,60,Punjab,amritsar ganesh,125,84,karnataka,bengaluru bindu,325,66,Maharastra,nagpur sai,225,77,Punjab,chandigarh bhavesh,425,78,karnataka,tumkur dhruv,175,55,Maharastra,Mumbai 
 
                                         HBASE Commands 
 
Objective: To create and execute hbase commands for the following operations 
 
>hbase shell -- open hbase terminal 
 
1. >status  -- status 'simple'                      status 'detailed' 
 
    >version -- current version 
 
2) Create table  
 
create <tablename>, <columnfamilyname1> <columnfamilyname2> 
 
>create 'emp', 'personal data', 'professional data'    
 
put 'emp','1','personal data:name','raju' put 'emp','1','personal data:city','hyderabad' put 'emp','1','professional data:designation','manager' put 'emp','1','professional data:salary','50000' put 'emp','1','professional data:vechiv','50000'  put 'emp','2','personal data:name','sathish' put 'emp','2','personal data:city','bangalore' put 'emp','2','professional data:designation','professor' put 'emp','2','professional data:salary','60000' put 'emp','3','personal data:name','muthu' put 'emp','3','personal data:city','chennai' put 'emp','3','professional data:designation','analyst' put 'emp','3','professional data:salary','20000' 
 
3.	To List all the tables 
                   
                                     list  
 
4.	To describe the table. 
 
     	 	 	 	 describe 'emp'  
 
5.	To delete a table or change its settings, you need to first disable the table using the disable command. 
 
  disable 'emp' 
 
  is_disabled  command is used to find whether a table is disabled. 
 
6.	To Drop the table 
 
                                     drop 'emp'  
7.	To find whether a table is enabled. 
                                                                               enable 'emp'  
 
      The following commands verifies whether the table named emp is enabled. If it is enabled, it will return true and if not, it will return false 
 
8.	Scan command is used to view the data in HBASETable. Using the scan command, you can get the table data.  
                           
                               scan 'emp'  
 
9 Using the drop command, you can delete a table. Before dropping a table, you have to disable it. 
 
hbase(main):018:0> disable 'emp' 
0 row(s) in 1.4580 seconds 
 
hbase(main):019:0> drop 'emp' 
0 row(s) in 0.3060 seconds 
 
 
10.	To get the particular record 
 
                get method > get 'emp', '1' 
 
11.	To delete the particular record 
 
  delete ‘<table name>’, ‘<row>’, ‘<column name >’, ‘<time stamp>’ 
                   
            delete 'emp', '1', 'personal data: city’,1417521848375 
 
 
12.	You can count the number of rows of a table using the count command. Its syntax is as follows: 
 
                  count ‘<table name>’  
                   count 'emp' 
 
13.	This command disables drops and recreates a table. The syntax of truncate is as follows: 
           hbase> truncate 'table name' 
  
                    truncate 'emp' 
 
 
Outcome: 
 
  Given HBASE commands are created and executed successfully  
 
 
 
