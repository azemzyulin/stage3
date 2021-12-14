ARG DOCKER_IMAGE_BUILDER=gradle:6.9.1-jdk11
ARG DOCKER_IMAGE_RUNNER=registry.access.redhat.com/ubi8/openjdk-11
FROM ${DOCKER_IMAGE_BUILDER} AS builder
COPY . /home/gradle/project
WORKDIR /home/gradle/project
RUN gradle wrapper
RUN ./gradlew tasks bootjar
### Package stage
FROM ${DOCKER_IMAGE_RUNNER}
COPY --from=builder /home/gradle/project/build/libs/*.jar /deployments/app.jar
COPY --from=builder /home/gradle/project/src/main/resources/application.properties /deployments/
COPY --from=builder /home/gradle/project/src/main/resources/file.json /deployments/
EXPOSE 8080
WORKDIR /deployments
