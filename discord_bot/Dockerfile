# ---- Build stage ----
FROM gradle:8.7-jdk17 AS build
WORKDIR /app

COPY build.gradle settings.gradle gradlew gradlew.bat /app/
COPY gradle /app/gradle

# dependency cache (실패해도 진행)
RUN ./gradlew --no-daemon dependencies || true

COPY src /app/src
RUN ./gradlew clean bootJar --no-daemon

# ---- Run stage ----
FROM eclipse-temurin:17-jre
WORKDIR /app

COPY --from=build /app/build/libs/*.jar app.jar

EXPOSE 8080
ENTRYPOINT ["java","-jar","/app/app.jar"]