# Install Elastic Search on Computer

**Pre-requisite:** Latest JDK

Download Zip from https://www.elastic.co/downloads/elasticsearch and extract it somewhere on computer

Go to folder where you have extracted Elastic Search and go to config folder of the installation:
C:\DDrive\MyData\SWs\Elastic\elasticsearch-6.2.4\config

Edit elasticsearch.yml
cluster.name: Packt
node.name: Packtnode

Open command prompt in: 
C:\DDrive\MyData\SWs\Elastic\elasticsearch-6.2.4\bin

And run batch file: elasticsearch-service.bat

    C:\DDrive\MyData\SWs\Elastic\elasticsearch-6.2.4\bin>elasticsearch-service.bat install
    Installing service      :  "elasticsearch-service-x64"
    Using JAVA_HOME (64-bit):  "C:\Program Files\Java\jdk-9.0.4" -Xms1g;-Xmx1g;-XX:+UseConcMarkSweepGC;-XX:CMSInitiatingOccupancyFraction=75;-XX:+UseCMSInitiatingOccupancyOnly;-XX:+AlwaysPreTouch;-Xss1m;-Djava.awt.headless=true;-Dfile.encoding=UTF-8;-Djna.nosys=true;-XX:-OmitStackTraceInFastThrow;-Dio.netty.noUnsafe=true;-Dio.netty.noKeySetOptimization=true;-Dio.netty.recycler.maxCapacityPerThread=0;-Dlog4j.shutdownHookEnabled=false;-Dlog4j2.disable.jmx=true;-Djava.io.tmpdir=C:\Users\naiky\AppData\Local\Temp\elasticsearch;-XX:+HeapDumpOnOutOfMemoryError;-Xlog:gc*,gc+age=trace,safepoint:file=logs/gc.log:utctime,pid,tags:filecount=32,filesize=64m;-Djava.locale.providers=COMPAT
    The service 'elasticsearch-service-x64' has been installed.

You should see elasticsearch-service-x64 under "Services".

Start the service using command:

    C:\DDrive\MyData\SWs\Elastic\elasticsearch-6.2.4\bin>elasticsearch-service.bat start
    The service 'elasticsearch-service-x64' has been started

Open http://localhost:9200/ in browser to confirm if installation is ok.
Response:

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

Then test this conf file using command:

    C:\DDrive\MyData\SWs\Elastic\logstash-6.2.4\bin>logstash -f LogstashPipeline.conf --config.test_and_exit
    Sending Logstash's logs to C:/DDrive/MyData/SWs/Elastic/logstash-6.2.4/logs which is now configured via log4j2.properties
    [2018-07-12T12:33:42,441][INFO ][logstash.modules.scaffold] Initializing module {:module_name=>"fb_apache", :directory=>"C:/DDrive/MyData/SWs/Elastic/logstash-6.2.4/modules/fb_apache/configuration"}
    [2018-07-12T12:33:42,464][INFO ][logstash.modules.scaffold] Initializing module {:module_name=>"netflow", :directory=>"C:/DDrive/MyData/SWs/Elastic/logstash-6.2.4/modules/netflow/configuration"}
    [2018-07-12T12:33:42,558][INFO ][logstash.setting.writabledirectory] Creating directory {:setting=>"path.queue", :path=>"C:/DDrive/MyData/SWs/Elastic/logstash-6.2.4/data/queue"}
    [2018-07-12T12:33:42,564][INFO ][logstash.setting.writabledirectory] Creating directory {:setting=>"path.dead_letter_queue", :path=>"C:/DDrive/MyData/SWs/Elastic/logstash-6.2.4/data/dead_letter_queue"}
    [2018-07-12T12:33:42,694][WARN ][logstash.config.source.multilocal] Ignoring the 'pipelines.yml' file because modules or command line options are specified Configuration OK
    [2018-07-12T12:33:44,711][INFO ][logstash.runner          ] Using config.test_and_exit mode. Config Validation Result: OK. Exiting Logstash

If you get any error make sure you have correct version of JDK and JRE on classpath, compatible with Logstash version you are running.


