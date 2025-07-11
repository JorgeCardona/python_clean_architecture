# python_clean_architecture
This project is a base template for building Python microservices using Clean Architecture. It isolates business logic from external tech (frameworks, databases, APIs), enabling a decoupled, testable, and maintainable system.

![clean_architecture](https://raw.githubusercontent.com/JorgeCardona/python_microservice_scaffolding_clean_architecture/refs/heads/master/resources/clean_architecture.jpg)

# 🛰️ SCAFFOLDING FOR CLEAN ARCHITECTURE IN MICROSERVICES
```
📦 jorge_cardona_project [project_directory]
┣ 🧩 application [package]                     ← Lógica principal y orquestación de la app
┃ ┣ 🛠️ configuration                           ← Configuración general (DB, CORS, logs, entorno)
┃ ┣ 🧰 utils                                   ← Funciones auxiliares reutilizables
┃ ┣ 🧠 domain [package]                        ← Núcleo del negocio (arquitectura limpia)
┃ ┃ ┣ 🧱 entities [package]                    ← Define las estructuras de dominio: modelos de datos y sus esquemas de validación
┃ ┃ ┃ ┣ 🐍 models [package]                    ← Clases con `@dataclass` que representan directamente las tablas de la base de datos. Sin validación ni lógica externa.
┃ ┃ ┃ ┣ ➿ schemas [package]                   ← Esquemas `pydantic` que validan entrada y salida en la API. Sirven de interfaz entre cliente y dominio.
┃ ┃ ┣ 📜 interfaces [package]                  ← Contratos abstractos del dominio. Define cómo el sistema accede a datos o servicios externos.
┃ ┃ ┃ ┣ 🗃️ repositories [package]              ← Interfaces que definen las operaciones esperadas sobre la base de datos (CRUD, filtros, etc.)
┃ ┃ ┃ ┗ 🌐 business_logic [package]            ← Interfaces para servicios externos como email, autenticación, pagos, etc.
┃ ┃ ┣ 🧭 usecases [package]                    ← Contiene la lógica del negocio. Cada clase implementa un flujo funcional (crear, editar, eliminar, etc.)
┃ ┃ ┣ 🎛️ services [package]                    ← Implementaciones reales de interfaces: repositorios y lógica externa concretos (ej. SQL, SendGrid, etc.)
┃ ┣ 🚀 main.py [__main__]                      ← Punto de entrada de la app. Configura la API, inyecta dependencias y expone los endpoints
┣ 🧱 infraestructure [directory]               ← Archivos para empaquetar y desplegar el microservicio (Dockerfile, Kubernetes manifests, Terraform, etc.)
┃ ┣ 🎰 terraform  [directory]                  ← Punto de entrada de la app. Configura la API, inyecta dependencias y expone los endpoints
┣ 🔩 pipelines [directory]                     ← Automatización de Integración y Despliegue Continuo (CI/CD)
┃ ┣ 🧬 templates [directory]                   ← Plantillas reutilizables para configuración de entornos y despliegue.
┣ 📈 logs [directory]                          ← Carpeta donde se generan y almacenan archivos `.log` de ejecución (ej. app.log, db.log)
┣ 📊 reports [directory]                       ← Reportes generados automáticamente (ej. cobertura de tests, análisis estático, etc.)
┣ 📦 requirements [directory]                  ← Archivos de dependencias por entorno (`base.txt`, `dev.txt`, `test.txt`, etc.)
┣ 🧪 test [directory]                          ← Pruebas del sistema organizadas por tipo
┃ ┃ ┣ 📫 api_requests [directory]              ← Peticiones manuales o simuladas de clientes (Postman, REST Client, Thunder)
┃ ┃ ┣ 🎯 end-to-end [directory]                ← Pruebas E2E completas simulando flujos reales desde el cliente hasta el backend
┃ ┃ ┣ 🧪 mock [directory]                      ← Implementaciones simuladas (falsas) de interfaces para testear sin dependencias reales
┃ ┃ ┣ ⚡ performance [directory]               ← Scripts para pruebas de carga, estrés y concurrencia (Locust, JMeter, etc.)
┃ ┃ ┣ 📞 services [directory]                  ← Pruebas unitarias y funcionales de los endpoints HTTP definidos en `services/`
┃ ┃ ┣ 🧩 usecases [directory]                  ← Pruebas unitarias de lógica de negocio. Verifican reglas internas y uso correcto de interfaces
┣ 📘 README.md                                 ← Documentación principal del proyecto
┗ ⚠️ .gitignore                                ← Define qué archivos o carpetas deben ser ignorados por el control de versiones ← Ignora carpetas como `logs/`, 
```

# Arquitecturas
Las arquitecturas de software definen cómo estructurar las aplicaciones, no solo en términos de organización de directorios, sino también en la forma en que se diseña y organiza el código. Esto incluye decidir qué componentes van en qué capas, cómo se dividen las responsabilidades, y qué elementos deben comunicarse entre sí para lograr una solución coherente, escalable y mantenible.

# 🛸 PROJECT PACKAGES STRUCTURE
Este proyecto está diseñado para mantener una estructura modular, escalable y fácil de mantener.  
Cada directorio cumple una función específica dentro de la arquitectura, asegurando **separación de responsabilidades**, **alta cohesión** y **bajo acoplamiento**.

📦 jorge_cardona_project — Estructura completa

<details><summary>🧩 <strong>application</strong>/ — Núcleo del microservicio: orquestación, configuración, dominio y servicios</summary>

<details><summary>🎄 main.py [__main__]</summary>
<pre>🐍 main.py — Punto de entrada que lanza FastAPI y configura dependencias</pre>
</details>

<details><summary>🏁 __init__.py</summary>
<pre>🐍 __init__.py — Permite tratar `application` como un paquete importable</pre>
</details>

<details><summary>🛠️ <strong>configuration</strong>/ — Configuraciones modulares para cada contexto (API, DB, logging...)</summary>
<pre>
🐍 __init__.py — Expone todas las subconfiguraciones
📄.env — Variables de entorno privadas: credenciales, configuración por entorno y claves de API. No debe subirse al repositorio.
  
📂 rest/
  ┣ 🐍 __init__.py
  ┗ 🏩 app_configuration.py — Configuración general de la API
📂 environment/
  ┣ 🐍 __init__.py
  ┗ 📡 environment_configuration.py — Variables de entorno y configuración dinámica
📂 database/
  ┣ 🐍 __init__.py
  ┗ 🔑 database_configuration.py — URI de conexión, ORM, pool de sesiones
📂 log/
  ┣ 🐍 __init__.py
  ┗ 📜 log_configuration.py — Formato, handlers y nivel de logs
📂 cors/
  ┣ 🐍 __init__.py
  ┗ 🚧 cors_configuration.py — Reglas de CORS (Cross-Origin Resource Sharing)
📂 swagger/
  ┣ 🐍 __init__.py
  ┗ 📪 swagger_configuration.py — Metadatos y visualización Swagger UI
</pre>
</details>

<details><summary>🧰 <strong>utils</strong>/ — Funciones auxiliares reutilizables sin lógica de negocio</summary>
<pre>
🐍 __init__.py
🐍 script.py — Funciones genéricas: fechas, formateos, validaciones
🎰 file.yaml — Ejemplo de configuración externa
🖼️ image.jpg — Archivo de recurso (evitar en repositorio)
</pre>
</details>

<details><summary>🧠 <strong>domain</strong>/ — Capa central del negocio (arquitectura limpia)</summary>

<details><summary>🧱 <strong>entities</strong>/ — Modelos de dominio</summary>

<details><summary>📂 <strong>models</strong>/ — Representan entidades de base de datos</summary>
<pre>
🐍 __init__.py
🐍 model_for_entity_ONE.py — Estructura @dataclass equivalente a tabla ONE
🐍 model_for_entity_TWO.py
🐍 model_for_entity_N.py
</pre>
</details>

<details><summary>📂 <strong>schemas</strong>/ — Validación de datos para entrada/salida</summary>
<pre>
🐍 __init__.py
💦 schema_for_entity_ONE.py — Pydantic: validación y serialización de ONE
💦 schema_for_entity_TWO.py
💦 schema_for_entity_N.py
</pre>
</details>
</details>

<details><summary>📜 <strong>interfaces</strong>/ — Contratos abstractos para repositorios y servicios externos</summary>

<details><summary>🗃️ <strong>repositories</strong>/</summary>
<pre>
🐍 __init__.py
🐟 database_method_model_Entity_ONE.py — Métodos abstractos: buscar, guardar...
🐟 database_method_model_Entity_TWO.py
🐟 database_method_model_Entity_N.py
</pre>
</details>

<details><summary>🌐 <strong>business_logic</strong>/</summary>
<pre>
🐍 __init__.py
🐦 business_logic_method_model_Entity_ONE.py — Por ejemplo: EmailSender, TokenProvider
🐦 business_logic_method_model_Entity_TWO.py
🐦 business_logic_method_model_Entity_N.py
</pre>
</details>
</details>

<details><summary>🧭 <strong>usecases</strong>/ — Casos de uso que orquestan entidades e interfaces</summary>
<pre>
🐍 __init__.py
🎎 use_case_implementation_business_repository_logic_model_ONE.py
🎎 use_case_implementation_business_repository_logic_model_TWO.py
🎎 use_case_implementation_business_repository_logic_model_N.py
</pre>
</details>

<details><summary>🎛️ <strong>services</strong>/ — Implementaciones conectadas a la infraestructura real</summary>
<pre>
🐍 __init__.py
✈️ services_use_case_implementation_model_ONE.py
✈️ services_use_case_implementation_model_TWO.py
✈️ services_use_case_implementation_model_N.py
</pre>
</details>
</details>
</details>

<details><summary>📦 <strong>infrastructure</strong>/ — Scripts de empaquetado y despliegue (Docker, K8s, Terraform)</summary>
<pre>
🐍 __init__.py
🐳 Dockerfile — Imagen base y comandos de construcción
🎰 Manifest.yaml — Kubernetes manifest para despliegue
📂 terraform/
  🪵 main.tf — Infraestructura declarativa
  📄 variables.tf — Variables reutilizables
  📄 outputs.tf — Resultados del `apply`
  📄 terraform.tfvars — Parámetros concretos por entorno
</pre>
</details>

<details><summary>📈 <strong>logs</strong>/ — Archivos de auditoría</summary>
<pre>
📑 app.log — Logs generales de ejecución
📑 db.log — Logs de operaciones de base de datos
archive/
  ┗ 📑 app-2024-01-01.log — Logs antiguos archivados
</pre>
</details>

<details>
<summary>🔁 <strong>pipelines/</strong> — Automatización de Integración y Despliegue Continuo (CI/CD)</summary>

Contiene los archivos que definen y orquestan la ejecución automatizada de pruebas, análisis de calidad y despliegues del microservicio en entornos integrados (GitHub, GitLab, etc).

<details>
<summary>🤖 <strong>github-actions.yaml</strong></summary>
<pre>
🤖 github-actions.yaml — Workflow para GitHub Actions:
- 🔧 Instalación de dependencias
- 🧹 Linter y análisis estático
- 🧪 Ejecución de tests
- 🚀 Despliegue automático
</pre>
</details>

<details>
<summary>🐙 <strong>gitlab-ci.yml</strong></summary>
<pre>
🐙 gitlab-ci.yml — Alternativa para definir stages y jobs en GitLab CI/CD:
- 🏗️ build
- 🧪 test
- 📦 deploy
</pre>
</details>

<details>
<summary>🧭 <strong>sonar-project.properties</strong></summary>
<pre>
🧭 sonar-project.properties — Configuración para análisis estático con SonarQube.
Incluye:
- projectKey
- rutas de origen (source dirs)
- exclusiones y reglas de calidad
</pre>
</details>

<details>
<summary>📂 <strong>templates/</strong> — Workflows reutilizables por otros pipelines</summary>
<pre>
📋 templates/
┣ 🐳 docker_build.yml         — Construcción de imagen Docker
┣ 🧪 pytest_run.yml           — Pruebas automáticas con pytest
┣ 📣 notify_slack.yml         — Notificaciones de eventos a canales de Slack
</pre>
</details>

</details>

<details><summary>📊 <strong>reports</strong>/ — Reportes de calidad y cobertura</summary>
<pre>
📄 coverage.html — Cobertura de tests
📜 htmlcov/main_py.html — Archivos HTML generados por coverage.py
</pre>
</details>

<details><summary>📦 <strong>requirements</strong>/ — Dependencias por entorno</summary>
<pre>
🐍 __init__.py
📄 base.txt — Paquetes comunes
📄 dev.txt — Herramientas de desarrollo
📄 test.txt — Librerías de testing
📄 requirements.txt — Lista combinada
</pre>
</details>

<details><summary>🧪 <strong>test</strong>/ — Tests organizados por responsabilidad</summary>

<details><summary>📫 <strong>api_requests</strong>/</summary>
<pre>
📄 postman_productos_collection.json — Colección de pruebas manuales
📄 usuarios.rest — Peticiones REST Client VSCode
📘 README.md — Instrucciones de uso
</pre>
</details>

<details><summary>🎯 <strong>end-to-end</strong>/</summary>
<pre>
🐍 __init__.py
🐍 test_e2e_crear_producto.py — Valida flujo de creación completo
🐍 test_e2e_editar_producto.py — Valida edición
</pre>
</details>

<details><summary>🧪 <strong>mock</strong>/</summary>
<pre>
🐍 __init__.py
🐍 mock_producto_repository.py — Simulación de DB
🐍 mock_email_sender.py — Simulación de servicio externo
</pre>
</details>

<details><summary>⚡ <strong>performance</strong>/</summary>
<pre>
🐍 __init__.py
🐍 locustfile.py — Pruebas de carga
🐍 test_load_login_users.py — Concurrencia simulada
</pre>
</details>

<details><summary>📞 <strong>services</strong>/</summary>
<pre>
🐍 __init__.py
🐍 test_endpoint_crear_producto.py
🐍 test_endpoint_obtener_producto.py
</pre>
</details>

<details><summary>🧩 <strong>usecases</strong>/</summary>
<pre>
🐍 __init__.py
🐍 test_crear_producto_usecase.py
🐍 test_eliminar_producto_usecase.py
</pre>
</details>

</details>

<details><summary>📘 README.md</summary>
<pre>📘 README.md — Documentación general del proyecto</pre>
</details>

<details><summary>⚠️ .gitignore</summary>
<pre>⚠️ .gitignore — Archivos ignorados por Git (logs/, .env, __pycache__...)</pre>
</details>

# Application
Este directorio encapsula todo el código fuente del microservicio. Implementa la arquitectura limpia dividiendo la aplicación en capas bien definidas: `configuration`, `logging`, `utils`, `domain`, y el punto de entrada (`main.py`).

| Parte del sistema                     | ¿Qué hace?                                                                                                                                                             |
| ------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **`entities/model_for_entity_*.py`**  | ✅ Define la **estructura interna** de los datos. Son clases con `@dataclass` que representan tablas reales de la base de datos. Sin lógica ni validaciones.            |
| **`entities/schema_for_entity_*.py`** | ✅ Define los **esquemas de entrada y salida** que usa la API. Escritos con `pydantic`, validan datos de entrada y formatean la salida al cliente.                      |
| **`interfaces/`**                     | ✅ Declara **contratos abstractos** que el dominio necesita para acceder a datos (`repositories/`) o servicios externos (`business_logic/`). No contienen lógica.       |
| **`interfaces/repositories/`**        | ✅ Define interfaces para persistencia: `guardar()`, `buscar_por_id()`, `actualizar()`, etc. Sólo firmas de métodos. Sin implementación.                                |
| **`interfaces/business_logic/`**      | ✅ Define interfaces para lógica externa (email, autenticación, notificaciones, pagos, etc.). Declaración de métodos que otros deben implementar.                       |
| **`usecases/`**                       | ✅ Contiene la **lógica del negocio puro**. Cada clase representa un flujo funcional (crear, editar, eliminar) y **usa las interfaces** del dominio.                    |
|                                       | 🧠 Aquí también se **implementan los métodos definidos por las interfaces** y se escribe la lógica propia del negocio:                                                 |
|                                       | filtros, transformaciones, decisiones, validaciones del dominio, cálculos, restricciones de uso. No contiene lógica técnica ni externa.                                |
| **`services/`**                       | ✅ Define los **endpoints HTTP reales**. Aquí se reciben las peticiones, se validan usando schemas, se llama a los `usecases`, y se retorna la respuesta al cliente.    |
| **`main.py`**                         | ✅ Punto de entrada. Registra los endpoints desde `services/`, configura la aplicación y **conecta los usecases con sus dependencias reales**.                          |
| **`configuration/`**                  | ✅ Agrupa todas las configuraciones del sistema. Conexión a base de datos, variables de entorno, logging, CORS, Swagger, etc. Organizado en submódulos por contexto.    |
| **`utils/`**                          | ✅ Contiene funciones auxiliares reutilizables. Ej: manejo de fechas, normalización de strings, generadores de IDs, helpers sin lógica de negocio ni infraestructura.   |
| **`deployment/`**                     | ✅ Archivos necesarios para empaquetar y desplegar el microservicio. Incluye Dockerfile, manifest de Kubernetes, scripts, etc.                                          |
| **`requirements/`**                   | ✅ Lista de dependencias del proyecto divididas por entorno (`base.txt`, `dev.txt`, `test.txt`, `requirements.txt`). Para entornos reproducibles y mantenibles.         |
| **`test/`**                           | ✅ Contiene pruebas unitarias y de integración. Organizado por `usecases` y `services`. Usa mocks de interfaces para evitar conectarse a la base real en pruebas.       |
| **`logs/`**                           | ✅ Carpeta donde se escriben los archivos `.log` generados por la ejecución de la app. No contiene código ni se versiona. Ignorada por `.gitignore`.                    |
| **`reports/`**                        | ✅ Reportes generados automáticamente (ej. cobertura de código con `coverage.html`, análisis de calidad, performance, etc.). Carpeta ignorada del control de versiones. |
