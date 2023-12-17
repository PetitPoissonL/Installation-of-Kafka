# Installation and configuration of Kafka for [Spark real-time project](https://github.com/PetitPoissonL/Spark_Streaming_Real_Time)

This guide provides detailed steps to install and configure Apache Kafka on a cluster of three servers: `hadoop102`, `hadoop103`, and `hadoop104`.

## Prerequisites

Ensure the following prerequisites are met on all three servers:

- Linux-based environment (CentOS 7.5)
- Java JDK installed (version 1.8.0_212)
- [Zookeeper installed](https://github.com/PetitPoissonL/Installation-and-configuration-of-ZooKeeper) 
- Cluster distribution script [xsync](https://github.com/PetitPoissonL/Cluster-distribution-script-xsync/tree/main)
- Network connectivity between all three servers
- Sufficient permissions to install and configure software

Perform the following steps on the server `hadoop102`:

## Step 1: Download Kafka

1. Download the version 2.12-3.0.0 of Kafka from the Apache website:
```
cd /opt/software/
wget https://archive.apache.org/dist/kafka/3.0.0/kafka_2.12-3.0.0.tgz
```

## Step 2: Extract the Archive

Extract the downloaded tarball:
```
tar -zxvf kafka_2.12-3.0.0.tgz -C /opt/module/
```

Rename 'kafka_2.12-3.0.0/' to 'kafka':
```
cd /opt/module/
mv kafka_2.12-3.0.0/ kafka
```

## Step 3: Configure Kafka

1. Create 'datas' directory under the `/opt/module/kafka/` directory
```
mkdir datas
```

2. Modify the configuration file `server.properties`:
```
cd /opt/module/kafka/config/
vim server.properties
```
```
#Modify the following content
broker.id=0
log.dirs=/opt/module/kafka/datas
zookeeper.connect=hadoop102:2181,hadoop103:2181,hadoop104:2181/kafka
```

3. Distribute Kafka

```
xsync kafka/
```

Different configurations for `broker.id=1` and `broker.id=2` should be modified on `hadoop103` and `hadoop104` in the `/opt/module/kafka/config/server.properties` file

4. Configure environment variables

Add Kafka environment variable configuration in the `/etc/profile.d/my_env.sh` file:

```
sudo vim /etc/profile.d/my_env.sh
```
Add the following content:
```
#KAFKA_HOME
export KAFKA_HOME=/opt/module/kafka
export PATH=$PATH:$KAFKA_HOME/bin
```

Refresh the environment variables
```
source /etc/profile
```

Distribute the environment variable file to other nodes and then `source` it

## Step 4: Cluster startup script
Create `kf.sh` in the `~/bin/` directory
