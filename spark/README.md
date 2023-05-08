# Spark Installation

## Config Linux Cluster

Master(10.0.2.100) and Slave01(10.0.2.101) use NAT network in VirtualBox to communicate with each other

In the hosts file of master and slave01

```
su - 
vim /etc/hosts
```

Add following host IP mapping

```
10.0.2.100 master
10.0.2.101 slave01
```

## Stop firewall

```bash
su - 
systemctl disable firewalld
```

## Passwordless

* In Master
  
```bash
su - hadoop
ssh-keygen
ssh-copy-id hadoop@master
ssh-copy-id hadoop@slave01
```

* In Slave01

```bash
su - hadoop
ssh-keygen
ssh-copy-id hadoop@master
ssh-copy-id hadoop@slave01
```


Using hadoop to install spark

## Download Spark

Given using Hadoop 2.7.x

```bash
su - hadoop
cd /opt/bit010/src
wget --no-check-certificate https://dlcdn.apache.org/spark/spark-3.2.4/spark-3.2.4-bin-hadoop2.7.tgz
```

## Install Spark

```bash
cd /opt/bit010
tar zxvf src/spark-3.2.4-bin-hadoop2.7.tgz
ln -s spark-3.2.4-bin-hadoop2.7 spark
```

## Environment Configuration

```bash
vim ~/.bashrc
```

Append following to the end of .bashrc

```
export SPARK_HOME=/opt/bit010/spark
export PATH=$PATH:$SPARK_HOME/bin
export PATH=$PATH:$SPARK_HOME/sbin
```

```bash
source ~/.bashrc
```

## Spark Master Configuration

### Edit spark-env.sh

```bash
cp /opt/bit010/spark/conf/spark-env.sh.template /opt/bit010/spark/conf/spark-env.sh
vim /opt/bit010/spark/conf/spark-env.sh
```

Adding following configuration

```
export SPARK_LOCAL_IP=10.0.2.100
export SPARK_MASTER_HOST=MASTER
export JAVA_HOME=${JAVA_HOME}
```

## Add Workers

Edit the configuration file slaves

```bash
vim /opt/bit010/spark/conf/slaves
```

Add following configuration

```
master
slave01
```

## Spark Slave01 Configuration

Copy Spark from Master to Slave01 

In Master

```bash
scp -r /opt/bit010/spark-3.2.4-bin-hadoop2.7 hadoop@slave01:/opt/bit010/spark-3.2.4-bin-hadoop2.7
```

In Slave01

```
su - hadoop
ln -s /opt/bit010/spark-3.2.4-bin-hadoop2.7 /opt/bit010/spark
```

### Environment Configuration

Edit ~/.bashrc

```
vim ~/.bashrc
```

Adding following configuration

```
export SPARK_HOME=/opt/bit010/spark
export PATH=$PATH:$SPARK_HOME/bin
export PATH=$PATH:$SPARK_HOME/sbin
```

## Spark Slave01 Configuration

```bash
vim /opt/bit010/spark/conf/spark-env.sh
```

Change following configuration

```
export SPARK_LOCAL_IP=10.0.2.101
export SPARK_MASTER_HOST=MASTER
export JAVA_HOME=${JAVA_HOME}
```

## Start Spark Cluster

```
/opt/bit010/spark/sbin/start-all.sh
```

## Stop Spark Cluster

```
/opt/bit010/spark/sbin/stop-all.sh
```