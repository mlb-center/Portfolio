# Security
![image](https://user-images.githubusercontent.com/27158658/173853778-7b202652-cf50-441b-beb3-52d52402501d.png)


## 1. Contents
- [1. Contents](#1-contents)
- [2. Document History](#2-document-history)
- [3. Document Purpose](#3-document-purpose)
- [4. Introduction](#4-introduction)
- [5. Implementation](#4-implementation)


## 2. Document History
| Version | Changes | Author | Date |
|---------|---------|--------|------|
| 0.1     | Initial setup                                                           | Ruben Leerentveld | 15-06-2022 | 


## 3. Document Purpose
For the learning outcome "Security by design" I had already researched the OWASP top 10 and used Google Authentication.
To raise this learning outcome to an ```Proficient``` I have to implement role based authorization.


## 4. Introduction
I built authentication and authorization on my api with the Spring boot security framework. 
When a user logs in he will receive a cookie in his browser. He is now able to use controllers that are available for his role..

## 5. Implementation
To implement autentication and authorization I first implemented a h2 in-memory database.
Second, I added models for users and Roles. The current roles available are "USER" and "MOD".
Authentication is done by a JWT token in the cookie header, next, the authorization is handled by a role assigned to a user.
Underneath is a sequence diagram that shows the process of authentication and authorization

### 5.1 Login as USER
![image](https://user-images.githubusercontent.com/27158658/173853631-3a5fafeb-45dd-41ce-b0bf-6a0be170217c.png)

Step one is to register a new user! I first register a user with the ```USER``` role
![image](https://user-images.githubusercontent.com/27158658/173855170-76e9cf7c-d936-475f-9b97-1387d59543be.png)

Now that the user is registered in the H2-database I can login with his credentials. 
![image](https://user-images.githubusercontent.com/27158658/174024015-47f67770-e94c-4845-a92e-a9b4cd2d7206.png)

And when the login credentials get accepted I will receive a cookie in the header. This cookie I can use to authenticate and authoriza as a user

![image](https://user-images.githubusercontent.com/27158658/174024073-81add461-2be1-479e-a1b1-7fe5f135d278.png)

Here I am now able to view the user content with my cookie

![image](https://user-images.githubusercontent.com/27158658/174024294-625aaa82-e110-46fb-9dc3-993a6dbe90fa.png)

But the moderator function is forbidden since I do not have a moderator role!

![image](https://user-images.githubusercontent.com/27158658/174024344-60cef828-5841-4330-ab11-8083d5eef237.png)


### 5.2 Login as MODERATOR


![image](https://user-images.githubusercontent.com/27158658/174025540-127657b9-8242-41aa-be5d-fbb81afbf87e.png)

![image](https://user-images.githubusercontent.com/27158658/174025672-46585e50-b2c5-4422-8fa0-0c6607e35d1e.png)








