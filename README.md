# Install Elastic Search on Computer

**Pre-requisite:** Latest JDK

Download Zip from https://www.elastic.co/downloads/elasticsearch and extract it somewhere on computer

Go to folder where you have extracted Elastic Search and go to config folder of the installation:
E.g. C:\DDrive\MyData\SWs\Elastic\elasticsearch-6.2.4\config

Edit elasticsearch.yml and edit following entries. You can choose any name you want.

```
cluster.name: Packt
node.name: Packtnode
```

Open command prompt in: 
C:\DDrive\MyData\SWs\Elastic\elasticsearch-6.2.4\bin

And run batch file: elasticsearch-service.bat
```
C:\DDrive\MyData\SWs\Elastic\elasticsearch-6.2.4\bin>elasticsearch-service.bat install
Installing service      :  "elasticsearch-service-x64"
Using JAVA_HOME (64-bit):  "C:\Program Files\Java\jdk-9.0.4" -Xms1g;-Xmx1g;-XX:+UseConcMarkSweepGC;-XX:CMSInitiatingOccupancyFraction=75;-XX:+UseCMSInitiatingOccupancyOnly;-XX:+AlwaysPreTouch;-Xss1m;-Djava.awt.headless=true;-Dfile.encoding=UTF-8;-Djna.nosys=true;-XX:-OmitStackTraceInFastThrow;-Dio.netty.noUnsafe=true;-Dio.netty.noKeySetOptimization=true;-Dio.netty.recycler.maxCapacityPerThread=0;-Dlog4j.shutdownHookEnabled=false;-Dlog4j2.disable.jmx=true;-Djava.io.tmpdir=C:\Users\naiky\AppData\Local\Temp\elasticsearch;-XX:+HeapDumpOnOutOfMemoryError;-Xlog:gc*,gc+age=trace,safepoint:file=logs/gc.log:utctime,pid,tags:filecount=32,filesize=64m;-Djava.locale.providers=COMPAT
    The service 'elasticsearch-service-x64' has been installed.
```

You should see elasticsearch-service-x64 under "Services".

Start the service using command:

```
C:\DDrive\MyData\SWs\Elastic\elasticsearch-6.2.4\bin>elasticsearch-service.bat start
The service 'elasticsearch-service-x64' has been started
```

Open http://localhost:9200/ in browser to confirm if installation is ok.
Response:

```javascript
    {
		name: "Packtnode",
		cluster_name: "Packt",
		cluster_uuid: "bjGjfwUxRlakxGJTqo1C4A",
		version: {
			number: "6.2.4",
			build_hash: "ccec39f",
			build_date: "2018-04-12T20:37:28.497551Z",
			build_snapshot: false,
			lucene_version: "7.2.1",
			minimum_wire_compatibility_version: "5.6.0",
			minimum_index_compatibility_version: "5.0.0"
		},
		tagline: "You Know, for Search"
	}
```

# Install Logstash and Kibana on Computer

**Pre-requisite:** Latest JDK

Download Logstash zip from https://www.elastic.co/downloads/logstash

Download Kibana zip from https://www.elastic.co/downloads/kibana

For Windows, the option to run Logstash and Kibana as a service isn't available by default.
In order to run the Logstash and Kibana instance as a Windows service, we need to have a software named nssm which stands for Non-Sucking Service Manager installed in our system.
Download it from https://nssm.cc/download

## Testing configuration

Create a file called "LogstashPipeline.conf" under "bin" folder of Logstash.
C:\DDrive\MyData\SWs\Elastic\logstash-6.2.4\bin

```javascript
    input 
    {
	    file 
		{
			path => "C:\DDrive\MyData\SWs\Elastic\logstash-input.log"
			start_position => "beginning"
		}
	}
	output
	{
		file 
		{
			path => "C:\DDrive\MyData\SWs\Elastic\logstash-output.log"
		}	
	}
```

Then test this conf file using command:

