# Rabbit Message Queueing
![image](https://user-images.githubusercontent.com/27158658/163825503-a1d5b40f-4516-497e-9bb9-de5f96c94144.png)

## 1. Contents
- [1. Contents](#1-contents)
- [2. Document History](#2-document-history)
- [3. Document Purpose](#3-document-purpose)
- [4. Introduction](#4-introduction)
- [5. Experience](#5-experience)


## 2. Document History
| Version | Changes | Author | Date |
|---------|---------|--------|------|
| 0.1 | Initial Setup                                                           | Ruben Leerentveld | 18-04-2022 | 


## 3. Document Purpose
In my personal project I implemented a RabbitMQ server and I learned how to make use of it.
In this document I will mark down my experiences working with RabbitMQ

## 4. Introduction


## 5. Experience

### 5.1 Deployment of RabbitMQ server
I have Docker running on my server at home so deploying a server was quite easy.
To run the docker server I entered the following command:
```docker run -it -d --rm --name rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq:3.9-management```
Because of the ```-d``` flag, the container now runs in the backgroud.
So I entered the following docker command where you are able to see all the running containers.

```docker cotnainer ls```

If you look close enough you can see 2 containers are running, the first one is the RabbitMQ server
![image](https://user-images.githubusercontent.com/27158658/163827139-79de60c3-0ada-4365-bbfe-e3bd8b4caabd.png)

### 5.1 First Publish
Now that the RabbitMQ server is running I can try to make the first publish.
The API that needs to publish messages to the RabbitMQ server is made in Spring Boot.
I do have experience with RabbitMQ but not in Spring Boot and the implementation in Spring Boot seems a bit harder than i thought.

What did work was publising messages via the management API!

In Postman I made the following request work:


```http://192.168.24.235:15672/api/exchanges/%2F/amqp.order/publish```
![image](https://user-images.githubusercontent.com/27158658/163830501-dbeb6ab9-171a-49a2-aa05-1838b171a203.png)


That call worked and I now have a message in my "amqp.order" exchange:
![image](https://user-images.githubusercontent.com/27158658/163829505-17edc48f-2282-44df-bee2-9830bedb6e85.png)

### 5.3 The exchange

Having a message ending up the the "amqp.order" is cool but it never gets routed. 
The feedback from the call states ```unrouted``` aswell.
In order to get a message in to a queue we need to bind the order_queue with a routing key.

In the amqp.order exchange I bind the routing key ```order``` with the order_queue:

![image](https://user-images.githubusercontent.com/27158658/163830038-788581c8-2298-4862-add2-00e9978659d8.png)

When I now publish a message it should end up in the order_queue if tagged with the ```order``` routing key!

### 5.4 The queue
I make a new publish request with the ```order``` routing key:

![image](https://user-images.githubusercontent.com/27158658/163829055-632c8dd4-ceec-46eb-8186-7f4cfdbdd9d3.png)

The message enters the amwp.order exchange and exits directly:


![image](https://user-images.githubusercontent.com/27158658/163830627-190881fd-75d9-440f-b180-d9461589c890.png)

And if we check the queue, there now lives the message we sent!

![image](https://user-images.githubusercontent.com/27158658/163830921-edf534a9-02fa-4268-acb8-773947a17891.png)

### 5.5 Publish & Get Order
In this section I want to prove that I now can publish and get messages

First I publish an order with an orderId and a customerId:

![image](https://user-images.githubusercontent.com/27158658/163860497-3d52394f-a531-4f03-a15e-18732138aa3c.png)

After it entered the exchange and it is routed to the right queue, I can now retrieve it back from the queue:

![image](https://user-images.githubusercontent.com/27158658/163860704-629831dd-6e66-4fb6-bbbf-29eaaa17f760.png)

And that gives the following result:

![image](https://user-images.githubusercontent.com/27158658/163860751-dec460c1-e770-41c3-aec0-12127edf4d1b.png)

In the payload you can see the message I published in the beginning

