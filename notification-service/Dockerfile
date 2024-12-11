# Stage 1: Build
FROM gradle:8-jdk22 AS builder

# Copy the entire project for multi-module build
COPY --chown=gradle:gradle .. /home/gradle/src

# Set the working directory to the project root
WORKDIR /home/gradle/src

# Build the specific service module
RUN gradle :notification-service:bootJar -x test

# Stage 2: Runtime
FROM openjdk:22-jdk-slim

# Create a non-root user for running the application
RUN useradd -ms /bin/bash appuser

# Switch to the non-root user
USER appuser

# Create the application directory
RUN mkdir /app

# Copy the built JAR file from the builder stage
COPY --from=builder /home/gradle/src/notification-service/build/libs/*.jar /app/notification-service.jar

# Expose the application port
EXPOSE 8088

# Define the entry point to run the application
ENTRYPOINT ["java", "-jar", "/app/notification-service.jar"]