```
C:\DDrive\MyData\SWs\Elastic\logstash-6.2.4\bin>logstash -f LogstashPipeline.conf --config.test_and_exit
    Sending Logstash's logs to C:/DDrive/MyData/SWs/Elastic/logstash-6.2.4/logs which is now configured via log4j2.properties
[2018-07-12T12:33:42,441][INFO ][logstash.modules.scaffold] Initializing module {:module_name=>"fb_apache", :directory=>"C:/DDrive/MyData/SWs/Elastic/logstash-6.2.4/modules/fb_apache/configuration"}
[2018-07-12T12:33:42,464][INFO ][logstash.modules.scaffold] Initializing module {:module_name=>"netflow", :directory=>"C:/DDrive/MyData/SWs/Elastic/logstash-6.2.4/modules/netflow/configuration"}
[2018-07-12T12:33:42,558][INFO ][logstash.setting.writabledirectory] Creating directory {:setting=>"path.queue", :path=>"C:/DDrive/MyData/SWs/Elastic/logstash-6.2.4/data/queue"}
[2018-07-12T12:33:42,564][INFO ][logstash.setting.writabledirectory] Creating directory {:setting=>"path.dead_letter_queue", :path=>"C:/DDrive/MyData/SWs/Elastic/logstash-6.2.4/data/dead_letter_queue"}
[2018-07-12T12:33:42,694][WARN ][logstash.config.source.multilocal] Ignoring the 'pipelines.yml' file because modules or command line options are specified Configuration OK
[2018-07-12T12:33:44,711][INFO ][logstash.runner          ] Using config.test_and_exit mode. Config Validation Result: OK. Exiting Logstash
```

If you get any error make sure you have correct version of JDK and JRE on classpath, compatible with Logstash version you are running.

## Install Logstash using NSSM
Go to appropriate folder where NSSM is downloaded.
Go to folder for your OS type.
E.g. C:\DDrive\MyData\SWs\Elastic\nssm-2.24\win64
Open command prompt (in Admin mode) in this folder.

Type command:
```
C:\DDrive\MyData\SWs\Elastic\nssm-2.24\win64>nssm install Logstash
```

This will open following prompt.

