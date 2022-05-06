
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
![image](https://user-images.githubusercontent.com/27158658/167108957-28933b02-34c9-4c0e-964e-39fd448a1886.png)


### 5.2 InfluxDB
InfluxDB is a time series paltform that is easy to deploy and most important, scalable (clustering)

I pulled the official docker image and started the service oin port 8086 (influx default)

![image](https://user-images.githubusercontent.com/27158658/167110900-cc366794-9139-465a-8ad6-b9d9d5acdf39.png)


### 5.3 Grafana


