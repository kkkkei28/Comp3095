# Stage 1: Build
FROM gradle:8-jdk22 AS builder

# Copy the entire multi-module project
COPY --chown=gradle:gradle .. /home/gradle/src

# Set the working directory
WORKDIR /home/gradle/src

# Build the specific service
RUN gradle :order-service:bootJar -x test

# Stage 2: Runtime
FROM openjdk:22-jdk-slim

# Create a non-root user for the application
RUN useradd -ms /bin/bash appuser

# Switch to the non-root user
USER appuser

# Create the application directory
RUN mkdir /app

# Copy the built JAR file from the builder stage
COPY --from=builder /home/gradle/src/order-service/build/libs/*.jar /app/order-service.jar

# Expose the application port
EXPOSE 8082

# Define the entry point to run the application
ENTRYPOINT ["java", "-jar", "/app/order-service.jar"]