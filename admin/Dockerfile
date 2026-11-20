
# Étape 1 : construction de l'application
FROM maven:3.9-eclipse-temurin-21 AS build

WORKDIR /build

# Construire et installer le module partagé core
COPY core/pom.xml core/pom.xml
COPY core/src core/src

RUN mvn -B -f core/pom.xml clean install -DskipTests

# Construire le service admin
COPY admin/pom.xml admin/pom.xml
COPY admin/src admin/src

RUN mvn -B -f admin/pom.xml clean package -DskipTests

# Étape 2 : exécution
FROM eclipse-temurin:21-jre-alpine

WORKDIR /app

RUN addgroup -S spring && adduser -S spring -G spring

COPY --from=build --chown=spring:spring /build/admin/target/*.jar app.jar

USER spring

EXPOSE 8081

ENTRYPOINT ["java", "-jar", "app.jar"]
