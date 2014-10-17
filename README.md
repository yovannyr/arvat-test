Task
======

1. 搭建Tomcat，并利用模拟输出Log
2. 使用Logstash中间件收集Log到Redis中间件，并同步至Elastic Search
3. 使用Kibana展示Log信息


Work
======

### How-to use instance

1. ensure latest version of **Vagrant** and **Virtualbox** installed on local machine, or you can fetch software from below official links:

		Vagrant:
			http://www.vagrantup.com/downloads.html
		
		Virtualbox:
			https://www.virtualbox.org/wiki/Downloads
			
2. extract the zip package, or you can checkout from <https://github.com/ericstone57/arvat-test>
3. go to project workspace
4. run command to boot up the instance:

		vagrant up
		
5. wait a moment, it is need time to download vagrant box from internet, which limit by network speed. It will boot the instance up soon after you download complete.
6. check from you browser through <http://127.0.0.1:8080> with kibana3 dashboard
7. if you want to check inside of this instance, execute command:

		vagrant ssh
		// user `vagrant` have sudoer permission



### Services:

* <http://127.0.0.1:8080> kibana
* <http://127.0.0.1:9292> elastic search
* <http://127.0.0.1:8090> tomcat instance 1
* <http://127.0.0.1:8091> tomcat instance 2
* <http://127.0.0.1:8092> tomcat instance 3


### Configuration

#### Tomcat

* version 7
* 3 instances, listen on port 8080, 8081 and 8082.
* tomcat instance log output to folder `/var/lib/tomcat7/instanceX/logs/`
* an simple HTML welcome page

#### Elasticsearch

* version 1.3.4
* standard setup
* listen on port 9200 and 9300

#### logstash

* version 1.4.2
* standard install and setup
* configuration files put into `/etc/logstash/conf.d/`
* `/etc/logstash/conf.d/10-tomcat.conf`, collect tomcat logs, include access log and catalina.out
* `/etc/logstash/conf.d/30-redis.conf`, as log queue


#### Kibana

* version 3.1.1
* boot by Nginx
* files put into `/var/www/kibana3`

#### Redis

* version 1.8


### Explain how the system work

Data flow

	from software wise:
	
		logstash shipper --> Redis --> Elasticsearch --> Kibana
	
	from logic wise:
	
		collect and process logs --> queue logs --> store and indexes --> visualization



#### logstash

* collects and processes the logs coming, include function like: input, filter, re-format, parse and output
* create the indices
* as shipper, output log to Redis
* easy to managing events and logs

#### Redis

* queues up the logs for indexing from logstash

#### Elasticsearch

* base on document search engine, apache lucene
* persistent and indexes logs
* real-time searching and display
* distributed
* schema free
* provide RESTful API

#### Kibana

* logs data visualization
* time based
* base on AngualrJS, easy to adapt


Notice
======

The instance running slow due to I just give 2GB memory, for test purpose. You easy to adjust memory setting to speed up performance by change `Vagrantfile` in root of project.

		vb.customize ["modifyvm", :id, "--memory", "2048"]
		

Kibana dashboard not customize yet, it should ok to check Tomcat log and syslog by use the default one `Sample Dashboard`.
		
The vagrant box contains: jdk7, tomcat, nginx, logstash, redis and elasticsearch, so it is big then usual. nearly 4.06GB.

Let me know if you can not download the box from server successful, I will try another way send to your.
