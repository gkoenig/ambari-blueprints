# set up a minimal cluster consisting of 2 nodes
The installed HDP stack will be version 2.2 (latest available at Jan 27th,2015) and will serve as a base HDFS/YARN cluster with two instances of Kafka and Hive+Pig support.
Additional tools like Falcon, Slider, Storm won't be installed due to lack of resources...

## Prerequisites:
* two linux boxes with CentOS/RH (I tried 6.6)
* at least 4GB Ram each
* configured repositories to access Ambari,HDP and HDP-utils
* Ambari server is running (examples are using port 8080) on host *ambari-1.geko* (adjust it according to your ambari-server hostname)
* ensure that the ambari-client is running on both nodes, and that they are connected to the ambari-server, e.g. check the registered hosts via http://```<your-ambari-server>```/api/v1/hosts

## Usage:

Ambari Hostname(FQDN): *ambari-1.geko* , change it to match your hostname.
Ambari credentials: *admin/admin* , the default settings, you should change it !

You need to replace the hostnames of the two cluster nodes in file *minicluster-hostmapping.json* according to your setup !

```
git clone https://github.com/gkoenig/ambari-blueprints
cd ambari-blueprints/minicluster
<adjust/set the hostnames of your cluster in the file minicluster-hostmapping.json>
curl --user admin:admin -H 'X-Requested-By:geko' -X POST http://ambari-1.geko:8080/api/v1/blueprints/minicluster.json
curl --user admin:admin -H 'X-Requested-By:geko' -X POST http://ambari-1.geko:8080/api/v1/clusters/cl-minicluster -d @minicluster-hostmapping.json
```

The first *curl* call sets the blueprint, the second call creates the cluster and starts the installation

## The blueprint will setup a cluster with the following services, distributed over the two nodes
* NODE-1
    * ZOOKEEPER_SERVER
    * KAFKA_BROKER
    * GANGLIA_SERVER
    * NAMENODE
    * NODEMANAGER
    * HDFS_CLIENT
    * YARN_CLIENT
    * HCAT
    * NAGIOS_SERVER
    * MAPREDUCE2_CLIENT
    * TEZ_CLIENT
    * DATANODE
    * GANGLIA_MONITOR
* NODE-2
    * ZOOKEEPER_SERVER
    * ZOOKEEPER_CLIENT
    * PIG
    * HISTORYSERVER
    * KAFKA_BROKER
    * HIVE_SERVER
    * HCAT
    * SECONDARY_NAMENODE
    * TEZ_CLIENT
    * HIVE_METASTORE
    * APP_TIMELINE_SERVER
    * NODEMANAGER
    * HDFS_CLIENT
    * HIVE_CLIENT
    * YARN_CLIENT
    * MAPREDUCE2_CLIENT
    * DATANODE
    * MYSQL_SERVER
    * GANGLIA_MONITOR
    * WEBHCAT_SERVER
    * RESOURCEMANAGER
        
## and adjust some properties to match this veeery restricted cluster
* set the db password to "p"
* sets the nagios contact mail accordingly
* configures some Hive properties. They will be written to hive-site.xml
* configures the HDFS replication factor to *2*, since we only have 2 datanodes
* configures several base parameters to match the restricted resources of the minimal cluster, e.g. lower the Java memory consumption
* create a HDP cluster called *cl-minicluster*

After initial installation has finished, I had to restart service *Hive* manually to bring it online.

## Ambari dashboard
anything GREEN :D

![ambaridashboard](https://cloud.githubusercontent.com/assets/50473/5926262/16225d3c-a66a-11e4-8fd3-ab20c55aa393.png)


