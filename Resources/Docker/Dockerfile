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

ARG SonarProjectKey
ARG SonarHost
ARG SonarLogin
RUN mvn test sonar:sonar \
    -Dsonar.projectKey=$SonarProjectKey \
    -Dsonar.host.url=$SonarHost \
    -Dsonar.login=$SonarLogin \
    -Dsonar.projectDescription='RC MAVEN TEST' \
    -Dsonar.projectName='RC Pet Clinic Maven'


#############################################
#                  Run                      #
#############################################
FROM java:8-jdk-alpine
COPY --from=build /usr/src/app/target/*.jar ./SNAPSHOT.jar
EXPOSE 8080
CMD ["java", "-jar", "-Dspring.profiles.active=mysql", "SNAPSHOT.jar"]
