# Usar una imagen base de Maven con OpenJDK 17 (Eclipse Temurin)
FROM maven:3.9.5-eclipse-temurin-17 AS build

# Establecer el directorio de trabajo
WORKDIR /app

# Copiar el archivo pom.xml y descargar las dependencias
COPY pom.xml .
RUN mvn dependency:go-offline

# Copiar el código fuente
COPY src ./src

# Construir el artefacto de Spring Boot
RUN mvn package -DskipTests

# Usar una imagen base de OpenJDK para ejecutar la aplicación
FROM eclipse-temurin:17-jdk

# Establecer el directorio de trabajo
WORKDIR /app

# Copiar el artefacto construido desde la etapa anterior
COPY --from=build /app/target/webflux-0.0.1-SNAPSHOT.jar .

# Exponer el puerto que usará la aplicación
EXPOSE 8084

# Definir el comando para ejecutar la aplicación
CMD ["java", "-jar", "webflux-0.0.1-SNAPSHOT.jar"]
