# Security
![image](https://user-images.githubusercontent.com/27158658/167381983-99c97dae-347c-48e6-b386-b1431f371873.png)


## 1. Contents
- [1. Contents](#1-contents)
- [2. Document History](#2-document-history)
- [3. Document Purpose](#3-document-purpose)
- [4. Introduction](#4-introduction)
- [5. The OWASP top 10](#5-the-owasp-top-10)
- [6. Sonarqube](#6-sonarqube)
- [7. OWASP top 10 CI/CD](#7-owasp-top-10-cicd)


## 2. Document History
| Version | Changes | Author | Date |
|---------|---------|--------|------|
| 0.1 | Initial Setup                                                           | Ruben Leerentveld | 09-05-2022 | 


## 3. Document Purpose
One of the learning outcomes covers security. In this document you will find my experiences on reasearching the OWASP top 10 and actions taken


## 4. Introduction
To improve my knowledge on security I will be doing a short research on what the OWASP top 10 is and best practices. 
Besides that I will let Sonarqube check my code on security risks and try some hacking myseld.

## 5. The OWASP top 10
The OWASP top 10 is a standard awareness document for developers and web application security. It represents the top 10 most critical security risks to web applications

### 5.1 A01:2021-Broken Access Control
Broken Access control is the weakness that allows an attacker to gain access to user accounts. This way the webapp can easily be exploited

### 5.2 A02:2021-Cryptographic failures
Cryptographic failures is formerly known as sensitive data exposure. this occurs when important data is stored or sent without encryption

### 5.3 A03:2021-Injection
Code injection or SQL injection occurs when invalid data is sent by the attacker in order to make the application do something it was not designed to do. A bad structured sql call can be an example here

### 5.4 A04:2021-Insecure Design
Insecure design is related to design flaws. 

### 5.5 A05:2021-Security Misconfiguration
Security misconfiguration is new in the OWASP top 10. A default test or root account that is still enabled on deployment is a great example.

### 5.6 A06:2021-Vulnerable and Outdated Components
When security risks (CVEs) are found, they are commonly fixed by a software update. If you do not update the software your application uses, you will still have these security risks. Updating commonly is the solution.

### 5.7 A07:2021-Identification and Authentication failures
Previously known as broken authentication, this risk describes the bad implementation of authentication and sessionmanagement related functions. When implemented incorrectly, an attacker is able to compromise passwords, keywords and sessions which lead to stolen user identity

### 5.8 A08:2021-Software and Data Integrity Failures
This risk focuses on software updates, critical data and Ci/CD pipelines used without verifying integrity (insecure deserialization added later on)

### 5.9 A09:2021-Security Logging and Monitoring Failures
Formerly known as insufficient logging and monitoring. logging and monitoring are activities that should be performed on a website frequently. Failure to do so leaves a site vulnerable to more severe compromising.

### 5.10 A190:2021-Server-Side Request Forgery
SSRF can happen whem a web application fetches a remote resource without validating the user-supplied URL. This allows an attacker to make the application send a crafted request to an unexpected destination

Underneath is a nice overview of how severity has changes past 4 years

![image](https://user-images.githubusercontent.com/27158658/167400711-ea2885ce-3c35-4edd-a100-4a014c8e12ef.png)


## 6. Sonarqube
To ensure quality of my code I implemented Sonarqube. On every push to master, sonarqube will check my codebase on quality, bugs, code smells, security hotspots and vulnerability.

After fixing some codesmells and security flags my analysis is quite neat!
![image](https://user-images.githubusercontent.com/27158658/167405575-815c0dce-0cbc-4f58-a76d-ac302b5363c0.png)

## 7. OWASP top 10 CICD
To ensure I have no vulnerabilities that occur in the OWASP top 10 I run an extra check in my Ci/Cd that checks on the CiCd practices

## 8. Authentication and Authorization
To make sure users can only access their own data, I built Authentication and Authorization.

And the best way to build that is, well, to not do it yourself :)

I am using google as my authentication provider.
Upon logging in, users get directed to the google login page where they will enter their credentials. 
After that, my Api receives their email and profile data which are needed in the application. 

First I signed up my app in de developer window of Google 
![image](https://user-images.githubusercontent.com/27158658/171005082-5c2f5f58-7b82-4aa3-a520-2e8fa2e9fc42.png)

After signing up I made a pair of credentials which I can use in my application to authenticate users
![image](https://user-images.githubusercontent.com/27158658/171113962-c887da98-f470-4598-9ba9-8cd683ef2ed1.png)

Authenticating with a third party provides looks like this
![image](https://user-images.githubusercontent.com/27158658/171114391-66b25222-aa90-436a-810a-e2cce0c4ac29.png)



