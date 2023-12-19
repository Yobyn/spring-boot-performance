# Instructions
Please follow the instructions below to run the performance tests yourself.

## Required software
The following software is required to run the performance tests:

- Java 21
- Docker engine
- Docker Compose
- [Apache JMeter](https://jmeter.apache.org/)

## Compiling the code
There are 2 Gradle Spring Boot projects that need to be compiled to a local Docker imgame. They are in the sub-folders servlet and webflux.

1. Go to the sub folder.
2. Run the following command:

**Windows:**
```bash
gradlew.bat bootBuildImage
```

**Linux / Mac:**
```bash
./gradlew bootBuildImage
```
3. This will create one of the following 2 standard JVM docker images depending on the sub folder:
- servlet &#8594; people-servlet-jvm21-sb320:0.0.1
- webflux &#8594; people-webflux-jvm21-sb320:0.0.1

4. To create the GraalVM Docker images update the file **build.gradle** and uncomment the following line:
```
// id 'org.graalvm.buildtools.native' version '0.9.28'
```

5. Run steps 2 again to crete the GraalVM native image.

6. This will create one of the following 2 GraalVM native images depending on the sub folder:
- servlet &#8594; people-servlet-gvm21-sb320:0.0.1
- webflux &#8594; people-webflux-gvm21-sb320:0.0.1

## Running the code
In order to run the Docker container created above for one of the test scenarios do the following:

1. Go to the **./Docker** sub folder containing a number of Docker Compose files.
2. Run the following command.
```bash
docker-compose -f Servlet-Jvm21SB320.yml up -d
```
3. This will start up a PostgreSQL database container and then a Docker container for our test application.
4. To view and test the REST API manually navigate your web browser to:
http://localhost:9080/actuator/swagger-ui
5. To view the memory usage of the application navigate to:
http://localhost:9080/actuator/metrics/jvm.memory.used
6. To view the application logs including the startup time run the command:
```bash
docker logs docker-people-1
```
7. After running the tests the application and database can be shut down using the following command:
```bash
docker-compose -f Servlet-Jvm21SB320.yml down
```

## Running the tests
To run the performance tests do the following:

1. Open Apache Jmeter.
2. Open the JMtere test project file **./JMeter/Person service performance test.jmx**.
3. In the project view on the left, click on **Summary Report**.
4. Click the **Start** menu item to start the tests.
5. To clear the tests results before running another test, click the **Clear All** menu item.