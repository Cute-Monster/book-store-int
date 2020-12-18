#############################################
#               Build & Test                #
#############################################
FROM maven:3.6.3-jdk-11-openj9  AS build
ARG SQL_USER
ARG SQL_PASS
ARG SQL_URL
WORKDIR /usr/src/app
COPY ./pom.xml ./pom.xml
COPY ./src ./src
RUN mvn dependency:go-offline -B
RUN mvn package



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
FROM openjdk:11.0.9.1-jre-buster
COPY --from=build /usr/src/app/target/*.jar ./SNAPSHOT.jar
ENV MYSQL_USER=${SQL_USER}
ENV MYSQL_PASS=${SQL_PASS}
ENV MYSQL_URL=${SQL_URL}
RUN echo "${MYSQL_URL}"
EXPOSE 8080
CMD ["java", "-jar", "./SNAPSHOT.jar"]
