


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

# Install Kibana using NSSM
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

#### Query DSL and Mapping
Query DSL is JSON style domain-specific language that is used to execute queries in Elastic Search.

Structure of query:
```javascript
QUERY_NAME: { // MANDATORY
	FIELD_NAME: { // OPTIONAL
		ARGUMENT: VALUE,
		ARGUMENT: VALUE, ...
	}
}
```

#### Query (scoring) vs Filters (non scoring)
| Query | Filters |
|--|--|
| Better for Full text search <br/> e.g. Title of a blog, summary, etc. where free text will be entered by user | Exact match on values <br/> e.g. language code, category, etc. where we have exact values with no special meaning |
| How well does it match? Relevance | Does it match Yes or No. <br/> When  relevance does not matter |
| For analyzed/tokenized data  | For non-analyzed data |
| Not cacheable | Cacheable |
| Slow compared to Filter | Faster |

#### Types of Queries

 - Full text Query
 - Term level Query
 - Compound Query
 - Joining Query
 - Geo Query
 - Specialized Query
 - Span Query

### Inverted Index
https://www.elastic.co/guide/en/elasticsearch/guide/current/inverted-index.html

https://www.elastic.co/blog/found-elasticsearch-from-the-bottom-up

### Analyzers
We can specifiy different types of analyzers for fields when building the index.

##### Whitespace analyzer
The "whitespace" analyzer will split the text on whitespace (space, tabs, newlines).
```javascript
POST _analyze
{
  "analyzer": "whitespace",
  "text": "Elastic search\nis the heart of ELK stack and\t it uses inverted index."
}
```

Response is as follows when using "whitespace" analyzer.

```javascript
{
  "tokens": [
    { "token": "Elastic",  "start_offset": 0,  "end_offset": 7,  "type": "word", "position": 0 },
    { "token": "search",   "start_offset": 8,  "end_offset": 14, "type": "word", "position": 1 },
    { "token": "is",       "start_offset": 15, "end_offset": 17, "type": "word", "position": 2 },
    { "token": "the",      "start_offset": 18, "end_offset": 21, "type": "word", "position": 3 },
    { "token": "heart",    "start_offset": 22, "end_offset": 27, "type": "word", "position": 4 },
    { "token": "of",       "start_offset": 28, "end_offset": 30, "type": "word", "position": 5 },
    { "token": "ELK",      "start_offset": 31, "end_offset": 34, "type": "word", "position": 6 },
    { "token": "stack",    "start_offset": 35, "end_offset": 40, "type": "word", "position": 7 },
    { "token": "and",      "start_offset": 41, "end_offset": 44, "type": "word", "position": 8 },
    { "token": "it",       "start_offset": 46, "end_offset": 48, "type": "word", "position": 9 },
    { "token": "uses",     "start_offset": 49, "end_offset": 53, "type": "word", "position": 10 },
    { "token": "inverted", "start_offset": 54, "end_offset": 62, "type": "word", "position": 11 },
    {"token": "index.",    "start_offset": 63, "end_offset": 69, "type": "word", "position": 12 }
  ]
}
```

##### Stop analyzer
The "stop" analyzer will removes the stop words (like is, the, of, it, and) from the text.
```javascript
POST _analyze
{
  "analyzer": "stop",
  "text": "Elastic search\nis the heart of ELK stack and\t it uses inverted index."
}
```
Response is as follows when using "stop" analyzer.
```javascript
{
  "tokens": [
    { "token": "elastic", "start_offset": 0, "end_offset": 7, "type": "word", "position": 0 },
    {"token": "search", "start_offset": 8, "end_offset": 14, "type": "word", "position": 1 },
    { "token": "heart", "start_offset": 22, "end_offset": 27, "type": "word", "position": 4 },
    { "token": "elk", "start_offset": 31, "end_offset": 34, "type": "word", "position": 6 },
    { "token": "stack", "start_offset": 35, "end_offset": 40, "type": "word", "position": 7 },
    { "token": "uses", "start_offset": 49, "end_offset": 53, "type": "word", "position": 10 },
    { "token": "inverted", "start_offset": 54, "end_offset": 62, "type": "word", "position": 11 },
    { "token": "index", "start_offset": 63, "end_offset": 68, "type": "word", "position": 12 }
  ]
}
```

