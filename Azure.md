
# Azure
![image](https://user-images.githubusercontent.com/27158658/163884438-2f51ab63-ae20-48f9-b2b5-5e462fb576f2.png)

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
| 0.2 | Added Autoscaling                                                       | Ruben Leerentveld | 20-04-2022 | 



## 3. Document Purpose
For my individual project I am implementing Azure functionality to prove the learning outcome "Cloud services"
In this document I mark down my experiences and path regarding Azure!

## 4. Introduction
The goal is to build an API in Azure that can give me information on games that I want to buy tickets for
The functionalities of Azure that I probably need are:

- Azure Functions (For creating an API like endpoint)
- Azure Cosmos DB (For storing data)

## 5. Experience

### 5.1 Setting up the Resource group
To build a project a Azure you need to create a so called ```Resource group```
You can see a resource group as a container that bundles all your functionality for your project

![image](https://user-images.githubusercontent.com/27158658/163885803-5657a93a-0dca-4a56-8e6a-d751102b0d4e.png)

In the snippet above you can see my created resource group called "mlb-center"

### 5.2 Setting up Azure Cosmos DB
I picked Cosmos DB because that was the cheapest to run :)

I created a Cosmos DB with a ```Container``` called "mlb-center" and a ```Database``` called "games"

- A container bundles the databases under a custom name tag

This is what we have so far:
![image](https://user-images.githubusercontent.com/27158658/163886812-c1627a30-ab0a-488b-8b44-6302546421e2.png)

Now it is time to gain some bytes!
I was able to add data to the database very easily:
![image](https://user-images.githubusercontent.com/27158658/163887088-530c8fe3-0036-47e0-ae0b-62a9a4177f80.png)

Now we gained an entry we can inspect the entry:
![image](https://user-images.githubusercontent.com/27158658/163887178-e565f61a-e892-47e3-8283-ab4c8beb55ce.png)

### 5.3 Scaling the cosmos database
Scaling is an important functionality in cloud services. With autoscaling you can dynamically and automatically change the size of the database.
The default for scaling is set to autoscale with a max troughput of 1000 RU/s
(Aanvraageenheden worden uitgedrukt in RU per Seconde)

![image](https://user-images.githubusercontent.com/27158658/164175444-d75ed727-eb13-4622-ad0d-a70a18c26b68.png)

The settings above indicate that there are a maximum of 1000 request per second allowed, but that will scale down if less is used. Till a minimum of 10% of the max capacity.

The goal of autoscaling is reducing costs. With a max 1000 RU/s I'll pay 8.76 dollar per month, but when I change that to 100 I'll only pay 0.88 cents.

If we compare autoscaling with the manual setting we see a huge difference when making only 100 request per second when having a 1000 Ru/s database

|         | 10% of 1000 Ru/s| 100% of 1000 Ru/s |
|---------|-----------------|-------------------|
| Manual |$58.40|$58.40|
|autoscale|$8.76|$87.60|



### 5.4 Setting up Azure Functions
First, I made a function app called "mlb-center-app"
(There is already some data generated because I made this document after developing)
![image](https://user-images.githubusercontent.com/27158658/163887543-8106ba7d-7c2f-4569-9aa4-09a330af5798.png)
