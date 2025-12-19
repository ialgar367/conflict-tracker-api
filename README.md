## Descripción del Proyecto

Conflict Tracker es una aplicación backend que proporciona una API REST completa para gestionar información sobre conflictos bélicos, facciones, países involucrados y eventos clave. El proyecto implementa una arquitectura por capas bien definida, utilizando Spring Boot, JPA/Hibernate y una base de datos relacional.

## Características Principales

- ✅ API REST completa con operaciones CRUD
- ✅ Modelo de datos relacional con JPA/Hibernate
- ✅ Arquitectura por capas (Controller - Service - Repository)
- ✅ DTOs para desacoplar el modelo de datos de la API
- ✅ Gestión de relaciones Many-to-Many y One-to-Many
- ✅ Validación de datos con Bean Validation
- ✅ Manejo global de excepciones
- ✅ Base de datos H2 en memoria (desarrollo)
- ✅ Soporte para PostgreSQL (producción)
- ✅ Frontend testimonial para pruebas

## Tecnologías Utilizadas

- **Java 17**
- **Spring Boot 3.2.0**
- **Spring Data JPA**
- **H2 Database** (desarrollo)
- **PostgreSQL** (producción)
- **Maven** (gestión de dependencias)
- **Lombok** (reducción de código boilerplate)

## Modelo de Datos

### Entidades

1. **Conflict** (Conflicto)
   - `id`: Long
   - `name`: String
   - `startDate`: LocalDate
   - `status`: Enum (ACTIVE, FROZEN, ENDED)
   - `description`: String
   - Relaciones: ManyToMany con Country, OneToMany con Faction y Event

2. **Faction** (Facción)
   - `id`: Long
   - `name`: String
   - Relaciones: ManyToOne con Conflict, ManyToMany con Country

3. **Country** (País)
   - `id`: Long
   - `name`: String
   - `code`: String

4. **Event** (Evento)
   - `id`: Long
   - `eventDate`: LocalDate
   - `location`: String
   - `description`: String
   - Relaciones: ManyToOne con Conflict

## Instalación y Ejecución

### Prerrequisitos

- Java 17 o superior
- Maven 3.6 o superior

### Pasos para Ejecutar

1. **Clonar el repositorio**
   ```bash
   git clone <url-del-repositorio>
   cd conflict-tracker
   ```

2. **Compilar el proyecto**
   ```bash
   mvn clean install
   ```

3. **Ejecutar la aplicación**
   ```bash
   mvn spring-boot:run
   ```

4. **Acceder a la aplicación**
   - API: http://localhost:8080/api/v1
   - Frontend: http://localhost:8080
   - Consola H2: http://localhost:8080/h2-console
     - JDBC URL: `jdbc:h2:mem:conflictdb`
     - Usuario: `sa`
     - Contraseña: (dejar en blanco)

## Endpoints de la API

### Conflicts

| Método | Endpoint | Descripción |
|--------|----------|-------------|
| GET | `/api/v1/conflicts` | Obtener todos los conflictos |
| GET | `/api/v1/conflicts?status=ACTIVE` | Filtrar conflictos por estado |
| GET | `/api/v1/conflicts/{id}` | Obtener un conflicto por ID |
| POST | `/api/v1/conflicts` | Crear un nuevo conflicto |
| PUT | `/api/v1/conflicts/{id}` | Actualizar un conflicto |
| DELETE | `/api/v1/conflicts/{id}` | Eliminar un conflicto |

### Factions

| Método | Endpoint | Descripción |
|--------|----------|-------------|
| GET | `/api/v1/factions` | Obtener todas las facciones |
| GET | `/api/v1/factions/{id}` | Obtener una facción por ID |
| POST | `/api/v1/factions` | Crear una nueva facción |
| PUT | `/api/v1/factions/{id}` | Actualizar una facción |
| DELETE | `/api/v1/factions/{id}` | Eliminar una facción |

### Events

| Método | Endpoint | Descripción |
|--------|----------|-------------|
| GET | `/api/v1/events` | Obtener todos los eventos |
| GET | `/api/v1/events/{id}` | Obtener un evento por ID |
| POST | `/api/v1/events` | Crear un nuevo evento |
| PUT | `/api/v1/events/{id}` | Actualizar un evento |
| DELETE | `/api/v1/events/{id}` | Eliminar un evento |

