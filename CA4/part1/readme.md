# Technical Report for CA4-Part1

## Introduction

This is a technical report for de Class Assignment 4 - Part 1, of the DevOps class from the "Switch - Curso de Especialização em Desenvolvimento de Software" made by Simão Campos, student nº 1231859.

The following report is a technical document that provides a detailed account of the steps taken to complete the assignment, including the commands used, the issues encountered, and the solutions implemented. The tutorial section provides a step-by-step guide to the assignment tasks.

## Version1
1. clone the project
2. creat the docker file
3. build the docker container
4. run the docker container

## 1. clone the project

```bash
git clone https://bitbucket.org/pssmatos/gradle_basic_demo
```
Inside the gradle_basic_demo folder, execute the following command to build the project:
```bash
./gradlew build
```

## 2. creat the docker file
Creat the dockerfile inside the same package as the project
Inside the dockerfile add the following code:

```bash
FROM gradle:jdk21
WORKDIR /app
COPY ./build/libs/basic_demo-0.1.0.jar /app
EXPOSE 59001
ENTRYPOINT ["java", "-cp", "basic_demo-0.1.0.jar", "basic_demo.ChatServerApp", "59001"]
```

## 3. build the docker container
Execute the following command to build the docker container being inside the same package as the docker file.

```bash
docker build -t chat-server:v1 -f Dockerfile-v1 .     
```

## 4. run the docker container
Execute the following command to run the docker container being inside the same package as the docker file.

```bash
docker run -p 59001:59001 chat-server:v1    
```

## Version2
Make the same steps as made on version1, but don't build the project.
The project is going to be belt inside the docker.
In this case the dockerfile is:

```bash
FROM gradle:jdk21
WORKDIR /app
COPY . /app
RUN gradle clean build
EXPOSE 59001
ENTRYPOINT ["java", "-cp", "build/libs/basic_demo-0.1.0.jar", "basic_demo.ChatServerApp", "59001"]
```

Build the docker container
```bash
docker build -t chat-server:v2 -f Dockerfile-v2 .     
```

If the image is not pulling properly it can be pulled by the following command.
```bash
 docker pull openjdk:17-jdk-slim   
 ```

Run docker file.
```bash
docker run -p 59001:59001 chat-server:v2        
```

To text the app, open another terminal and execute the following gradle task from the project's root directory:
```bash
./gradlew runClient
```

## Publish image at Docker Hub

1. create an account at https://hub.docker.com/
2. login in the Docker desktop app
3. Execute the following command:
```bash
docker login
```

4. Tag the image with the following command:
```bash
docker tag chat-server:v2 1231859/chat-server:v2
```
1231859 is the username and chat-server is the repository name.

5. Push the image to Docker Hub with the following command:
```bash
docker push 1231859/chat-server:v2
```
The image is now available at https://hub.docker.com/r/1231859/chat-server