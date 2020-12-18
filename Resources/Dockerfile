#############################################
#               Build & Test                #
#############################################


FROM maven:3.6.3-jdk-11-openj9  AS build
WORKDIR /usr/src/app
COPY ./pom.xml ./pom.xml
COPY ./src ./src
ENV MYSQL_USER=runtov
ENV MYSQL_PASS=21AJjEQE9yToMrAa
ENV MYSQL_URL=jdbc:mysql:aurora://aurora-cluster-internship.cluster-cpct4armoank.us-east-1.rds.amazonaws.com:3306/runtov
RUN echo """$MYSQL_URL"""
RUN mvn dependency:go-offline -B
RUN mvn -B package
#RUN mvn spring-boot:repackage -Dspring.profiles.active=mysql /usr/src/app/target/*.jar



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
ARG SQL_USER
ARG SQL_PASS
ARG SQL_URL
ENV MYSQL_USER="runtov"
ENV MYSQL_PASS="21AJjEQE9yToMrAa"
ENV MYSQL_URL="jdbc:mysql:aurora://aurora-cluster-internship.cluster-cpct4armoank.us-east-1.rds.amazonaws.com:3306/runtov"
RUN echo """$MYSQL_URL"""
RUN echo '$SQL_URL'
EXPOSE 8080
CMD ["java", "-jar", "./SNAPSHOT.jar -Dspring.profiles.active=mysql"]
