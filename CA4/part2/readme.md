# Technical Report for CA4-Part2

## Introduction

This is a technical report for de Class Assignment 4 - Part 2, of the DevOps class from the "Switch - Curso de Especialização em Desenvolvimento de Software" made by Simão Campos, student nº 1231859.

The following report is a technical document that provides a detailed account of the steps taken to complete the assignment, including the commands used, the issues encountered, and the solutions implemented. The tutorial section provides a step-by-step guide to the assignment tasks.

## 1. Create the Dockerfiles

Inside the project CA2-part2/react-and-spring-data-rest-basic, create the Dockerfiles for the "db" and "web" machines.
At the application.properties file, change the spring.datasource.url to the following:
```bash
spring.datasource.url=jdbc:h2:tcp://db:9092/./jpadb
```

### 1.1. Dockerfile for the "db" machine

Create the Dockerfile for the "db" machine with the following content:

```bash
FROM gradle:jdk21
WORKDIR /opt/h2
RUN wget https://repo1.maven.org/maven2/com/h2database/h2/1.4.200/h2-1.4.200.jar -O h2.jar
EXPOSE 8082
EXPOSE 9092
CMD ["java", "-cp", "h2.jar", "org.h2.tools.Server", "-ifNotExists", "-web", "-webAllowOthers", "-webPort", "8082", "-tcp", "-tcpAllowOthers", "-tcpPort", "9092"]
```

### 1.2. Dockerfile for the "web" machine

Create the Dockerfile for the "web" machine with the following content:

```bash
FROM gradle:jdk21
WORKDIR /app
COPY . /app
EXPOSE 8080
CMD ["java", "-jar", "build/libs/react-and-spring-data-rest-basic-0.0.1-SNAPSHOT.jar"]
```

## 2. Make the docker-compose.yml file

Create the docker-compose.yml file with the following content:

```bash
services:
  db:
    build:
      context: .
      dockerfile: Dockerfile-db
    container_name: h2-db
    ports:
      - "8082:8082"
      - "9092:9092"
    volumes:
      - h2-data:/opt/h2-data
      - db-backup:/backup

  web:
    build:
      context: .
      dockerfile: Dockerfile-web
    container_name: spring-web
    ports:
      - "8080:8080"
    depends_on:
      - db

volumes:
  h2-data:
    driver: local
  db-backup:
    driver: local
```

## 3. Run the project

Run the project with the command `docker-compose up`

The project will start the "db" and "web" machines.
The "db" machine will start the H2 database at the address `http://localhost:8082`. 
The JDBC URL is `jdbc:h2:tcp://db:9092/./jpadb`. I the can be checked at the application.properties file.
The "web" machine will start the application at the address `http://localhost:8080/basic-0.0.1-SNAPSHOT`.

## 4. Make a backup of the database at the "db" machine

To make a backup of the database at the "db" machine run the following command:

```bash
docker exec -it h2-db bash -c "cp /opt/h2/jpadb.mv.db /backup"
```

If you want to copy the database to your local machine, run the following command:

```bash
docker cp 81ea4d14f207:/opt/h2/jpadb.mv.db .
```

The `81ea4d14f207` is the container id of the "db" machine. You can get the container id by running the command `docker ps`.

You can enter na explore the machine through the bash with the following command:

```bash 
docker exec -it h2-db bash
```

## 5. Publish the project at the Docker Hub

To publish the project at the Docker Hub, you need to create an account at the Docker Hub and login with the command `docker login`.

After that, you need to tag the images with the following commands:

```bash
docker tag react-and-spring-data-rest-basic-db 1231859/h2-db
docker tag react-and-spring-data-rest-basic-web 1231859/spring-web
```
This is using my local image names and account. You need to change the names to your local images names and account.

And then push the images to the Docker Hub with the following commands:

```bash
docker push 1231859/h2-db
docker push 1231859/spring-web
```

The machines can be verified at the Docker Hub at the following addresses: The image is now available at https://hub.docker.com/r/1231859
