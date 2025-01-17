# This is a Sample SpringBoot Project

### Prerequisite
JAVA 17 or Higher Version


MySql 8.x Version

SonarQube 9.9 Version 

Gradle 8.x version 





MySql 8.x Version 

SonarQube 9.9 Version

STS or IntelliJ Idea or Eclipse IDE

**Clone the project to specified folder and import it into Your Favourite IDE**

```bash
git clone https://github.com/flabdev/Spring-Boot-Starter.git
```

**Setting Up Local Properties File**

In the cloned repository, navigate to src/main/resources and copy the application.properties file 
Past the file in any location on your computer other than the project folder and name it application-local.properties 
add the mysql details for the project 

Change the datasourcename and datasourcePassword in the file to below values 

```bash

spring.datasource.url=@ConnectionURL 
spring.datasource.username=@UserName
spring.datasource.password=@Password
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
```
## Project execution 

Navigate to root folder and open the terminal and execute below command 

```bash
gradle clean build

```
It will generate JaCoco report as well as jar for our application 

jacoco report location build/jacocoHtml/index.html
jar file location build/libs/Spring-Boot-Starter-0.0.1-SNAPSHOT.jar

Execute the below command
```bash
java -jar build/libs/Spring-Boot-Starter-0.0.1-SNAPSHOT.jar --spring.profile.active=local --spring.config.location=@YourApplicationPropertiesLocation
```

App Will be run on port 9090 

Open the Postman and verify 

## [Integrate sonarqube into the springboot project](https://github.com/flabdev/Spring-Boot-Starter/wiki)

**We need JAVA 11 or 17 for sonarqube**

**To run sonarqube**

```bash
./gradlew sonar -D "sonar.projectKey=Spring-Boot-Starter" -D "sonar.host.url=http://localhost:9000" -D "sonar.login={token generated while integeration}"
```
To check your code quality go to your browser
where sonarqube is running http://localhost:9000
go to your project and See the status of code


# Dockerizing Spring-Boot-Starter project 

Step1:  
pull the mysql image from docker hub
```bash
docker pull mysql
```

Step2:  
create a docker network to communicate Springboot app and MySql database

```bash
docker network create springboot-mysql-net
```
To verify

```bash
docker network ls
```

Step3:
Run the mysql container in the network
```bash
docker run --name mysqldb --network springboot-mysql-net -e MYSQL_ROOT_PASSWORD=1234 -e MYSQL_DATABASE=student -e MYSQL_USER=sa -e MYSQL_PASSWORD=1234 -d mysql:latest
```
To verify
```bash
docker ps
```

Step4:
Run Mysql in interactive mode

```bash
docker exec -it [containerid] bash
```
after that type

```bash
mysql -usa -p1234
```


Step5:
update application.properties file

```bash
spring.datasource.driver-class-name =com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://mysqldb:3306/student
spring.datasource.username=sa
spring.datasource.password=1234
spring.jpa.show-sql=true
spring.jpa.hibernate.ddl-auto=update
spring.jpa.properties.hibernate.dialect = org.hibernate.dialect.MySQL5Dialect
server.port=9090

```

Step6:
Build the spring boot docker image 
in IntelliJ Idea terminal

```bash
docker build -t springbootstarter .
```
To verify

```bash
docker images
```
Step7:
Start the springboot container on the same network


```bash
docker run --network springboot-mysql-net --name springboot-container -p 9090:9090 -d springbootstarter
```




















# Testcontainers SpringBoot
This quick starter will guide you to configure and use Testcontainers in a SpringBoot project.

## 1. Setup Environment
Make sure you have Java 8+ and a [compatible Docker environment](https://www.testcontainers.org/supported_docker_environment/) installed.
If you are going to use Maven or gradle build tool then make sure Java 17 or higher version is installed.



For example:
```shell
$ java -version
openjdk version "17.0.4" 2022-07-19
OpenJDK Runtime Environment Temurin-17.0.4+8 (build 17.0.4+8)
OpenJDK 64-Bit Server VM Temurin-17.0.4+8 (build 17.0.4+8, mixed mode, sharing)

$ docker version
... 
Server: Docker Desktop 4.12.0 (85629)
 Engine:
  Version:          20.10.17
  API version:      1.41 (minimum version 1.12)
  Go version:       go1.17.11
...

```

## 2. Run Tests


The sample project uses JUnit tests and Testcontainers to run them against actual databases running in containers.

Run the command to run the tests.
```shell
$ ./gradlew test //for Gradle
$ ./mvnw verify  //for Maven
```

The tests should pass.

# Consumer Driven Contracts with Pact
This quick starter will guide you to configure and use Testcontainers in a SpringBoot project.

## 1. Setup Environment
Make sure you have Java 8+ and a [compatible Docker environment](https://www.testcontainers.org/supported_docker_environment/) installed.
If you are going to use Maven build tool then make sure Java 17+ is installed.

## 2. Generate the pact file
Use the below gradle command to verify the contracts
```shell
$ ./gradle test --tests PactConsumerJUnit5Test
```
Pact file should generated under path
```
/build/pacts/UserConsumer-UserServiceJUnit5.json
```

## 3. Verifying contracts
Use the below gradle command to verify the contracts
```shell
$ ./gradle pactVerify
```
The tests should pass.

## 4. Running a Pact Broker locally
The pact broker requires a postgres database to work, so use docker compose to spin up and configure containers.

## 5. Publishing Pact contracts
Use the below command to publish the generated contracts
```shell
$ ./gradle pactPublish
```
Go to a browser and visit http://localhost:9292/ to open the broker web UI.

## 6. Retrieving and verifying contracts
Use the below command to publish the generated contracts
```shell
$ ./gradle pactVerify
```