### Countries

| Método | Endpoint | Descripción |
|--------|----------|-------------|
| GET | `/api/v1/countries/{code}/conflicts` | Obtener conflictos por código de país |

## Ejemplos de Uso

### Obtener todos los conflictos

```bash
curl -X GET http://localhost:8080/api/v1/conflicts
```

### Filtrar conflictos activos

```bash
curl -X GET "http://localhost:8080/api/v1/conflicts?status=ACTIVE"
```

### Obtener un conflicto específico

```bash
curl -X GET http://localhost:8080/api/v1/conflicts/1
```

### Crear un nuevo conflicto

```bash
curl -X POST http://localhost:8080/api/v1/conflicts \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Nuevo Conflicto",
    "startDate": "2024-01-01",
    "status": "ACTIVE",
    "description": "Descripción del conflicto",
    "countries": []
  }'
```

### Actualizar un conflicto

```bash
curl -X PUT http://localhost:8080/api/v1/conflicts/1 \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Conflicto Actualizado",
    "startDate": "2024-01-01",
    "status": "FROZEN",
    "description": "Nueva descripción",
    "countries": []
  }'
```

### Eliminar un conflicto

```bash
curl -X DELETE http://localhost:8080/api/v1/conflicts/1
```

### Obtener conflictos de un país

```bash
curl -X GET http://localhost:8080/api/v1/countries/UKR/conflicts
```

### Crear una facción

```bash
curl -X POST http://localhost:8080/api/v1/factions \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Nueva Facción",
    "conflictId": 1,
    "supportingCountries": []
  }'
```

### Crear un evento

```bash
curl -X POST http://localhost:8080/api/v1/events \
  -H "Content-Type: application/json" \
  -d '{
    "eventDate": "2024-01-15",
    "location": "Ciudad Ejemplo",
    "description": "Descripción del evento",
    "conflictId": 1
  }'
```

## Configuración de PostgreSQL

Para usar PostgreSQL en lugar de H2:

1. **Crear la base de datos**
   ```sql
   CREATE DATABASE conflictdb;
   ```

2. **Modificar application.properties**
   
   Comentar la configuración de H2 y descomentar la de PostgreSQL:
   
   ```properties
   spring.datasource.url=jdbc:postgresql://localhost:5432/conflictdb
   spring.datasource.username=postgres
   spring.datasource.password=yourpassword
   spring.jpa.database-platform=org.hibernate.dialect.PostgreSQLDialect
   spring.jpa.hibernate.ddl-auto=update
   spring.h2.console.enabled=false
   ```

## Arquitectura del Proyecto

```
src/main/java/com/conflicttracker/
├── controller/          # Controladores REST
├── service/            # Lógica de negocio
├── repository/         # Repositorios JPA
├── model/              # Entidades JPA
├── dto/                # Data Transfer Objects
├── mapper/             # Conversores Entity <-> DTO
├── exception/          # Manejo de excepciones
└── ConflictTrackerApiApplication.java
```

## Datos de Prueba

La aplicación incluye datos de prueba que se cargan automáticamente al iniciar:

- 10 países
- 4 conflictos principales (Rusia-Ucrania, Israel-Palestina, Siria, Yemen)
- 8 facciones
- 9 eventos históricos

## Estructura de DTOs

Los DTOs implementados garantizan el desacoplamiento entre el modelo de datos y la API:

- `ConflictDTO`: Para operaciones completas con conflictos
- `ConflictSummaryDTO`: Para listados resumidos
- `FactionDTO`: Para gestión de facciones
- `EventDTO`: Para eventos
- `CountryDTO`: Para países

## Validaciones

Se aplican validaciones mediante anotaciones de Bean Validation:

- `@NotBlank`: Campos de texto requeridos
- `@NotNull`: Campos obligatorios
- Mensajes de error personalizados

## Manejo de Errores

La aplicación incluye un manejador global de excepciones que devuelve respuestas JSON estructuradas:

- `ResourceNotFoundException`: Recurso no encontrado (404)
- `MethodArgumentNotValidException`: Error de validación (400)
- Excepciones generales (500)