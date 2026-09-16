# Mi proyecto de FASTAPI - Elias Gomez 9A

README.md REALIZADO COMPLETAMENTE CON IA

Proyecto realizado para la materia **Desarrollo para dispositivos inteligentes** de la Universidad Tecnológica de Tijuana.

El proyecto consiste en una API desarrollada con **FastAPI**, utilizando Python, SQLModel, PostgreSQL mediante Docker y SQLite como alternativa. La API permite realizar operaciones CRUD sobre usuarios, validar los datos recibidos y proteger las contraseñas mediante hasheo.

---

# 1. Instalación y ejecución del proyecto

Para ejecutar el proyecto en otra computadora es necesario seguir los siguientes pasos.

## 1.1 Requisitos

Antes de comenzar, es necesario tener instalados:

- [Python](https://www.python.org/)
- [Git](https://git-scm.com/)
- [Docker Desktop](https://www.docker.com/products/docker-desktop/)
- Visual Studio Code u otro editor de código.

Se recomienda tener **Docker Desktop abierto y ejecutándose** antes de iniciar la aplicación.

---

## 1.2 Clonar el repositorio

Primero se debe clonar el repositorio desde GitHub:

```bash
git clone https://github.com/EliasGomezC/-Mi-proyecto-de-FASTAPI-EliasGomez-9A.git
```

Después se debe entrar a la carpeta del proyecto:

```bash
cd -Mi-proyecto-de-FASTAPI-EliasGomez-9A
```

---

## 1.3 Crear el entorno virtual

Se recomienda crear un entorno virtual para mantener separadas las dependencias del proyecto.

En Windows:

```bash
python -m venv venv
```

Después se activa con:

```bash
venv\Scripts\activate
```

Si se activó correctamente, la terminal mostrará algo parecido a:

```text
(venv) C:\ruta\del\proyecto>
```

---

## 1.4 Instalar las dependencias

El proyecto cuenta con un archivo `requirements.txt`, donde se encuentran las dependencias necesarias.

Para instalarlas se utiliza:

```bash
pip install -r requirements.txt
```

De esta manera se instalan todas las librerías necesarias para ejecutar el proyecto.

---

# 2. Configurar la base de datos

El proyecto puede utilizar **PostgreSQL mediante Docker** y también cuenta con **SQLite como alternativa**.

El archivo encargado de realizar la conexión con la base de datos es:

```text
db/database.py
```

Este archivo intenta utilizar la variable de entorno:

```text
DATABASE_URL
```

Si no se encuentra configurada, utiliza SQLite como alternativa.

---

## 2.1 Ejecutar PostgreSQL con Docker

El proyecto cuenta con el archivo:

```text
docker-compose.yml
```

Este archivo contiene la configuración necesaria para ejecutar PostgreSQL mediante Docker.

Para iniciar el contenedor se utiliza:

```bash
docker compose up -d
```

Para comprobar que el contenedor está ejecutándose:

```bash
docker ps
```

El proyecto utiliza un contenedor de PostgreSQL configurado en `docker-compose.yml`.

---

## 2.2 Configurar `DATABASE_URL`

Si se desea utilizar PostgreSQL, se puede crear un archivo `.env` en la carpeta principal del proyecto.

Ejemplo:

```env
DATABASE_URL=postgresql://mike:super_secret_password@localhost:5432/mikedb
```

La variable `DATABASE_URL` permite indicarle a la aplicación dónde se encuentra la base de datos.

Si esta variable no está configurada, `database.py` utiliza SQLite como alternativa:

```text
sqlite:///./mikedb.db
```

---

# 3. Ejecutar FastAPI

Después de activar el entorno virtual y configurar la base de datos, se puede iniciar la aplicación.

El comando utilizado para ejecutar el proyecto durante el desarrollo es:

```bash
fastapi dev main.py
```

Una vez iniciado correctamente, la aplicación estará disponible en:

```text
http://127.0.0.1:8000
```

También se puede acceder mediante:

```text
http://localhost:8000
```

---

# 4. Documentación de la API

FastAPI genera automáticamente una documentación interactiva para probar los endpoints.

Para acceder a ella se debe abrir:

```text
http://127.0.0.1:8000/docs
```

Desde esta página se pueden consultar y probar los diferentes endpoints disponibles en el proyecto.

También existe el endpoint principal:

```text
http://127.0.0.1:8000/
```

y el endpoint de comprobación:

```text
http://127.0.0.1:8000/health
```

El endpoint `/health` permite comprobar que la aplicación se encuentra funcionando correctamente.

---

# 5. Estructura del proyecto

La estructura principal del proyecto es:

```text
-Mi-proyecto-de-FASTAPI-EliasGomez-9A/
│
├── api/
│   └── v1/
│       └── user_api.py
│
├── core/
│   ├── cors.py
│   └── security.py
│
├── db/
│   └── database.py
│
├── models/
│   └── user_model.py
│
├── schemas/
│   └── user_schemas.py
│
├── .gitignore
├── docker-compose.yml
├── main.py
├── requirements.txt
└── README.md
```

Cada carpeta tiene una responsabilidad diferente dentro del proyecto.

---

# 6. Archivo `main.py`

`main.py` es el archivo principal de la aplicación.

Se encarga de crear y configurar la aplicación FastAPI y conectar las diferentes partes del proyecto.

Entre sus principales funciones se encuentran:

- Crear la aplicación FastAPI.
- Inicializar las tablas de la base de datos.
- Configurar CORS.
- Registrar las rutas de usuarios.
- Crear el endpoint `/`.
- Crear el endpoint `/health`.

Al iniciar la aplicación se ejecuta la creación de las tablas mediante:

```python
create_db_and_tables()
```

Después se configura CORS mediante:

```python
setup_cors(app)
```

Finalmente se registran las rutas de usuarios.

Las rutas de usuarios utilizan el prefijo:

```text
/api/v1/users
```

Por lo tanto, `main.py` funciona como el punto principal que conecta los diferentes componentes del sistema.

---

# 7. Carpeta `api`

La carpeta `api` contiene los endpoints o rutas que permiten que otras aplicaciones se comuniquen con la API.

Su estructura es:

```text
api/
└── v1/
    └── user_api.py
```

La carpeta `v1` representa la versión actual de la API.

Separar las rutas por versiones permite mantener una estructura organizada y facilita agregar futuras versiones de la API.

---

# 8. Carpeta `api/v1`

La carpeta `v1` contiene los endpoints correspondientes a la primera versión de la API.

Actualmente contiene:

```text
user_api.py
```

Este archivo contiene las rutas relacionadas con los usuarios.

---

# 9. Archivo `api/v1/user_api.py`

`user_api.py` contiene los endpoints utilizados para realizar las operaciones sobre los usuarios.

En este archivo se implementan las operaciones CRUD:

- Crear usuarios.
- Consultar usuarios.
- Consultar un usuario específico.
- Actualizar usuarios.
- Eliminar usuarios.

Las principales rutas son:

```text
POST   /api/v1/users/
GET    /api/v1/users/
GET    /api/v1/users/{user_id}
PUT    /api/v1/users/{user_id}
DELETE /api/v1/users/{user_id}
```

---

## 9.1 Crear usuario

Para crear un usuario se utiliza:

```http
POST /api/v1/users/
```

El endpoint recibe los datos del usuario y primero valida la información.

Después comprueba que el nombre de usuario y el correo electrónico no estén registrados.

Posteriormente, la contraseña se transforma mediante una función de hasheo antes de almacenarse en la base de datos.

---

## 9.2 Consultar usuarios

Para obtener todos los usuarios se utiliza:

```http
GET /api/v1/users/
```

Este endpoint consulta los usuarios registrados en la base de datos.

---

## 9.3 Consultar un usuario

Para obtener un usuario específico se utiliza:

```http
GET /api/v1/users/{user_id}
```

Por ejemplo:

```text
GET /api/v1/users/1
```

---

## 9.4 Actualizar un usuario

Para actualizar un usuario se utiliza:

```http
PUT /api/v1/users/{user_id}
```

Este endpoint permite modificar la información de un usuario.

Si se modifica la contraseña, esta vuelve a pasar por el proceso de hasheo antes de guardarse.

---

## 9.5 Eliminar un usuario

Para eliminar un usuario se utiliza:

```http
DELETE /api/v1/users/{user_id}
```

Este endpoint elimina el usuario indicado mediante su ID.

---

# 10. Carpeta `core`

La carpeta `core` contiene funcionalidades relacionadas con la configuración y seguridad de la aplicación.

Actualmente contiene:

```text
core/
├── cors.py
└── security.py
```

---

# 11. Archivo `core/cors.py`

El archivo `cors.py` se encarga de configurar los permisos **CORS** de la aplicación FastAPI.

CORS permite controlar qué aplicaciones externas pueden realizar peticiones a la API.

En este proyecto se utiliza:

```python
allow_origins=origins
```

y por defecto:

```python
origins = ["*"]
```

Esto significa que durante el desarrollo se permite el acceso desde cualquier origen.

También se permiten todos los métodos HTTP:

```python
allow_methods=["*"]
```

y todos los encabezados:

```python
allow_headers=["*"]
```

En pocas palabras, `cors.py` permite configurar los permisos necesarios para que un frontend u otra aplicación pueda comunicarse con la API.

---

# 12. Archivo `core/security.py`

El archivo `security.py` se encarga de las funciones relacionadas con la seguridad de las contraseñas.

El proyecto utiliza **Argon2** para realizar el hasheo de las contraseñas.

Este archivo contiene principalmente dos funciones:

```text
get_password_hash()
verify_password()
```

---

## 12.1 `get_password_hash()`

Esta función recibe una contraseña y genera un hash.

Ejemplo:

```python
hashed_password = get_password_hash(password)
```

La contraseña original no se almacena directamente en la base de datos.

En su lugar, se guarda el resultado del hasheo.

---

## 12.2 `verify_password()`

Esta función permite comprobar si una contraseña coincide con un hash almacenado.

De esta forma se puede comprobar una contraseña sin tener que guardar la contraseña original.

---

# 13. Carpeta `db`

La carpeta `db` contiene los elementos relacionados con la conexión a la base de datos.

Actualmente contiene:

```text
db/
└── database.py
```

---

# 14. Archivo `db/database.py`

El archivo `database.py` se encarga de establecer la conexión con la base de datos.

Primero carga las variables de entorno y busca:

```text
DATABASE_URL
```

Si existe esta variable, se utiliza para realizar la conexión.

Si no existe, se utiliza SQLite como alternativa:

```text
sqlite:///./mikedb.db
```

El archivo también crea el motor de conexión utilizando SQLModel.

Además contiene la función:

```python
create_db_and_tables()
```

Esta función se utiliza para crear las tablas necesarias cuando se inicia la aplicación.

También contiene:

```python
get_session()
```

que proporciona una sesión para realizar las operaciones sobre la base de datos.

---

# 15. Carpeta `models`

La carpeta `models` contiene los modelos que representan las tablas de la base de datos.

Actualmente contiene:

```text
models/
└── user_model.py
```

---

# 16. Archivo `models/user_model.py`

El archivo `user_model.py` contiene la estructura que tendrá la tabla de usuarios.

El modelo principal es:

```python
class User(SQLModel, table=True):
```

El modelo contiene información como:

| Campo | Descripción |
|---|---|
| `id` | Identificador del usuario |
| `username` | Nombre de usuario |
| `email` | Correo electrónico |
| `hashed_password` | Contraseña almacenada como hash |
| `is_active` | Indica si el usuario está activo |
| `created_at` | Fecha de creación |

El campo `id` funciona como identificador del usuario.

El nombre de usuario y el correo electrónico cuentan con restricciones para evitar registros duplicados.

La contraseña se guarda en:

```text
hashed_password
```

por lo que no se almacena directamente como texto plano.

---

# 17. Carpeta `schemas`

La carpeta `schemas` contiene los esquemas utilizados para validar los datos que entran y salen de la API.

Actualmente contiene:

```text
schemas/
└── user_schemas.py
```

---

# 18. Archivo `schemas/user_schemas.py`

El archivo `user_schemas.py` utiliza **Pydantic** para definir y validar la información relacionada con los usuarios.

Su función principal es establecer cómo deben ser los datos antes de que sean procesados por la API.

Actualmente contiene esquemas como:

```text
UserCreate
UserUpdate
UserResponse
```

---

## 18.1 `UserCreate`

`UserCreate` se utiliza para validar los datos necesarios para crear un usuario.

Los principales campos son:

```text
username
email
password
```

El nombre de usuario tiene restricciones de longitud.

El correo electrónico debe tener un formato válido.

La contraseña también cuenta con restricciones de longitud.

Esto permite detectar datos incorrectos antes de intentar guardarlos en la base de datos.

---

## 18.2 `UserUpdate`

`UserUpdate` se utiliza para actualizar la información de un usuario.

Sus campos pueden ser opcionales, permitiendo modificar únicamente la información necesaria.

Entre los campos se encuentran:

```text
username
email
password
is_active
```

Si se modifica la contraseña, esta vuelve a pasar por el proceso de hasheo.

---

## 18.3 `UserResponse`

`UserResponse` define los datos que se devuelven al cliente después de realizar una operación.

Incluye información como:

```text
id
username
email
is_active
created_at
```

No incluye:

```text
hashed_password
```

Esto evita devolver la contraseña hasheada como parte de la respuesta de la API.

---

# 19. Archivo `docker-compose.yml`

El archivo `docker-compose.yml` contiene la configuración utilizada para ejecutar PostgreSQL mediante Docker.

Su objetivo es facilitar la creación de la base de datos sin tener que instalar PostgreSQL directamente en la computadora.

La configuración incluye un servicio de PostgreSQL y un volumen para conservar la información de la base de datos.

Para iniciar el servicio se utiliza:

```bash
docker compose up -d
```

Para detenerlo:

```bash
docker compose down
```

Para comprobar los contenedores activos:

```bash
docker ps
```

---

# 20. Archivo `requirements.txt`

El archivo `requirements.txt` contiene las librerías de Python utilizadas por el proyecto.

En lugar de instalar cada dependencia manualmente, se puede ejecutar:

```bash
pip install -r requirements.txt
```

Esto permite instalar las dependencias necesarias para ejecutar el proyecto.

---

# 21. Archivo `.gitignore`

El archivo `.gitignore` indica a Git qué archivos y carpetas no deben ser incluidos en el repositorio.

Normalmente se utiliza para evitar subir elementos como:

- Entornos virtuales.
- Archivos temporales.
- Archivos de configuración local.
- Archivos generados automáticamente.
- Archivos con información sensible.

---

# 22. Flujo general del sistema

El funcionamiento general del proyecto puede representarse de la siguiente manera:

```text
                         CLIENTE
                            │
                            ▼
                     FastAPI / main.py
                            │
                            ▼
                  api/v1/user_api.py
                            │
                ┌───────────┴───────────┐
                │                       │
                ▼                       ▼
       schemas/user_schemas.py   core/security.py
                │                       │
                │                    Argon2
                │                       │
                └───────────┬───────────┘
                            ▼
                  models/user_model.py
                            │
                            ▼
                    db/database.py
                            │
                    ┌───────┴───────┐
                    ▼               ▼
                PostgreSQL        SQLite
                   Docker        alternativa
```

---

# 23. Flujo para crear un usuario

Cuando el cliente realiza una petición:

```http
POST /api/v1/users/
```

el proceso general es:

### 1. El cliente envía los datos

Ejemplo:

```json
{
    "username": "Elias",
    "email": "elias@gmail.com",
    "password": "12345678"
}
```

### 2. Se validan los datos

Los datos son revisados mediante el esquema:

```text
UserCreate
```

Se comprueba que los campos tengan el tipo y formato correcto.

### 3. Se verifica que el usuario no exista

La API comprueba que el nombre de usuario y el correo electrónico no estén registrados.

### 4. Se realiza el hasheo

La contraseña pasa por:

```text
core/security.py
```

utilizando Argon2.

### 5. Se crea el modelo

Los datos se convierten en un objeto:

```text
User
```

definido en:

```text
models/user_model.py
```

### 6. Se guarda en la base de datos

La información se guarda utilizando la conexión y sesión proporcionadas por:

```text
db/database.py
```

### 7. Se devuelve la respuesta

Finalmente, la información se devuelve utilizando:

```text
UserResponse
```

sin incluir el campo de la contraseña hasheada.

---

# 24. Prueba de creación de usuarios

Una vez iniciado el proyecto se puede acceder a:

```text
http://127.0.0.1:8000/docs
```

En la documentación de FastAPI se debe buscar:

```text
POST /api/v1/users/
```

Después se selecciona:

```text
Try it out
```

y se introducen los datos del usuario.

Ejemplo:

```json
{
    "username": "Elias",
    "email": "elias@gmail.com",
    "password": "12345678"
}
```

Después de ejecutar la petición, el usuario se guarda en la base de datos y la contraseña se almacena mediante un hash.

Posteriormente se puede utilizar:

```text
GET /api/v1/users/
```

para consultar los usuarios registrados.

---

# 25. Tecnologías utilizadas

El proyecto utiliza las siguientes tecnologías:

- **Python:** lenguaje utilizado para desarrollar la aplicación.
- **FastAPI:** framework utilizado para crear la API.
- **SQLModel:** utilizado para trabajar con los modelos y la base de datos.
- **Pydantic:** utilizado para validar los datos.
- **PostgreSQL:** base de datos utilizada mediante Docker.
- **SQLite:** alternativa para la base de datos local.
- **Docker:** utilizado para ejecutar PostgreSQL mediante un contenedor.
- **Argon2:** utilizado para realizar el hasheo de las contraseñas.
- **Git:** utilizado para el control de versiones.
- **GitHub:** utilizado para almacenar y compartir el proyecto.

---

# 26. Resumen de la estructura

| Carpeta / Archivo | Función |
|---|---|
| `main.py` | Punto principal de la aplicación FastAPI |
| `api/` | Contiene las rutas de la API |
| `api/v1/` | Contiene la versión 1 de la API |
| `user_api.py` | Endpoints CRUD de usuarios |
| `core/` | Configuración y seguridad |
| `cors.py` | Configuración de CORS |
| `security.py` | Hasheo y verificación de contraseñas |
| `db/` | Conexión con la base de datos |
| `database.py` | Configuración de PostgreSQL/SQLite y sesiones |
| `models/` | Modelos de la base de datos |
| `user_model.py` | Estructura de la tabla User |
| `schemas/` | Validación de datos |
| `user_schemas.py` | Esquemas para crear, actualizar y responder usuarios |
| `docker-compose.yml` | Configuración de PostgreSQL con Docker |
| `requirements.txt` | Dependencias del proyecto |
| `.gitignore` | Archivos excluidos del repositorio |
| `README.md` | Documentación del proyecto |

---

# 27. Conclusión

Este proyecto permite comprender cómo estructurar una API utilizando FastAPI y separar las diferentes responsabilidades de la aplicación.

Las rutas, modelos, esquemas, conexión con la base de datos y funciones de seguridad se encuentran organizadas en diferentes carpetas y archivos.

La aplicación permite realizar operaciones CRUD sobre usuarios, validar los datos recibidos mediante Pydantic y proteger las contraseñas mediante hasheo con Argon2.

Además, Docker permite ejecutar PostgreSQL mediante un contenedor, mientras que SQLite funciona como una alternativa para la ejecución local.

Esta estructura facilita el mantenimiento del proyecto y permite agregar nuevas funcionalidades de manera más organizada.
