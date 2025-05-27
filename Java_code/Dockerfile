#------------------- Stage 1 ---------------- #
FROM maven:3.9.5-eclipse-temurin-17 AS builder
WORKDIR /app

COPY pom.xml .
COPY src ./src
COPY Webcontent ./Webcontent

RUN mvn clean package -DskipTests

ADD https://repo1.maven.org/maven2/com/github/jsimone/webapp-runner/8.0.33.0/webapp-runner-8.0.33.0.jar Webapp-runner.jar

#------------------- Stage 2 ---------------- #
FROM eclipse-temurin:17-jdk-alpine
WORKDIR /app

COPY --from=builder /app/target/*.war app.war
COPY --from=builder /app/Webapp-runner.jar Webapp-runner.jar

EXPOSE 8080

ENTRYPOINT ["java", "-jar", "Webapp-runner.jar", "app.war"]
