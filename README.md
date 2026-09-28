# GaneAPI - Actividad colaborativa #3

API REST de videojuegos desarrollada con Java y Spring Boot. Esta versión integra persistencia con MySQL, relación entre entidades, consumo de una API externa y mecanismos básicos de observabilidad.

## Tecnologías

- Java 25
- Spring Boot 3.5.5
- Spring Web
- Spring Data JPA / Hibernate
- MySQL
- Spring Boot Actuator
- Micrometer + Prometheus
- Maven

## Modelo

**Genero 1 ---- N Videojuego**

Un género puede tener varios videojuegos y cada videojuego pertenece a un género.

## MySQL

Crear la base de datos:

```sql
CREATE DATABASE ganeapi;
```

No se incluyen contraseñas en el repositorio.

Variables recomendadas:

```text
DB_URL=jdbc:mysql://localhost:3306/ganeapi?useSSL=false&serverTimezone=UTC
DB_USERNAME=root
DB_PASSWORD=tu_contrasena
```

## Ejecución

Desde la carpeta del proyecto:

```bash
mvn spring-boot:run
```

La API queda disponible en:

```text
http://localhost:8080
```

## Endpoints principales

### Géneros

```text
GET    /api/generos
GET    /api/generos/{id}
POST   /api/generos
```

Ejemplo POST:

```json
{
  "nombre": "Acción"
}
```

### Videojuegos

```text
GET    /api/videojuegos
GET    /api/videojuegos/{id}
GET    /api/videojuegos/genero/{generoId}
POST   /api/videojuegos?generoId=1
PUT    /api/videojuegos/{id}?generoId=1
DELETE /api/videojuegos/{id}
```

Ejemplo POST:

```json
{
  "titulo": "The Legend of Zelda",
  "descripcion": "Videojuego de aventura.",
  "anioLanzamiento": 1986
}
```

## API externa

Se utiliza **PokeAPI** como servicio público externo.

Endpoint interno:

```text
GET /api/videojuegos/externa/pokemon/pikachu
```

La aplicación realiza la solicitud con `RestClient`, procesa la respuesta JSON y la devuelve al cliente.

Si el servicio externo presenta un error, la aplicación registra el evento y responde con HTTP 503.

## Observabilidad

Actuator:

```text
GET /actuator/health
GET /actuator/metrics
GET /actuator/prometheus
```

Métrica personalizada:

```text
ganeapi.videojuegos.consultas
```

La métrica se incrementa al consultar videojuegos.

Logs:

- INFO para consultas, creación y actualización.
- WARN para eliminaciones.
- ERROR para fallos de consumo de la API externa.

Trazabilidad:

Cada solicitud recibe un encabezado:

```text
X-Request-ID
```

Además, se incluye el identificador en los logs mediante MDC.

Indicador de salud personalizado:

```text
ganeapiHealth
```

## Pruebas para el video

1. Mostrar el proyecto y paquetes.
2. Mostrar las entidades `Genero` y `Videojuego`.
3. Mostrar la relación `@OneToMany` / `@ManyToOne`.
4. Mostrar la base de datos `ganeapi` en MySQL.
5. Crear un género con POST.
6. Crear un videojuego asociado al género.
7. Consultar los videojuegos.
8. Consultar `/api/videojuegos/externa/pokemon/pikachu`.
9. Probar un nombre inexistente para evidenciar el manejo de error.
10. Mostrar `/actuator/health`.
11. Mostrar `/actuator/metrics`.
12. Buscar la métrica `ganeapi.videojuegos.consultas`.
13. Mostrar `/actuator/prometheus`.
14. Mostrar la consola con los logs y el `X-Request-ID`.
15. Mostrar el repositorio GitHub y README.

## Declaración de IA para el video

Se utilizó inteligencia artificial como herramienta de apoyo durante el desarrollo para comprender configuraciones de Spring Boot, analizar errores, revisar código y estructurar algunos componentes de observabilidad. Las sugerencias fueron revisadas, adaptadas y probadas en el proyecto. La solución final fue verificada mediante la ejecución de la API, las operaciones CRUD, el consumo del servicio externo y los endpoints de Actuator.

## Importante

No publicar contraseñas, tokens ni credenciales en GitHub.
