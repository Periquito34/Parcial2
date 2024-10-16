# Usar una imagen base de Maven para compilar la aplicación
FROM maven:3.8.4-openjdk-11 AS build

# Establecer el directorio de trabajo dentro del contenedor
WORKDIR /app

# Copiar el archivo pom.xml y descargar las dependencias sin construir
COPY pom.xml .
RUN mvn dependency:go-offline -B

# Copiar el resto de los archivos del proyecto
COPY src ./src

# Compilar y empaquetar el JAR (sin ejecutar los tests para acelerar el proceso)
RUN mvn clean package -DskipTests

# Usar una imagen más ligera para ejecutar el JAR
FROM openjdk:11-jdk-slim

# Establecer el directorio de trabajo para la aplicación
WORKDIR /app

# Copiar el archivo JAR generado en la fase anterior
COPY --from=build /app/target/*.jar app.jar

# Exponer el puerto que usa Spring Boot
EXPOSE 8081

# Comando para ejecutar el JAR de la aplicación
ENTRYPOINT ["java", "-jar", "app.jar"]
