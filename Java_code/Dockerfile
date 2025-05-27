#-------------------Stage :1 ---------------- #
FROM maven:3.9.5-eclipse-temurin-17 AS builder 
WORKDIR /app
COPY pom.xml .
COPY src ./src
COPY Webcontent ./Webcontent
RUN mvn clean package -DskipTests

#---------------Stage : 2 ---------#

FROM eclipse-temurin:17-jdk-alpine
WORKDIR /app
COPY --from=builder /app/target/*.war app.war
COPY --from=builder /app/target/webapp-runner.jar webapp-runner.jar
EXPOSE 8080
ENTRYPOINT ["java","-jar","webapp-runner.jar","app.war"]

