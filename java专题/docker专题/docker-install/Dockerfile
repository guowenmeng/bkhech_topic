FROM openjdk:11-slim
MAINTAINER guowm
ADD web-0.0.1-SNAPSHOT.jar  /data/app.jar
EXPOSE 8080
ENTRYPOINT ["sh", "-c", "java -jar -Dspring.profiles.active=test /data/app.jar"]
  