### Scripting
Elastic search supports scripting in different languages.

- **General purpose**
	- Painless
	- Groovy
	- Javascript
	- Python

- **Special purpose**
	- Lucene Expressions
	- Mustache
	- Java

#### Example of Scripting
Example of documents in Bank index are shown below.

```javascript
{ "_index": "bank", "_type": "account", "_id": "25", "_score": 1, 
"_source": {"account_number": 25, "balance": 40540, "firstname": "Virginia", "lastname": "Ayala", "age": 39, "gender": "F", "address": "171 Putnam Avenue", "employer": "Filodyne", "email": "virginiaayala@filodyne.com", "city": "Nicholson", "state": "PA" }
}

{ "_index": "bank", "_type": "account", "_id": "44", "_score": 1, 
"_source": {"account_number": 44, "balance": 34487, "firstname": "Aurelia", "lastname": "Harding", "age": 37, "gender": "M", "address": "502 Baycliff Terrace", "employer": "Orbalix", "email": "aureliaharding@orbalix.com", "city": "Yardville", "state": "DE" }
}

{ "_index": "bank", "_type": "account", "_id": "99", "_score": 1, 
"_source": { "account_number": 99, "balance": 47159, "firstname": "Ratliff", "lastname": "Heath", "age": 39, "gender": "F", "address": "806 Rockwell Place", "employer": "Zappix", "email": "ratliffheath@zappix.com", "city": "Shaft", "state": "ND" }
}
```

In the following example, we subtract 2 from "age" present in every document in the "bank" index.
We use inline scriping in this case.
```javascript
GET bank/_search
{
  "script_fields": {
    "age": {
      "script": {
        "lang": "expression",
        "inline": "doc['age'] - subtractor",
        "params": {"subtractor": 2}
      }
    }
  }
}
```

Response:

```javascript
#! Deprecation: Deprecated field [inline] used, expected [source] instead
{
  "took": 1,
  "timed_out": false,
  "_shards": { "total": 5, "successful": 5, "skipped": 0, "failed": 0
  },
  "hits": { "total": 1000, "max_score": 1, "hits": [
      { "_index": "bank", "_type": "account", "_id": "25", "_score": 1, "fields": { "age": [37] } },
      { "_index": "bank", "_type": "account", "_id": "44", "_score": 1, "fields": { "age": [35] } },
      { "_index": "bank", "_type": "account", "_id": "99", "_score": 1, "fields": { "age": [37] } },
      { "_index": "bank", "_type": "account", "_id": "119", "_score": 1, "fields": { "age": [26] } },
      { "_index": "bank", "_type": "account", "_id": "126", "_score": 1, "fields": { "age": [37] } },
      { "_index": "bank", "_type": "account", "_id": "145", "_score": 1, "fields": { "age": [30] } },
      { "_index": "bank", "_type": "account", "_id": "183", "_score": 1, "fields": { "age": [24] } },
      { "_index": "bank", "_type": "account", "_id": "190", "_score": 1, "fields": { "age": [28] } },
      { "_index": "bank", "_type": "account", "_id": "208", "_score": 1, "fields": { "age": [24] } },
      { "_index": "bank", "_type": "account", "_id": "222", "_score": 1, "fields": { "age": [34] } }
    ]
  }
}
```

# Logstash
Logstash can be used to collect, parse and transform data.

## Stashing with Logstash
Logstash pipeline has three stages.
Source -> INPUT -> FILTER -> OUTPUT -> Destination

### Input Stage/Plugin
	Input stage generates the event.
	It enables a specific source of events to be read by Logstash. 

Few example of input plugins are:
 - Twitter plugin
 - RabbitMQ plugin
 - Kafka plugin
 - Amazon SQS plugin

### Filter Stage/Plugin
	Filter is an intermediary process on an event.
	Filter will modify the event.
	This is optional

Few examples of filter plugins are:
- drop
- split
- json
- xml
- mutate

### Output Stage/Plugin
	- Output plugin is used in the final stage of Logstash pipeline. 
	- It is used to send the event data to a specific destination.

For each destination, normally there will be a output plugin that is to be used.
Few examples of output plugin are:
 - Elastic Search
 - JIRA
 - MongoDB

### Codec Plugin
	This plugin is used to change the data representation of an event and 
	can be applied either in Input or Output stage.

