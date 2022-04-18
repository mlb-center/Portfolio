
# Continuous Integration Continuous Delivery
![image](https://user-images.githubusercontent.com/27158658/163890196-234f0978-68ff-413d-ad31-ea55a704bd92.png)

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
To improve code quality and work more efficient I implemented a ci/cd. In this document I will mark down my experiences.

## 4. Introduction
I have built a cicd pipeline before in Jenkins. This semester I want to thrive my skills and use Github Actions!
My plan is to setup the pipeline with the following workflow

1. Lint code
2. Run sonarcloud on code
3. Create docker image on dockerhub (Only when pushed to master)

## 5. Experience

### 5.1 Setting up the pipeline
After pushing my project to GitHub I added a new folder ```.github/workflows```
In this folder all the workflows are stored.

In the snippet underneath you can see the foilder that contains 2 workflows
(I wrote this document after development so there are already 2 workflows built)
![image](https://user-images.githubusercontent.com/27158658/163890662-0bd23aa6-6fbd-4284-b600-aa7e90533a79.png)

### 5.2 Code linting
The first workflow I created runs on every push and hadnles Code Linting
```yaml
name: Lint Code Base

on: push

jobs:
  lint:
    name: Lint Code Base
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2
        
      - name: Run linter
        uses: github/super-linter@v4.7.3
        env:
          VALIDATE_ALL_CODEBASE: false
          DEFAULT_BRANCH: master
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
``` 
Frist the code checksout and then the linter is run, on every push

The results are positive!
![image](https://user-images.githubusercontent.com/27158658/163892148-bb86e181-4ff0-49ee-990c-0dbe652ce806.png)

### 5.3 Building Docker image

Each time when I push a stable version to master I want to have new Docker image ready to be deployed. 
To achieve that I have made this new workflow:
```yaml
name: Docker Build

on:
 push:
    branches:
      - master

jobs:
  docker:
    runs-on: ubuntu-latest
    steps:
      -
        name: Checkout
        uses: actions/checkout@v2
      -
        name: Set up QEMU
        uses: docker/setup-qemu-action@v1
      -
        name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v1
      -
        name: Login to DockerHub
        uses: docker/login-action@v1
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}
      -
        name: Build and push
        uses: docker/build-push-action@v2
        with:
          context: .
          push: true
          tags: rubenleerentveld/mlb-center-api:latest
``` 

After the workflow is done, there is an updated version of the docker image available on https://hub.docker.com/repository/docker/rubenleerentveld/mlb-center-api !

![image](https://user-images.githubusercontent.com/27158658/163892354-fd50b094-e54c-4fec-a71e-def197913950.png)

After a new version is deployed i can run the new image on docker with ```docker run rubenleerentveld/mlb-center-api```
When I run this command, docker will check if I have the most recent version and if not, it will install the changes!
```NOTE: docker does not download a complete new image! It only installs the new parts... this is what makes docker fast```
