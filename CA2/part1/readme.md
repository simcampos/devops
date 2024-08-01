# Technical Report for CA2-Part1

## Introduction

This is a technical report for de Class Assignment 2 - Part 1, of the DevOps class from the "Switch - Curso de Especialização em Desenvolvimento de Software" made by Simão Campos, student nº 1231859.

The following report is a technical document that provides a detailed account of the steps taken to complete the assignment, including the commands used, the issues encountered, and the solutions implemented. The tutorial section provides a step-by-step guide to the assignment tasks.

## 1. Tutorial

1. Create CA2.Part1 directory on the Devops repository created on CA1.
2. Download https://bitbucket.org/pssmatos/gradle_basic_demo/ and add it to the Part1 directory.
3. Read the instructions available in the readme.md file and experiment with the
   application.
4. Add new task to server.
5. Add given unit test and execute it.
6. Add a new task of type Copy to be used to make a backup of the sources of the
   application.
   It should copy the contents of the src folder to a new backup folder.
7. Add a new task of type Zip to be used to make an archive (i.e., zip file) of the
   sources of the application.
   It should copy the contents of the src folder to a new zip file.
8. At the end of the part 1 of this assignment mark your repository with the tag
   ca2-part1.

### 1. Create CA2.Part1 directory on the Devops repository created on CA1.

```bash
mkdir CA2.Part1
```

### 2. Download https://bitbucket.org/pssmatos/gradle_basic_demo/ and add it to the Part1 directory.

```bash
git clone https://bitbucket.org/pssmatos/gradle_basic_demo/
```
Note: when transfer the content of the gradle_basic_demo to the CA2.Part1 directory, the .git directory should be removed.

```bash

## 3. Read the instructions available in the readme.md file and experiment with the application.

```bash
cd gradle_basic_demo
```
### Build
To build a .jar file with the application:

```bash
./gradlew build
```

### Run the server
Open a terminal and execute the following command from the project's root directory:
```bash
java -cp build/libs/basic_demo-0.1.0.jar basic_demo.ChatServerApp <server port>
```
Substitute by a valid por number, e.g. 59001

### Run a client
Open another terminal and execute the following gradle task from the project's root directory:
```bash
./gradlew runClient
```

After the build command the server and clients should be run in different terminals. The server should be run first.
The opened windows for the clients ask for a name and after that the messages can be sent between the clients.

### 4. Add new task to server.
On the build.gradle file, add the following task to the server:
```bash
task runServer(type:JavaExec, dependsOn: classes){
    group = "DevOps"
    description = "Launches a chat server that listens on port 59001"

    classpath = sourceSets.main.runtimeClasspath

    mainClass = 'basic_demo.ChatServerApp'

    args '59001'
}
```
Now the server can be run with the following command:
```bash
./gradlew runServer
```

### 5. Add given unit test and execute it.
Create a test directory on the src directory and add the following test to the Apptest.java file:
```java
@Test
public void testAppHasAGreeting() {
        App classUnderTest = new App();
        assertNotNull("app should have a greeting", classUnderTest.getGreeting());
        }
}
```

Run the test with the following command:
```bash
./gradlew test
```

### 6. Add a new task of type Copy to be used to make a backup of the sources of the application.
On the build.gradle file, add the following task to copy the src directory to a backup directory:
```bash
task backupSources(type: Copy) {
    from 'src'
    into 'backup'
}
```

Now the backup can be made with the following command:
```bash
./gradlew backupSources
```

### 7. Add a new task of type Zip to be used to make an archive (i.e., zip file) of the sources of the application.
On the build.gradle file, add the following task to zip the src directory:
```bash
task zipSources(type: Zip) {
    from 'src'
    archiveFileName = 'src.zip'
    destinationDir file('backup')
}
```
The src.zip file will be created in the backup directory.

Now the zip can be made with the following command:
```bash
./gradlew zipSources
```

### 8. At the end of the part 1 of this assignment mark your repository with the tag ca2-part1.
```bash
git tag ca2-part1
git push origin ca2-part1
```

