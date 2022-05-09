
# CMonitoring
![image](https://user-images.githubusercontent.com/27158658/166927493-a0c36355-ee4a-4727-a11e-1690a1e660af.png)

## 1. Contents
- [1. Contents](#1-contents)
- [2. Document History](#2-document-history)
- [3. Document Purpose](#3-document-purpose)
- [4. Introduction](#4-introduction)
- [5. Experience](#5-experience)


## 2. Document History
| Version | Changes | Author | Date |
|---------|---------|--------|------|
| 0.1 | Initial Setup                                                           | Ruben Leerentveld | 05-05-2022 | 


## 3. Document Purpose
In this document you will find my exeriences building a monitoring cluster with Telegraf, InfluxDB and Grafana.

## 4. Introduction
Earlier in the project I configured a RabbitMQ server that handles requests.
In a presentation hosted by Merel I learned that validation is the way to go from a "Beginning" to "Proficient" in my learning outcomes.
RabbitMQ is an important part of my system setup. In order to ensure functionality, availability and performance I built a monitoring cluster with Telegraf, InfluxDB and Grafana.


## 5. Experience
### 5.1 Telegraf
The first step is to get data from the RabbitMQ server. This is where I use ```Telegraf```
Telegraf is a server-based agent for collecting and sending all metrics and events from systems and IoT sensors (and RabbitMQ).
In the office where I work as Software Engineer we use telegraf to get data from devices like switches and routers!

To start a Telegraf service you need a ```Telegraf.conf``` file. In here you configure your inputs (what data will be collected) and the outputs (where data will be sent to)


The inputs of my ```Telegraf.conf``` look like this:
```pan
[[inputs.rabbitmq]]
  ## Management Plugin url. (default: http://localhost:15672)
  url = "http://192.168.24.235:15672"
  ## Tag added to rabbitmq_overview series; deprecated: use tags
  name = "rmq-server-1"
  ## Credentials
  username = "XXX"
  password = "XXX"
```
Some explaination:
- Telegraf will recognize the ```[[inputs.rabbitmq]]``` tag will take care of the rest!
- The ```URL``` value is where we state our RabbitMQ host and the management port! 🐇
- The username and password fields are where the crendentials of the rabbitmq server are stored.

Besides the ```Inputs``` you need to configure the ```Outputs```

My output is the influx database and looks like this:
```pan
[[outputs.influxdb]]
  urls = ["http://influxdb:8086"]
  database = "influx"
  timeout = "5s"
  username = "telegraf"
  password = "metricsmetricsmetricsmetrics"
```
Now the inputs and outputs are configured I can start my Telegraf container.
And we are up and running (like a rabbit 🐇)
![image](https://user-images.githubusercontent.com/27158658/167363538-d9edf47e-9b00-4368-8a47-d63189be15c0.png)


### 5.2 InfluxDB
InfluxDB is a time series paltform that is easy to deploy and most important, scalable (clustering)

I pulled the official docker image and started the service on port 8086 (influx default)

![image](https://user-images.githubusercontent.com/27158658/167363447-e59f95a0-f1cc-494c-9882-3f95e494e968.png)
When I now start the telegraf service, all metrics will be stored in the Influx database!

### 5.3 Grafana
For displaying the metrics I am going to use Grafana.

I pull the official Grafana docker image and host it on the grafana default port 3000
![image](https://user-images.githubusercontent.com/27158658/167363037-c46db1cf-915b-479c-820a-dbe5ae6a1c7d.png)

When I now visit port 3000 I get presented with a login!

![image](https://user-images.githubusercontent.com/27158658/167363818-ab7ae6a2-56f9-4766-9aeb-294b14803891.png)

The first step is configuring a datasource in grafana, which will be my influx database
Note that in the URL field I entered ```influxdb:8086```
If you are commmunicating between docker containers it is best practice to use the container name!

Little secret: I spent hours debugging because I entered LOCALHOST, which is ofcourse the localhost of the container, not my entire server... 😳
![image](https://user-images.githubusercontent.com/27158658/167364160-8c9719ca-82d8-48b8-acdc-fce5899ad941.png)
![image](https://user-images.githubusercontent.com/27158658/167364217-c75e897d-0f3b-49b6-bc31-68a3d6513a58.png)

After configuring the source, the fun part begins !
I'm adding a new dashboard to display data. The first thing I want to display is the number of messages currently hold on the RabbitMQ server
To get data from the influx database we use the Flux language.
![image](https://user-images.githubusercontent.com/27158658/167365300-16258c92-4a48-40d1-b27f-59fb561c2433.png)

- In the from field I select the RabbitMQ metrics where the host = 192.168.24.235
- Then I select the field "messages" en get the sum of that.

And the final result looks like this:

![image](https://user-images.githubusercontent.com/27158658/167365131-cf0c632a-e8e3-4701-aab8-b14611b69300.png)

And I checked my RabbitMQ, and indeed I got 23 messages on hold.
![image](https://user-images.githubusercontent.com/27158658/167365750-e43c6439-1aef-433c-9fd2-97ae6808d61d.png)

After playing around I managed to build a nice and clean dashboard:
![image](https://user-images.githubusercontent.com/27158658/167378007-6f93b460-762e-4c56-addb-fd65c2d0577b.png)

You are able to see:
- Number of messages on hold (Value)
- Number of messages on hold (Graph)
- Memory Available (Gauge)
- Memory Available (Graph)
- Number of exchanges (Value)
- Number of queues (Value)
- Number of connections (Value)
- Node status (Traffic light)

