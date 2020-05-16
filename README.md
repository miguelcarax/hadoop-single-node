# Ansible Role: Hadoop Single Node

Ansible role that setup a single node Hadoop installation on RedHat/CentOS 7


## Requirements

+ At least Java 8
+ `JAVA_HOME` environment variable need to be setted 
+ EPEL repository enabled


## Role variables

The version of Hadoop to be installed. Only tested on `3.0.0` and above

	hadoop_version: 3.2.1

Hadoop installation directory, will create a `hadoop-x.y.z` directory under it

	hadoop_installation_dir: /opt

Some custom Hadoop properties to be added on Hadoop `.xml` configuration files. 

Currently support:

+ `core-site.xml`
+ `hdfs-site.xml`
+ `yarn-site.xml`
+ `mapred-site.xml`

This are the bare-minimum to allow everything to works fine

	hadoop_core_site_properties:
	  - name: fs.defaultFS
	    value: 'hdfs://localhost:9000'
	
	hadoop_hdfs_site_properties:
	  - name: dfs.replication
	    value: 1
	
	hadoop_yarn_site_properties:
	  - name: yarn.nodemanager.aux-services
	    value: mapreduce_shuffle
	  - name: yarn.nodemanager.env-whitelist
	    value: "JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,\
	     CLASSPATH_PREPEND_DISTCACHE,HADOOP_YARN_HOME,HADOOP_MAPRED_HOME"
	
	hadoop_mapred_site_properties:
	  - name: mapreduce.framework.name
	    value: yarn
	  - name: mapreduce.application.classpath
	    value: '$HADOOP_MAPRED_HOME/share/hadoop/mapreduce/*:$HADOOP_MAPRED_HOME/share/hadoop/mapreduce/lib/*'


## Dependencies

+ geerlingguy.repo-epel
+ geerlingguy.java


## Example of a Playbook

	---
	- name: Converge
	  hosts: all
	  become: true
	
	  vars:
	    java_packages:
	      - 'java-1.8.0-openjdk-1.8.0.252.b09-2.el7_8.x86_64'
	    java_home: '/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.252.b09-2.el7_8.x86_64/jre/'
	    hadoop_version: 3.2.1
	
	  roles:
	    - role: geerlingguy.repo-epel
	    - role: geerlingguy.java
	    - role: hadoop-single-node


## License

MIT
