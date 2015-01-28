# set up a single node cluster
This example will install a single-node Hadoop cluster, based on Hortonworks HDP 2.1.
The Hadoop stack will be installed on the same node as Ambari is running on, please ensure that you adjust the hostname accordingly (this example uses hostname "ambari-1.geko") in the hostmapping file as well as on the commandline calls.

## Prerequisites:
* a linux box with CentOS/RH (I tried 6.6)
* at least 4GB Ram
* configured repositories to access Ambari,HDP and HDP-utils (repo files see afterwards)
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

Hostname(FQDN): *ambari-1.geko* , **change it to match your hostname**

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

***
## Repository files for RH based linux systems
  * Ambari (etc/yum.repos.d/ambari.repo)
```
[ambari-1.x]
name=Ambari 1.x
baseurl=http://public-repo-1.hortonworks.com/ambari/centos6/1.x/GA
gpgcheck=1
gpgkey=http://public-repo-1.hortonworks.com/ambari/centos6/RPM-GPG-KEY/RPM-GPG-KEY-Jenkins
enabled=1
priority=1

[Updates-ambari-1.7.0]
name=ambari-1.7.0 - Updates
baseurl=http://public-repo-1.hortonworks.com/ambari/centos6/1.x/updates/1.7.0
gpgcheck=1
gpgkey=http://public-repo-1.hortonworks.com/ambari/centos6/RPM-GPG-KEY/RPM-GPG-KEY-Jenkins
enabled=1
priority=1
```
  * HDP (etc/yum.repos.d/HDP.repo)
```
[HDP-2.1]
name=HDP
baseurl=http://public-repo-1.hortonworks.com/HDP/centos6/2.x/updates/2.1.7.0
path=/
enabled=1
gpgcheck=0
```
  * HDP-utils (etc/yum.repos.d/HDP-UTILS.repo)
```
[HDP-UTILS-1.1.0.19]
name=HDP-UTILS
baseurl=http://public-repo-1.hortonworks.com/HDP-UTILS-1.1.0.19/repos/centos6
path=/
enabled=1
gpgcheck=0

```
***
## Ambari dashboard
After restarting the Hive-server/WebHCat service manually, et voila =>
![ambaridashboard](https://cloud.githubusercontent.com/assets/50473/5542652/6145a324-8ae9-11e4-87ea-d9492e29d7f5.png)


