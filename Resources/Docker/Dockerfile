#############################################
#                  Build                    #
#############################################
FROM maven:3.6.0-jdk-11-slim  AS build
WORKDIR /usr/src/app
COPY ./pom.xml ./pom.xml
COPY ./src ./src
RUN mvn dependency:go-offline
RUN mvn package


#############################################
#                 Scanner                   #
#############################################
FROM build as scanner
RUN mvn test sonar:sonar \
    -Dsonar.projectKey=Maven-CR-CI \
    -Dsonar.host.url=https://sonarqube.trydevops.com \
    -Dsonar.login=62f01a67de1b8869ed7f0f1f46df7c159efca3f7 \
    -Dsonar.projectDescription='RC MAVEN TEST' \
    -Dsonar.projectName='RC Pet Clinic Maven'


#############################################
#                  Run                      #
#############################################
FROM java:8-jdk-alpine
COPY --from=build /usr/src/app/target/*.jar ./SNAPSHOT.jar
EXPOSE 8080
CMD ["java", "-jar", "-Dspring.profiles.active=mysql", "SNAPSHOT.jar"]
