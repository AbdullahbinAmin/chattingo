# --- STAGE 1: Maven Build Environment ---
# This stage uses a Maven/JDK base image to build the Spring Boot application.
FROM maven:3.9.4-eclipse-temurin-17 AS builder

# Set the working directory inside the container.
WORKDIR /app

# Copy the Maven project files. This helps with caching.
COPY pom.xml .

# This command downloads all the dependencies, but doesn't build the app.
# If the pom.xml doesn't change, Docker can reuse this layer.
RUN mvn dependency:go-offline

# Copy the rest of the application source code.
COPY src ./src

# Build the Spring Boot application and create the executable JAR file.
RUN mvn clean package -DskipTests

# --- STAGE 2: Production Runtime ---
# This stage uses a lightweight JRE (Java Runtime Environment) to run the application.
FROM eclipse-temurin:17-jre-alpine

# Set the working directory.
WORKDIR /app

# Copy the built JAR file from the 'builder' stage.
# The JAR file is typically located in the 'target' directory.
COPY --from=builder /app/target/*.jar ./app.jar

# Expose port 8080 so the Spring Boot application can be accessed.
EXPOSE 8080

# The command to run the Spring Boot application.
CMD ["java", "-jar", "app.jar"]