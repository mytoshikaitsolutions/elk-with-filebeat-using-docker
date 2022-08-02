FROM openjdk:8-jdk-alpine
VOLUME /tmp
ADD target/springboot-with-elk.jar springboot-with-elk.jar
ENTRYPOINT ["java", "-jar", "/springboot-with-elk.jar" ]