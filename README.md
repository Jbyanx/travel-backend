# Travel Backend - Sistema de Reservas de Vuelos

## 1. Visión General
Este servicio es una API REST diseñada para la gestión integral de reservas de vuelos. Permite administrar aerolíneas, aeropuertos, vuelos y escalas, proporcionando a los usuarios una plataforma segura para gestionar sus itinerarios de viaje.

El sistema utiliza una arquitectura monolítica basada en Spring Boot, con un enfoque en la integridad de los datos y seguridad de acceso.

---

## 2. Tecnologías y Stack Técnico
| Componente | Detalle |
| :--- | :--- |
| **Framework** | Spring Boot 3.3.3 (Java 17) |
| **Persistencia** | Spring Data JPA / Hibernate |
| **Base de Datos** | PostgreSQL |
| **Seguridad** | Spring Security + JWT (Json Web Token) |
| **Mapeo** | MapStruct & Lombok |
| **Contenedores** | Docker |

---

## 3. Seguridad y Control de Acceso (RBAC)
El sistema implementa seguridad a nivel de método (`@PreAuthorize`) y autenticación basada en JWT, siguiendo el principio de mínimo privilegio.

| Módulo | Endpoint | Funcionalidad | Roles Requeridos |
| :--- | :--- | :--- | :--- |
| **Auth** | `POST /auth/signup` | Registro de nuevos usuarios | Público |
| **Auth** | `POST /auth/login` | Autenticación y obtención de Token | Público |
| **Reservas** | `GET /reservas` | Listar todas las reservas | `ROLE_ADMIN` |
| **Reservas** | `GET /reservas/misreservas` | Ver historial propio | `ROLE_USER` |
| **Reservas** | `POST /reservas` | Crear nueva reserva | Autenticado |
| **Vuelos** | `GET /vuelos` | Consultar disponibilidad | Público / Autenticado |

---

## 4. Configuración y Despliegue

### Requisitos Previos
- Docker y Docker Compose
- Java 17+
- Variable de entorno `JWT_SECRET_KEY` configurada

### Base de Datos
Ejecuta el contenedor de PostgreSQL con el siguiente comando:

```bash
docker run --name postgres-pweb \
  -e POSTGRES_PASSWORD=sebaj \
  -e POSTGRES_USER=jabes \
  -e POSTGRES_DB=travel_db \
  -p 15432:5432 \
  -d postgres
```

### Ejecución del Proyecto
1. Clona el repositorio.
2. Verifica la configuración en `src/main/resources/application.properties`.
3. Inicia la aplicación:
   ```bash
   ./gradlew bootRun
   ```

---

## 5. Guía de Inserción de Datos
Para mantener la integridad referencial, se recomienda seguir este orden al poblar la base de datos:
1. **Aerolíneas** y **Aeropuertos** (Independientes).
2. **Clientes** (Usuarios del sistema).
3. **Vuelos** (Requiere Aerolínea y Aeropuertos).
4. **Escalas** (Requiere Vuelo y Aeropuerto).
5. **Reservas** (Relación entre Cliente y Vuelo).