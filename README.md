# Ambari blueprints

This repo provides some examples to start diving
into setting up a Hadoop cluster quickly by using
[blueprints](https://cwiki.apache.org/confluence/display/AMBARI/Blueprints) for Ambari.

Currently (12/23/14) blueprints can be used to define and install
Hortonworks HDP clusters. The required steps are (high-level view):
* install your cluster nodes with OS
* install (one) Ambari server, and agents on all nodes
* define the blueprint in JSON
* define the hostmapping to apply the blueprint
* set and apply your JSON configurations by using Ambaris REST API


1.) set up a [single node cluster](https://github.com/gkoenig/ambari-blueprints/tree/master/single-node-cluster) with base HDFS/YARN components including Monitoring. HDP version 2.1

2.) set up a [two node minicluster](https://github.com/gkoenig/ambari-blueprints/tree/master/minicluster) with base HDFS/YARN components, Hive&Pig, Monitoring and Kafka brokers. HDP version 2.2

Screenshot after successfull installation of a Hortonworks HDP 2.1 (single-node) cluster

![ambaridashboard](https://cloud.githubusercontent.com/assets/50473/5542652/6145a324-8ae9-11e4-87ea-d9492e29d7f5.png)
