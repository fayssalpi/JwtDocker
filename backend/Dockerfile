#FROM openjdk:17
#VOLUME /demo
#WORKDIR /app
#COPY . /app

#COPY target/*.jar app.jar
#ENTRYPOINT ["java","-jar","/app/app.jar"]

# Use the official Maven image as the base image
#FROM maven:3.8.4-openjdk-17-slim

# Create a working directory for your application
#WORKDIR /app

# Copy the application source code into the container
#COPY . /app

# Build the application using Maven
#RUN mvn clean install

# Copy the built JAR file to the container's root directory
#COPY target/*.jar app.jar

# Copy the application JAR from the build image to the final image
#COPY --from=build /app/target/*.jar app.jar

# Set the entry point to run the application
#ENTRYPOINT ["java", "-jar", "app.jar"]

# Use the official Maven image as the base image
FROM maven:3.8.4-openjdk-17-slim AS build

# Set a working directory for the build phase
WORKDIR /app

# Copy only the project's POM file to leverage Docker layer caching
COPY pom.xml .

# Download the project dependencies and build the project's dependencies cache
RUN mvn dependency:go-offline

# Copy the entire application source code into the container
COPY . .

# Build the application JAR
RUN mvn package

# Use a lightweight JRE image as the final image
FROM openjdk:17-slim

# Set the working directory
WORKDIR /app

# Copy the application JAR from the build image to the final image
COPY --from=build /app/target/*.jar app.jar

# Set the entry point to run the application
ENTRYPOINT ["java", "-jar", "app.jar"]
