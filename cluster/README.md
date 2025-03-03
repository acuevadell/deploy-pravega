# Deploying Pravega in a Linux System
Following the steps of https://pravega.io/docs/nightly/deployment/manual-install/

Download required versions:
```
wget https://archive.apache.org/dist/zookeeper/zookeeper-3.6.1/apache-zookeeper-3.6.1-bin.tar.gz
wget https://archive.apache.org/dist/bookkeeper/bookkeeper-4.11.1/bookkeeper-server-4.11.1-bin.tar.gz
wget https://github.com/pravega/pravega/releases/download/v0.13.0/pravega-0.13.0.tgz
```

## File Storage

Pravega requires a HDFS storage cluster or a NFS share, but for this local experiment we will use a local folder as Tier 2 Storage.
```
sudo mkdir /var/tier2
sudo chmod 777 /var/tier2
```

## Zookeeper

```
tar xfvz apache-zookeeper-3.6.1-bin.tar.gz
mv apache-zookeeper-3.6.1-bin zookeeper
cp zoo.cfg ./zookeeper/conf/zoo.cfg
./zookeeper/bin/zkServer.sh start
./zookeeper/bin/zkCli.sh -server localhost:2181 create /pravega
./zookeeper/bin/zkCli.sh -server localhost:2181 create /pravega/bookkeeper
```

## Bookkeeper 

```
tar xfvz bookkeeper-server-4.11.1-bin.tar.gz
mv bookkeeper-server-4.11.1 bookkeeper
cp bk_server.conf bookkeeper/conf/bk_server.conf
sudo mkdir /bk/
sudo chmod 777 /bk/
./bookkeeper/bin/bookkeeper shell metaformat -nonInteractive
./bookkeeper/bin/bookkeeper bookie
```

## Pravega

```
tar xfvz pravega-0.13.0.tgz
mv pravega-0.13.0 pravega
cp controller.config.properties pravega/conf/controller.config.properties
./pravega/bin/pravega-controller
```

# Segment Store

```
cp config.properties pravega/conf/config.properties
pravega/bin/pravega-segmentstore
```