Few examples of Codec plugin are:
 - avro
 - cloudfront
 - json
 - multiline

## Loading CSV file in ElasticSearch using Logstash
In following example, we will use the [summer.csv](https://raw.githubusercontent.com/yogeshrnaik/ELK-stack/master/input/summer.csv) which contains details regarding Olympic medal winners.

Go to the bin folder under Logstash installation. <LOGSTASH_INSTALLATION>/bin
e.g. C:\DDrive\MyData\SWs\Elastic\logstash-6.2.4\bin

Create a new file "[LogstashPipelineCSV.conf](https://raw.githubusercontent.com/yogeshrnaik/ELK-stack/master/logstash/LogstashPipelineCSV.conf)" to provide mapping for [summer.csv](https://raw.githubusercontent.com/yogeshrnaik/ELK-stack/master/input/summer.csv) file.

Validate it using below command.
```
C:\DDrive\MyData\SWs\Elastic\logstash-6.2.4\bin>logstash -f "C:\DDrive\MyData\Yogesh\git_repo\ELK-stack\logstash\LogstashPipelineCSV.conf" --config.test_and_exit
Sending Logstash's logs to C:/DDrive/MyData/SWs/Elastic/logstash-6.2.4/logs which is now configured via log4j2.properties
[2018-07-13T15:33:36,745][INFO ][logstash.modules.scaffold] Initializing module {:module_name=>"fb_apache", :directory=>"C:/DDrive/MyData/SWs/Elastic/logstash-6.2.4/modules/fb_apache/configuration"}
...
[2018-07-13T15:33:36,795][INFO ][logstash.modules.scaffold] Initializing module {:module_name=>"netflow", :directory=>"C:/DDrive/MyData/SWs/Elastic/logstash-6.2.4/modules/netflow/configuration"}
[2018-07-13T15:33:37,071][WARN ][logstash.config.source.multilocal] Ignoring the 'pipelines.yml' file because modules or command line options are specified
Configuration OK
[2018-07-13T15:33:40,905][INFO ][logstash.runner          ] Using config.test_and_exit mode. Config Validation Result: OK. Exiting Logstash
```
To create the Logstash pipeline, run the following command.

```
C:\DDrive\MyData\SWs\Elastic\logstash-6.2.4\bin>logstash -f "C:\DDrive\MyData\Yogesh\git_repo\ELK-stack\logstash\LogstashPipelineCSV.conf"

Sending Logstash's logs to C:/DDrive/MyData/SWs/Elastic/logstash-6.2.4/logs which is now configured via log4j2.properties
[2018-07-16T11:44:47,517][INFO ][logstash.modules.scaffold] Initializing module {:module_name=>"fb_apache", :directory=>"C:/DDrive/MyData/SWs/Elastic/logstash-6.2.4/modules/fb_apache/configuration"}
[2018-07-16T11:44:47,530][INFO ][logstash.modules.scaffold] Initializing module {:module_name=>"netflow", :directory=>"C:/DDrive/MyData/SWs/Elastic/logstash-6.2.4/modules/netflow/configuration"}
[2018-07-16T11:44:47,680][WARN ][logstash.config.source.multilocal] Ignoring the 'pipelines.yml' file because modules or command line options are specified
[2018-07-16T11:44:48,085][INFO ][logstash.runner          ] Starting Logstash {"logstash.version"=>"6.2.4"}
[2018-07-16T11:44:48,456][INFO ][logstash.agent           ] Successfully started Logstash API endpoint {:port=>9600}
[2018-07-16T11:44:50,730][INFO ][logstash.pipeline        ] Starting pipeline {:pipeline_id=>"main", "pipeline.workers"=>8, "pipeline.batch.size"=>125, "pipeline.batch.delay"=>50}
[2018-07-16T11:44:51,075][INFO ][logstash.outputs.elasticsearch] Elasticsearch pool URLs updated {:changes=>{:removed=>[], :added=>[http://localhost:9200/]}}
[2018-07-16T11:44:51,118][INFO ][logstash.outputs.elasticsearch] Running health check to see if an Elasticsearch connection is working {:healthcheck_url=>http://localhost:9200/, :path=>"/"}
[2018-07-16T11:44:51,271][WARN ][logstash.outputs.elasticsearch] Restored connection to ES instance {:url=>"http://localhost:9200/"}
[2018-07-16T11:44:51,317][INFO ][logstash.outputs.elasticsearch] ES Output version determined {:es_version=>6}
[2018-07-16T11:44:51,320][WARN ][logstash.outputs.elasticsearch] Detected a 6.x and above cluster: the `type` event field won't be used to determine the document _type {:es_version=>6}
[2018-07-16T11:44:51,333][INFO ][logstash.outputs.elasticsearch] Using mapping template from {:path=>nil}
[2018-07-16T11:44:51,348][INFO ][logstash.outputs.elasticsearch] Attempting to install template {:manage_template=>{"template"=>"logstash-*", "version"=>60001, "settings"=>{"index.refresh_interval"=>"5s"}, "mappings"=>{"_default_"=>{"dynamic_templates"=>[{"message_field"=>{"path_match"=>"message", "match_mapping_type"=>"string", "mapping"=>{"type"=>"text", "norms"=>false}}}, {"string_fields"=>{"match"=>"*", "match_mapping_type"=>"string", "mapping"=>{"type"=>"text", "norms"=>false, "fields"=>{"keyword"=>{"type"=>"keyword", "ignore_above"=>256}}}}}], "properties"=>{"@timestamp"=>{"type"=>"date"}, "@version"=>{"type"=>"keyword"}, "geoip"=>{"dynamic"=>true, "properties"=>{"ip"=>{"type"=>"ip"}, "location"=>{"type"=>"geo_point"}, "latitude"=>{"type"=>"half_float"}, "longitude"=>{"type"=>"half_float"}}}}}}}}
[2018-07-16T11:44:51,385][INFO ][logstash.outputs.elasticsearch] New Elasticsearch output {:class=>"LogStash::Outputs::ElasticSearch", :hosts=>["//localhost"]}
[2018-07-16T11:44:52,031][INFO ][logstash.pipeline        ] Pipeline started successfully {:pipeline_id=>"main", :thread=>"#<Thread:0x19d325f0 sleep>"}
[2018-07-16T11:44:52,096][INFO ][logstash.agent           ] Pipelines running {:count=>1, :pipelines=>["main"]}
```

Check the status of index "olympics-2018.07.16" by going to URL: http://localhost:9200/_cat/indices?v

```
health status index               uuid                   pri rep docs.count docs.deleted store.size pri.store.size
yellow open   bank                oXmgK5iVQ_uRDzTJ-WzGXQ   5   1       1000            0    475.1kb        475.1kb
yellow open   movies              mGwrDjrUREG3UqcTnZnGYw   5   2          0            0      1.2kb          1.2kb
green  open   .kibana             j1lQlXTrRO6PPFnZvq3WVw   1   0          2            0      6.6kb          6.6kb
yellow open   olympics-2018.07.16 5SPZHp0iRsKriNIl3S_5YA   5   1      31166            0     11.8mb         11.8mb
```
The count of documents in summer.csv is 31166. 
Once "docs.count" in index matches this count, you can terminate the logstash process.

#### Deleting index
If you run the above command again, it will load same documents again. So, there will be twice the number of documents. 

You can delete the index before running the command again.

Open Kibana (http://localhost:5601/) and go to "Dev Tools". 
Type and run the following command to delete the index "olympics-2018.07.16".
```
DELETE olympics-2018.07.16
```
Response will be:
```javascript
{
  "acknowledged": true
}
```

#### Check index in Kibana
Launch Kibana (http://localhost:5601) and go to "Management > Index Patterns".
Click on "Create Index Pattern" to create an index pattern for the Olympics data loaded in previous steps.

Type "olympics*" in the "Index pattern" text box and click on "Next Step".

![enter image description here](https://raw.githubusercontent.com/yogeshrnaik/ELK-stack/master/images/create-index-pattern-1.jpg)

![enter image description here](https://raw.githubusercontent.com/yogeshrnaik/ELK-stack/master/images/create-index-pattern-2.jpg)

Click on "Create Index pattern" to create the index pattern.

You can go to Discover tab in Kibana and select the index pattern "olympics*" to see data that was loaded.
![enter image description here](https://raw.githubusercontent.com/yogeshrnaik/ELK-stack/master/images/olympics-index.jpg)

## APIs for Logstash
There are different types of monitoring APIs provided by Logstash.  Using these APIs, we can monitor the Logstash pipelines. 

The APIs for Logstash is categorized into four groups.

 - Node info API
 - Plugins info API
 - Node stats API 
 - Hot threads API

We will use a browser to test these APIs as all these API call are using GET method.
If Logstash is not running, start it from "Services".

### Node info API
Open "http://localhost:9600/?pretty=true" in browser and you can see response as below.
```javascript
{
	host: "IN1WXL-301034",
	version: "6.2.4",
	http_address: "127.0.0.1:9600",
	id: "19dddf2f-31b7-4e46-88d5-5815bddf38db",
	name: "IN1WXL-301034",
	build_date: "2018-04-12T22:29:17Z",
	build_sha: "a425a422e03087ac34ad6949f7c95ec6d27faf14",
	build_snapshot: false
}
```
Go to "http://localhost:9600/_node" to see details about the node on which Logstash is running.

```javascript
{
	host: "IN1WXL-301034",
	version: "6.2.4",
	http_address: "127.0.0.1:9600",
	id: "19dddf2f-31b7-4e46-88d5-5815bddf38db",
	name: "IN1WXL-301034",
	pipelines: {
		main: {
			workers: 8,
			batch_size: 125,
			batch_delay: 50,
			config_reload_automatic: false,
			config_reload_interval: 3000000000,
			dead_letter_queue_enabled: false
		}
	},
	os: {
		name: "Windows 10",
		arch: "amd64",
		version: "10.0",
		available_processors: 8
	},
	jvm: {
		pid: 6184,
		version: "1.8.0_151",
		vm_version: "1.8.0_151",
		vm_vendor: "Oracle Corporation",
		vm_name: "Java HotSpot(TM) 64-Bit Server VM",
		start_time_in_millis: 1531729937257,
		mem: {
			heap_init_in_bytes: 1073741824,
			heap_max_in_bytes: 1037959168,
			non_heap_init_in_bytes: 2555904,
			non_heap_max_in_bytes: 0
		},
		gc_collectors: ["ParNew",
		"ConcurrentMarkSweep"]
	}
}
```
### Plugin info API
This provides information about all Logstash plugins that are currently installed.
Go to "http://localhost:9600/_node/plugins" see this list.
```javascript
{
	host: "IN1WXL-301034",
	version: "6.2.4",
	http_address: "127.0.0.1:9600",
	id: "19dddf2f-31b7-4e46-88d5-5815bddf38db",
	name: "IN1WXL-301034",
	total: 96,
	plugins: [
	{ name: "logstash-codec-cef", version: "5.0.2" },
	{ name: "logstash-codec-json", version: "3.0.5" },
	{ name: "logstash-codec-multiline", version: "3.0.9" },
	{ name: "logstash-filter-mutate", version: "3.3.1" },
	{ name: "logstash-filter-split", version: "3.1.6" },
	{ name: "logstash-filter-xml", version: "4.0.5" },
	{ name: "logstash-input-file", version: "4.0.5" },
	{ name: "logstash-input-kafka", version: "8.0.6" },
	{ name: "logstash-output-kafka", version: "7.0.10" },
	{ name: "logstash-output-s3", version: "4.1.1" },
	{ name: "logstash-output-sns", version: "4.0.7" },
	{ name: "logstash-output-sqs", version: "5.1.1" }
	// ...
    ]
}
```

### Node stat API
Open "http://localhost:9600/_node/stats" to see the Node stats.
![enter image description here](https://raw.githubusercontent.com/yogeshrnaik/ELK-stack/master/images/node_stats.jpg)

### Hot threads API
Hot thread is a thread that has high CPU Usage and executes for a longer period of time.

Using Hot thread API (http://localhost:9600/_node/hot_threads) we can retrieve the current hot threads running in Logstash.
```javascript
{
	host: "IN1WXL-301034",
	version: "6.2.4",
	http_address: "127.0.0.1:9600",
	id: "19dddf2f-31b7-4e46-88d5-5815bddf38db",
	name: "IN1WXL-301034",
	hot_threads: {
		time: "2018-07-16T14:40:53+05:30",
		busiest_threads: 3,
		threads: [{
			name: "LogStash::Runner",
			thread_id: 1,
			percent_of_cpu_time: 1.14,
			state: "timed_waiting",
			traces: ["java.lang.Object.wait(Native Method)",
			"java.lang.Thread.join(Thread.java:1260)",
			"org.jruby.internal.runtime.NativeThread.join(NativeThread.java:76)"]
		},
		{
			name: "[main]<file",
			thread_id: 35,
			percent_of_cpu_time: 0.31,
			state: "timed_waiting",
			traces: ["sun.misc.Unsafe.park(Native Method)",
			"java.util.concurrent.locks.LockSupport.parkNanos(LockSupport.java:215)",
			"java.util.concurrent.locks.AbstractQueuedSynchronizer.doAcquireSharedNanos(AbstractQueuedSynchronizer.java:1037)"]
		},
		{
			name: "Ruby-0-Thread-1",
			thread_id: 18,
			percent_of_cpu_time: 0.1,
			state: "timed_waiting",
			path: "C:\DDrive\MyData\SWs\Elastic\logstash-6.2.4\lib\bootstrap\environment.rb:6",
			traces: ["sun.misc.Unsafe.park(Native Method)",
			"java.util.concurrent.locks.LockSupport.parkNanos(LockSupport.java:215)",
			"java.util.concurrent.locks.AbstractQueuedSynchronizer.doAcquireSharedNanos(AbstractQueuedSynchronizer.java:1037)"]
		}]
	}
}
```
To make above response Human readable, use http://localhost:9600/_node/hot_threads?human=true
```
::: {}
Hot threads at 2018-07-16T14:43:47+05:30, busiestThreads=3: 
================================================================================
1.06 % of cpu usage, state: timed_waiting, thread name: 'LogStash::Runner', thread id: 1 
	java.lang.Object.wait(Native Method)
	java.lang.Thread.join(Thread.java:1260)
	org.jruby.internal.runtime.NativeThread.join(NativeThread.java:76)
--------------------------------------------------------------------------------
0.3 % of cpu usage, state: timed_waiting, thread name: '[main]<file', thread id: 35 
	sun.misc.Unsafe.park(Native Method)
	java.util.concurrent.locks.LockSupport.parkNanos(LockSupport.java:215)
	java.util.concurrent.locks.AbstractQueuedSynchronizer.doAcquireSharedNanos(AbstractQueuedSynchronizer.java:1037)
--------------------------------------------------------------------------------
0.09 % of cpu usage, state: timed_waiting, thread name: 'Ruby-0-Thread-1', thread id: 18 
C:\DDrive\MyData\SWs\Elastic\logstash-6.2.4\lib\bootstrap\environment.rb:6
	sun.misc.Unsafe.park(Native Method)
	java.util.concurrent.locks.LockSupport.parkNanos(LockSupport.java:215)
	java.util.concurrent.locks.AbstractQueuedSynchronizer.doAcquireSharedNanos(AbstractQueuedSynchronizer.java:1037)
--------------------------------------------------------------------------------
```

# Kibana
Kibana is an user interface to slice and dice the data loaded in Elastic Search.

## Discover tab
We will see how we can use the Discover tab in Kibana to search for records from Olympics data we have loaded earlier.

Open Kibana and go to Discover tab. Select the Index pattern "Olympics*" and it will show all the data.

![enter image description here](https://raw.githubusercontent.com/yogeshrnaik/ELK-stack/master/images/olympics-index.jpg)

To find out all Athletes whose name contains "jack", type "Athlete:Jack*" in the text box provided. This will show the results as below.
![enter image description here](https://raw.githubusercontent.com/yogeshrnaik/ELK-stack/master/images/kibana-discover-tab-1.jpg)

At the top, it shows the number of hits.
The results are shown on the center side and we can see all names containing "Jack" are highlighted by Kibana.

On the left side, we can see different fields from the index. We can add them to the result using the "add" button.

We can have multiple criteria such as:
```
Athlete:Jack* AND Year:>2000 AND Gender:WoMen
```
The result is shown below.
![enter image description here](https://raw.githubusercontent.com/yogeshrnaik/ELK-stack/master/images/kibana-discover-tab-2.jpg)

The fields that are added in the result table are shown on the left side.
And we can remove a field from result table using the "remove" button.

We can save the search criteria and the result using "Save" button on top right corner.
The saved search can be opened later using "Open" button on top right corner. 
