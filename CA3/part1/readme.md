# Technical Report for CA3-Part1

## Introduction

This is a technical report for de Class Assignment 3 - Part 1, of the DevOps class from the "Switch - Curso de Especialização em Desenvolvimento de Software" made by Simão Campos, student nº 1231859.

The following report is a technical document that provides a detailed account of the steps taken to complete the assignment, including the commands used, the issues encountered, and the solutions implemented. The tutorial section provides a step-by-step guide to the assignment tasks.

After installing the Ubuntu 20.04.3 LTS on the Virtual Box, the following steps were taken to complete the assignment:

### 1. install git
```bash
sudo apt update
sudo apt install git
```

### 2. install maven
```bash
sudo apt install maven
```
### 3. install java 17
```bash
sudo apt install openjdk-17-jdk openjdk-17-jre
```
verify version:
```bash
java -version
```

### 4. install gradle version 8.6
This step may not be needed, since the project already has the gradle wrapper. On my case when I run gradle wrapper, it installed a version 7.2.1, witch didn't work for the project. So I had to install the version 8.6.
```bash
wget https://services.gradle.org/distributions/gradle-8.6-bin.zip
sudo mkdir /opt/gradle
sudo unzip -d /opt/gradle gradle-8.6-bin.zip
echo "export GRADLE_HOME=/opt/gradle/gradle-8.6" >> ~/.bashrc
echo "export PATH=$PATH:$GRADLE_HOME/bin" >> ~/.bashrc
source ~/.bashrc
gradle -v
```

### 5. clone the repository
create folder and clone project
```bash
mkdir CA3
cd CA3

git clone https://github.com/simao-campos87/devops-23-24-JPE-1231859.git
(repository must be public)
```

### 6. configure maven wrapper and gradle wrapper

Maven wrapper and Gradle wrapper and must be given permission to execute.
```bash
chmod +x mvnw
chmod +x gradlew
```

### 7. run the project
#### * 7.1. run CA1 project
```bash
./mvnw spring-boot:run
```

to open project frontEnd on browser, get VM IP:
```bash
ip addr
```
put the IP and the port 8080 on the browser address.

#### * 7.2. run CA2 part1 project
```bash
./gradlew build
./gradlew runServer
```

To run the client in the same folder but on the computer terminal, on two different terminals:
```bash
./gradlew runClient --args="192.168.56.5 59001" 
```

#### * 7.3 run CA2 part2 project

on the basic folder:
```bash
./gradlew build
./gradlew bootRun
```
to open project frontEnd on browser:
get the VM IP:
```bash
ip addr
```
put the IP and the port 8080 on the browser address.


