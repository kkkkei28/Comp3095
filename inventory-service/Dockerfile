# Build Stage
# ----------
FROM gradle:8-jdk22 AS builder

# Copy the entire project with proper ownership
COPY --chown=gradle:gradle . /home/gradle/src

# Set the working directory
WORKDIR /home/gradle/src

# Build the project, excluding tests
RUN gradle build -x test

# Package Stage
# ---------------
FROM openjdk:22-jdk-slim

# Create a non-root user for running the application
RUN useradd -ms /bin/bash appuser

# Switch to the non-root user
USER appuser

# Create the application directory
RUN mkdir /app

# Copy the built JAR file from the builder stage
COPY --from=builder /home/gradle/src/build/libs/*.jar /app/inventory-service.jar

# Set environment variables
ENV POSTGRES_USERNAME=admin \
    POSTGRES_PWD=password

# Expose the application port
EXPOSE 8083

# Define the entry point to run the application
ENTRYPOINT ["java", "-jar", "/app/inventory-service.jar"]
