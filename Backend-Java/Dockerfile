FROM maven:3.8.3-openjdk-17 AS build
WORKDIR /workspace
COPY pom.xml mvnw .mvn/ /workspace/
COPY src /workspace/src
# Build the application and create the jar (skip tests for faster dev builds)
RUN mvn -B -f /workspace/pom.xml clean package -DskipTests

FROM eclipse-temurin:17-jdk
WORKDIR /app
# Adjust the jar name if your artifactId/version change in pom.xml
COPY --from=build /workspace/target/backend-java-0.0.1-SNAPSHOT.jar app.jar
EXPOSE 3000
ENTRYPOINT ["java","-jar","/app/app.jar"]
FROM maven:3.8.3-openjdk-17 AS build
WORKDIR /workspace
COPY . /workspace
# Build the application (skip tests to speed up build in dev)
RUN mvn clean package -DskipTests

FROM eclipse-temurin:17-jdk
WORKDIR /app
# Copy the packaged jar from the build stage (artifactId-version from pom.xml)
COPY --from=build /workspace/target/backend-java-0.0.1-SNAPSHOT.jar app.jar
# Expose the port the app uses (spring boot log shows Tomcat initialized with port 3000)
EXPOSE 3000
ENTRYPOINT ["java","-jar","app.jar"]