![nssm](https://raw.githubusercontent.com/yogeshrnaik/ELK-stack/master/images/nssm.jpg)

Select the path of "C:\DDrive\MyData\SWs\Elastic\logstash-6.2.4\bin\logstash.bat" file for Path.
Enter the argument as shown in screen shot below.

![Logstash](https://raw.githubusercontent.com/yogeshrnaik/ELK-stack/master/images/nssm-logstash.jpg)

And then click on "Install service". Once installed successfully, you can verify under "Services" that Logstash is installed as a service.

You can start the Logstash Service from Services.

To test if Logstash Pipeline works, open the file "logstash-input.log" and add new entries in this file.
If everything works ok, then you should see entries reflected in "logstash-output.log"

```
{"@timestamp":"2018-07-12T08:04:54.967Z","message":"Hello Elastic Stack\r","@version":"1","host":"IN1WXL-301034","path":"C:\\DDrive\\MyData\\SWs\\Elastic\\logstash-input.log"}
{"@timestamp":"2018-07-12T08:05:23.219Z","message":"Hello Logstash\r","@version":"1","host":"IN1WXL-301034","path":"C:\\DDrive\\MyData\\SWs\\Elastic\\logstash-input.log"}
{"@timestamp":"2018-07-12T08:06:22.648Z","message":"Hello Kibana\r","@version":"1","host":"IN1WXL-301034","path":"C:\\DDrive\\MyData\\SWs\\Elastic\\logstash-input.log"}
```

# Install Logstash using NSSM
Go to appropriate folder where NSSM is downloaded.
Go to folder for your OS type.
E.g. C:\DDrive\MyData\SWs\Elastic\nssm-2.24\win64
Open command prompt (in Admin mode) in this folder.

Type command:
```
C:\DDrive\MyData\SWs\Elastic\nssm-2.24\win64>nssm install Kibana
```

This will open following prompt.

![nssm](https://raw.githubusercontent.com/yogeshrnaik/ELK-stack/master/images/nssm.jpg)

Select the path of "C:\DDrive\MyData\SWs\Elastic\kibana-6.2.4-windows-x86_64\bin\kibana.bat" file for Path.

Enter the argument as shown in screen shot below.
![Kibana](https://raw.githubusercontent.com/yogeshrnaik/ELK-stack/master/images/nssm-kibana.jpg)

And then click on "Install service". Once installed successfully, you can verify under "Services" that Kibana is installed as a service.

You can start the Kibana Service from Services.
Go to: http://localhost:5601/ in order to verify if Kibana is running or not.

If you stop Elastic Search service and go to Kibana > Management page, you can see the status as Red (see screen-shot below).

![Kibana Status](https://raw.githubusercontent.com/yogeshrnaik/ELK-stack/master/images/kibana-without-elasticsearch-running.png)

# Elastic Search Concepts
## Cluster and Node
We can create cluster of Elastic Search nodes to create highly available environment.

To create a cluster of Elastic Search nodes, in "elasticsearch.yml" file we have to mention the "cluster.name" and "node.name" params before starting Elastic Search.

#### Types of Nodes in Elastic Search
 - **Master**
	 - Works as Supervisor all other nodes in same cluster. 
	 - Responsible for creating/deleting index, tracking which nodes are part of cluster, allocation of shards to other nodes.

- **Master eligible node**
	- There is a property in "elasticsearch.yml" called "node.master"
	- This property is true by default. 
	- If true, it means the node can be elected as master node
	- If current master node fails, then all nodes in cluster go through a process called "Master election" and they choose one of the master eligible nodes as their master.

- **Data node**
	- This node holds data and performs data related operations such as CRUD, search and aggregation
	- To make a node as Data node, the property in "elasticsearch.yml" called "node.data" must be set to true. This property is by default true.

- **Ingest node**
	- This node pre-processes the document before actual indexing takes place.
	- To make a node as Ingest node, the property in "elasticsearch.yml" called "node.ingest" must be set to true. This property is by default true.

- **Tribe node**
	- It is special type of node and used for coordination purpose only.
	- This node can connect to multiple clusters and can perform search and other operations across connected clusters.

## Elastic Search API
Elastic Search provides a rich API to perform all kinds of operations. 
These APIs are mainly classified into following types:

 1. Cluster APIs
 2. Indices APIs
 3. Document APIs
 4. CAT APIs - Used to provide human readable response
 5. Search APIs - Used to search across index

#### Example of Cluster API
Go to Kibana URL http://localhost:5601 and go to Dev Tools to access SENSE application.
SENSE application is used to try out different APIs.
For example, to check the health of Cluster, type following command in SENSE application.

```
GET _cluster/health
```

When you click on the Play icon next to this command, you can see the response in adjacent window.

![enter image description here](https://raw.githubusercontent.com/yogeshrnaik/ELK-stack/master/images/cluster_health.jpg)

The response is JSON and is normally human readable to most of the people. 

Using CAT API we can align the JSON properly and make it more human readable.

Open http://localhost:9200/_cat/health?v in any browser window and it will show a table with the health status.

```
epoch      timestamp cluster status node.total node.data shards pri relo init unassign pending_tasks max_task_wait_time active_shards_percent
1531392586 16:19:46  Packt   yellow          1         1     26  26    0    0       25             0                  -                 51.0%
```
We can get details of the nodes using:
```
GET _nodes
```

#### Example of Indices API
To create index use the PUT as shown below.

```javascript
PUT movies
{
	"settings": 
	{
		"index":
		{
			"number_of_shards": 5,
			"number_of_replicas": 2
		}
	}
}
```

This will create index called "movies".
Response:

```javascript
{
  "acknowledged": true,
  "shards_acknowledged": true,
  "index": "movies"
}
```
