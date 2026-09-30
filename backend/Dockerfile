ARG MAVEN_IMAGE=maven:3.9.6-eclipse-temurin-17-alpine
ARG RUNTIME_IMAGE=eclipse-temurin:17-jre-alpine

FROM ${MAVEN_IMAGE} AS builder

WORKDIR /build

COPY pom.xml .

RUN mvn -B dependency:go-offline

COPY src ./src

RUN mvn -B clean verify


FROM ${RUNTIME_IMAGE} AS prod

WORKDIR /app

RUN addgroup -S app && adduser -S app -G app

COPY --from=builder /build/target/*.jar ./app.jar

USER app
EXPOSE 8080

ENTRYPOINT ["java", "-jar", "app.jar"]