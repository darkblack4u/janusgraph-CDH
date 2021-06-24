> # JanusGraph中CDH相关Lib包替换
1. 从Spark目录传入spark-assembly-1.6.0-cdh5.7.6-hadoop2.6.0-cdh5.7.6.jar。
```
rm –f /opt/janusgraph-0.2.3-hadoop2/lib/spark-assembly-*.jar
cp /opt/cloudera/parcels/CDH/jars/spark-assembly-1.6.0-cdh5.7.6-hadoop2.6.0-cdh5.7.6.jar /opt/janusgraph-0.2.3-hadoop2/lib
```
2. 从Hadoop目录传入htrace-core4-4.0.1-incubating.jar、hadoop-common-2.6.0-cdh5.7.6.jar、hadoop-hdfs-2.6.0-cdh5.7.6.jar。
```
rm –f /opt/janusgraph-0.2.3-hadoop2/lib/htrace-core4-*.jar
cp /opt/cloudera/parcels/CDH/jars/htrace-core4-4.0.1-incubating.jar /opt/janusgraph-0.2.3-hadoop2/lib
rm –f /opt/janusgraph-0.2.3-hadoop2/lib/hadoop-common-*.jar
cp /opt/cloudera/parcels/CDH/jars/hadoop-common-2.6.0-cdh5.7.6.jar /opt/janusgraph-0.2.3-hadoop2/lib
rm –f /opt/janusgraph-0.2.3-hadoop2/lib/hadoop-hdfs-*.jar
cp /opt/cloudera/parcels/CDH/jars/hadoop-hdfs-2.6.0-cdh5.7.6.jar /opt/janusgraph-0.2.3-hadoop2/lib
```

3. 从Yarn目录cp入hadoop-yarn-api-2.6.0-cdh5.7.6.jar、hadoop-yarn-common-2.6.0-cdh5.7.6.jar、hadoop-yarn-server-web-proxy-2.6.0-cdh5.7.6.jar替换掉原有。
```
rm –f /opt/janusgraph-0.2.3-hadoop2/lib/hadoop-yarn-api-*.jar
cp /opt/cloudera/parcels/CDH/jars/hadoop-yarn-api-2.6.0-cdh5.7.6.jar /opt/janusgraph-0.2.3-hadoop2/lib
rm –f /opt/janusgraph-0.2.3-hadoop2/lib/hadoop-yarn-common-*.jar
cp /opt/cloudera/parcels/CDH/jars/hadoop-yarn-common-2.6.0-cdh5.7.6.jar /opt/janusgraph-0.2.3-hadoop2/lib
rm –f /opt/janusgraph-0.2.3-hadoop2/lib/hadoop-yarn-server-web-proxy-*.jar
cp /opt/cloudera/parcels/CDH/jars/hadoop-yarn-server-web-proxy-2.6.0-cdh5.7.6.jar /opt/janusgraph-0.2.3-hadoop2/lib
```

4. JanusGraph中新shade相关Jar包替换

打包好替换janusgraph-core-0.2.3.jar，janusgraph-es-0.2.3.jar ，janusgraph-hadoop-0.2.3.jar，janusgraph-hbase-0.2.3.jar。
```
rm –f /opt/janusgraph-0.2.3-hadoop2/lib/janusgraph-core-0.2.3.jar
rm –f /opt/janusgraph-0.2.3-hadoop2/lib/janusgraph-es-0.2.3.jar
rm –f /opt/janusgraph-0.2.3-hadoop2/lib/janusgraph-hadoop-0.2.3.jar
rm –f /opt/janusgraph-0.2.3-hadoop2/lib/janusgraph-hbase-0.2.3.jar
```

