

Dockerfile
```Dockerfile
FROM openjdk:21-slim  
ARG JAR_fILE=target/*.jar  
COPY ./target/demo-0.0.1-SNAPSHOT.jar app.jar  
  
ENTRYPOINT ["java", "-jar", "/app.jar"]
```

docker build : 
```
docker build -t <name> .
```
. : current location

 docker run : 
```
docker run -p 8080:9090 <image-id>
```
9090 : container port on system
9090 : your host port on which it would be reflected