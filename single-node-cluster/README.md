# set up a single node cluster

## Prerequisites:
* a linux box with CentOS/RH (I tried 6.6)
* at least 4GB Ram
* configured repositories to access HDP and HDP-utils
* Ambari server is running (examples are using port 8080)

## The blueprint will
* install the services
  * NAMENODE
  * SECONDARY_NAMENODE
  * DATANODE
  * HDFS_CLIENT
  * RESOURCEMANAGER
  * NODEMANAGER
  * YARN_CLIENT
  * HISTORYSERVER
  * MAPREDUCE2_CLIENT
  * ZOOKEEPER_SERVER
  * ZOOKEEPER_CLIENT
  * GANGLIA_SERVER
  * HIVE_SERVER
  * HIVE_CLIENT
  * TEZ_CLIENT
  * WEBHCAT_SERVER
  * MYSQL_SERVER
  * NAGIOS_SERVER
* set the db password to "pw"
* sets the nagios contact mail accordingly
* configures some Hive properties. They will be written to hive-site.xml
* create a HDP cluster called *single-node*

## Usage:

Hostname(FQDN): *ambari-1.geko* , change it to match your hostname
Ambari credentials: *admin/admin* , the default settings, you should change it !
```
git clone https://github.com/gkoenig/ambari-blueprints
cd ambari-blueprints/single-node-cluster
curl --user admin:admin -H 'X-Requested-By:geko' -X POST http://ambari-1.geko:8080/api/v1/blueprints/single-node -d @single-node-hdfs-yarn.json
curl --user admin:admin -H 'X-Requested-By:geko' -X POST http://ambari-1.geko:8080/api/v1/clusters/single-node -d @single-node-hostmapping.json
```

The first *curl* call sets the blueprint, the second call creates the cluster and starts the installation

_ISSUE_ : I experienced problems at service *Hive-server* and *WebHCat* , the didn't start up after initial
installation. After the installation has finished, I was able to start those services manually ... ?!?!

## Ambari dashboard
After restarting the Hive-server/WebHCat service manually, et voila =>
![ambaridashboard](https://cloud.githubusercontent.com/assets/50473/5542652/6145a324-8ae9-11e4-87ea-d9492e29d7f5.png)


