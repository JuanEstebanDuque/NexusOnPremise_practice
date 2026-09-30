ARG MAVEN_IMAGE=maven:3.9.9-eclipse-temurin-17
ARG RUNTIME_IMAGE=eclipse-temurin:17-jre-alpine

FROM ${MAVEN_IMAGE} AS builder

WORKDIR /build

COPY pom.xml .

RUN mvn -B dependency:go-offline

COPY src ./src

RUN mvn -B clean verify


FROM ${RUNTIME_IMAGE} AS prod

RUN addgroup -S app && adduser -S app -G app

WORKDIR /app

COPY --from=builder /build/target/*.jar ./app.jar

USER app
EXPOSE 8080

ENTRYPOINT ["java", "-jar", "app.jar"]