## Use JDK 17 base image
#FROM openjdk:17-jdk
#
## Set the working directory inside the container
#WORKDIR /app
#
## Copy the JAR file from the target directory into the container
#COPY target/api-gateway-0.0.1-SNAPSHOT.jar /app/api-gateway.jar
#
## Expose the port on which the Spring Boot application will run
#EXPOSE 8087
#
## Define the command to run the Spring Boot application
#CMD ["java", "-jar", "api-gateway.jar"]

# ---------- Stage 1: Build the application ----------
FROM maven:3.9.6-eclipse-temurin-17 AS build

# Set working directory for build stage
WORKDIR /app

# Copy Maven project files
COPY pom.xml .

# Download Maven dependencies first (cache benefit)
RUN mvn dependency:go-offline

# Copy the full source code
COPY src ./src

# Build the application (skip tests for faster build)
RUN mvn clean package -DskipTests


# ---------- Stage 2: Runtime image ----------
FROM openjdk:17-jdk

# Set working directory inside runtime container
WORKDIR /app

# Copy only the built JAR from build stage
COPY --from=build /app/target/api-gateway-0.0.1-SNAPSHOT.jar /app/api-gateway.jar

# Expose Spring Boot app port
EXPOSE 8087

# Run the Spring Boot application
CMD ["java", "-jar", "api-gateway.jar"]
