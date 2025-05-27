# ----------- Stage 1: Build the WAR using Maven -----------
FROM maven:3.9.5-eclipse-temurin-17 AS builder
WORKDIR /app

# Copy Maven project files
COPY pom.xml .
COPY src ./src

# Run Maven package (skip tests for faster builds)
RUN mvn clean package -DskipTests

# Download webapp-runner JAR (Tomcat embedded)
ADD https://repo1.maven.org/maven2/com/github/jsimone/webapp-runner/8.0.33.0/webapp-runner-8.0.33.0.jar Webapp-runner.jar

# ----------- Stage 2: Create runtime image with WAR + runner -----------
FROM eclipse-temurin:17-jdk-alpine
WORKDIR /app

# Copy WAR and runner from build stage
COPY --from=builder /app/target/*.war app.war
COPY --from=builder /app/Webapp-runner.jar Webapp-runner.jar

# Expose default HTTP port
EXPOSE 8080

# Run the application
ENTRYPOINT ["java", "-jar", "Webapp-runner.jar", "app.war"]


