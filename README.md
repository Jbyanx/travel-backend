# Travel Backend

Este proyecto es una API REST para la gestión de un sistema de reservas de vuelos, desarrollada con Spring Boot.

## Tecnologías
- Java 17
- Spring Boot 3.3.3 (Data JPA, Web, Security, Validation)
- PostgreSQL
- Docker
- MapStruct & Lombok
- JWT para autenticación

## Base de Datos
Para ejecutar la base de datos localmente usando Docker:

```bash
docker run --name postgres-pweb \
  -e POSTGRES_PASSWORD=sebaj \
  -e POSTGRES_USER=jabes \
  -e POSTGRES_DB=travel_db \
  -p 15432:5432 \
  -d postgres
```

## Ejecución
1. Asegúrate de tener Docker corriendo con la base de datos.
2. Configura las credenciales en `application.properties` si es necesario.
3. Asegúrate de configurar la variable de entorno `JWT_SECRET_KEY` para la seguridad.
4. Ejecuta el proyecto con `./gradlew bootRun`.

## Orden Sugerido para Inserción de Datos
Debido a las relaciones entre entidades, se recomienda este orden:
1. **Aerolínea**: Independiente.
2. **Aeropuerto**: Independiente.
3. **Cliente**: Independiente.
4. **Vuelo**: Requiere Aerolínea y Aeropuertos (origen y destino).
5. **Escala**: Requiere Aeropuerto y Vuelo.
6. **Reserva**: Requiere Cliente y Vuelo.