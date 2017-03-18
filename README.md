# Elasticsearch Practice Project

A project for practicing using Elasticsearch.

## About this Project

The following are some points about this project:

* The host OS is CoreOS with an IP of 10.0.0.2
* Access Elasticsearch at http://10.0.0.2:9200. It will take a minute or so before its available after starting it.
* It runs an Elasticsearch cluster containing two nodes
* Each node runs inside a Docker container
* The containers use a Docker network to see one another

## Setup

You will require:

1. VirtualBox, downloaded from the [VirtualBox website](https://www.virtualbox.org/wiki/Downloads).
2. Vagrant, downloaded from the [Hashicorp website](https://www.vagrantup.com/downloads.html).
3. An SSH client, such as the one that ships with [Git for Windows](https://git-scm.com/download/win). Be sure to choose the option to include the Unix tools on the Windows command line.

To get started, clone this repository, `cd` into it and then issue the following command to create the environment:

```
~$ vagrant up
```

To stop the virtual machine:

```
~$ vagrant halt
```

To throw it away:

```
~$ vagrant destroy
```

## How to use

You can connect to the virtual machine using SSH:

```
~$ vagrant ssh
```

Then you can run Docker commands such as:

```
~$ sudo docker ps -a
~$ sudo docker logs es1
~$ sudo docker inspect es1
```

## Elasticsearch queries

Get node info:
* http://10.0.0.2:9200/_cat/nodes?v
* http://10.0.0.2:9200/_nodes

## TODO

* Figure out how to map a Docker volume to the container's */usr/share/elasticsearch/data* directory. Currently, this gives me an AccessDenied exception from Java. See https://github.com/docker-library/docs/issues/370.