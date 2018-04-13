---
layout: page
title: Spark for ElastXY
subtitle: Spark for ElastXY installation the easy way
bigimg: /img/documentation.jpg
---

# Spark installation the easy way

Here below some useful files to make the access to Spark easy for ElastXY users.

These are only sample files: please change configurations according to your environment.

## Pre-requisites

You should already have built ElastXY locally from sources, as explained on [documentation](/documentation#build-from-sources).


## WINDOWS OS

Tested with Windows 10 Professional, Windows 8.1.  

### Step 1: Install Apache Spark

- Download Apache Spark version "spark-2.2.0-bin-hadoop2.7" from [download site](https://spark.apache.org/downloads.html).
- Unzip in a directory of your choice, our `<SPARK_HOME>`.
  

### Step 2: Configure Apache Spark

Spark example basic configurations are provided with ElastXY project: log4j.properties, spark-defaults.conf.

For using them:
- Point to `<SPARK_HOME>/conf` directory and backup the two files mentioned, renaming them to ".orig".
- Get into ElastXY modules project root `<ELASTXY_HOME>`, the directory where you cloned and compiled ElastXY modules.
- Copy `elastxy-framework/etc/spark/conf` entire directory to `<SPARK_HOME>`: no files will be overriden.
- Open `<SPARK_HOME>/conf/spark-defaults.conf` and change local paths according to your system.
- Optionally, open `<SPARK_HOME>/conf/log4j.properties` and change log4j configurations as you like.


### Step 3: Run Apache Spark

Spark executables scripts are provided with ElastXY project as Windows commands to run a Spark Standalone Cluster.

For using them:
- Get into ElastXY modules project root `<ELASTXY_HOME>`, the directory where you cloned and compiled ElastXY modules.
- Copy `elastxy-framework/etc/spark/bin` entire directory to `<SPARK_HOME>`: no files will be overriden starting from a fresh Spark installation.
- Point to `<SPARK_HOME>/bin` directory.
- `run-master.cmd`: optionally, open the file and change the parameter `--webui-port` if you have processes that already use that port.
- `run-worker.cmd`: optionally, open the file and set local hostname or IP address (127.0.0.1 on localhost), and review available cores (-c) and memory (-m) parameters based on your machine wanted capacity.
- `run-history-server.cmd`: optionally, launches Server History.

Now, double-click and launch master script one time and worker two times: you should have an Apache Spark Driver up and running, with a Driver listening to default port 7077, and two Agents working for you accepting jobs!

To verify, please point with browser to:  
[http://localhost:8081](http://localhost:8081)

You should see Apache Spark manager console up and running.


## UNIX BASED OS

Tested with CentOS 7.1.

UNIX based systems have a similar procedure, please use ".sh" files instead of ".cmd" and you should be done!
TODO 

## Back to Multicolony tutorial

Now, you can go on with the [Multicolony tutorial](/documentation#multi-colony-tutorial).
