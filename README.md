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
┣ 🧱 deployment [directory]                    ← Archivos para empaquetar y desplegar el microservicio (Dockerfile, Kubernetes manifests, etc.)
┣ 📂 logs [directory]                          ← Carpeta donde se generan y almacenan archivos `.log` de ejecución (ej. app.log, db.log)
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

# 🛸 PROJECT PACKAGES STRUCTURE
```
📦 jorge_cardona_project [project_directory]
┣ 🧩 application [package]                     ← Lógica principal y orquestación del microservicio
┃ ┣ 🎄 main.py [__main__]                      ← Punto de entrada: inicia la API, configura rutas y dependencias
┃ ┣ 🛠️ configuration [package]                ← Configuraciones separadas por contexto
┃ ┃ ┣ 📄 __init__.py
┃ ┃ ┣ 📂 rest
┃ ┃ ┃ ┣ 📄 __init__.py
┃ ┃ ┃ ┗ 🏩 app_configuration.py               ← Inicializa y configura FastAPI
┃ ┃ ┣ 📂 environment
┃ ┃ ┃ ┣ 📄 __init__.py
┃ ┃ ┃ ┗ 📡 environment_configuration.py       ← Carga de variables de entorno
┃ ┃ ┣ 📂 database
┃ ┃ ┃ ┣ 📄 __init__.py
┃ ┃ ┃ ┗ 🔑 database_configuration.py          ← Configuración de conexión a la base de datos
┃ ┃ ┣ 📂 log
┃ ┃ ┃ ┣ 📄 __init__.py
┃ ┃ ┃ ┗ 📜 log_configuration.py               ← Configuración de logging
┃ ┃ ┣ 📂 cors
┃ ┃ ┃ ┣ 📄 __init__.py
┃ ┃ ┃ ┗ 🚧 cors_configuration.py              ← Configuración de reglas CORS
┃ ┃ ┣ 📂 swagger
┃ ┃ ┃ ┣ 📄 __init__.py
┃ ┃ ┃ ┗ 📪 swagger_configuration.py           ← Configuración de documentación OpenAPI
┃ ┣ 🧰 utils [package]                         ← Funciones auxiliares generales
┃ ┃ ┣ 📄 __init__.py
┃ ┃ ┣ 🐍 script.py                             ← Función de utilidad
┃ ┃ ┣ 🎰 file.yaml                             ← Archivo de ejemplo (config o data)
┃ ┃ ┗ 📜 image.jpg                             ← ⚠️ Evitar archivos binarios en código fuente
┃ ┣ 🧠 domain [package]                        ← Núcleo del dominio del negocio
┃ ┃ ┣ 📄 __init__.py
┃ ┃ ┣ 🧱 entities [package]                   ← Modelos de datos puros y validaciones externas
┃ ┃ ┃ ┣ 📄 __init__.py
┃ ┃ ┃ ┣ 📂 models
┃ ┃ ┃ ┃ ┣ 📄 __init__.py
┃ ┃ ┃ ┃ ┣ 🐍 model_for_entity_ONE.py          ← Modelo de entidad (`@dataclass`)
┃ ┃ ┃ ┃ ┣ 🐍 model_for_entity_TWO.py
┃ ┃ ┃ ┃ ┣ 🐍 model_for_entity_N.py
┃ ┃ ┃ ┣ 📂 schemas
┃ ┃ ┃ ┃ ┣ 📄 __init__.py
┃ ┃ ┃ ┃ ┗ 💦 schema_for_entity_ONE.py         ← Esquema `pydantic` para entrada/salida de API
┃ ┃ ┃ ┃ ┗ 💦 schema_for_entity_TWO.py
┃ ┃ ┃ ┃ ┗ 💦 schema_for_entity_N.py
┃ ┃ ┣ 📜 interfaces [package]                ← Contratos abstractos que define el dominio
┃ ┃ ┃ ┣ 📄 __init__.py
┃ ┃ ┃ ┣ 🗃️ repositories [package]            ← Interfaces de acceso a datos (ej. CRUD)
┃ ┃ ┃ ┃ ┣ 📄 __init__.py
┃ ┃ ┃ ┃ ┣ 🐟 database_method_model_Entity_ONE.py
┃ ┃ ┃ ┃ ┣ 🐟 database_method_model_Entity_TWO.py
┃ ┃ ┃ ┃ ┗ 🐟 database_method_model_Entity_N.py
┃ ┃ ┃ ┗ 🌐 business_logic [package]          ← Interfaces para servicios externos (correo, pagos, etc.)
┃ ┃ ┃ ┃ ┣ 📄 __init__.py
┃ ┃ ┃ ┃ ┣ 🐦 business_logic_method_model_Entity_ONE.py
┃ ┃ ┃ ┃ ┣ 🐦 business_logic_method_model_Entity_TWO.py
┃ ┃ ┃ ┃ ┗ 🐦 business_logic_method_model_Entity_N.py
┃ ┃ ┣ 🧭 usecases [package]                  ← Casos de uso del negocio. Ejecutan la lógica pura
┃ ┃ ┃ ┣ 📄 __init__.py
┃ ┃ ┃ ┣ 🎎 use_case_implementation_business_repository_logic_model_ONE.py
┃ ┃ ┃ ┣ 🎎 use_case_implementation_business_repository_logic_model_TWO.py
┃ ┃ ┃ ┗ 🎎 use_case_implementation_business_repository_logic_model_N.py
┃ ┃ ┣ 🎛️ services [package]                 ← Implementaciones concretas de interfaces (DB, APIs, servicios)
┃ ┃ ┃ ┣ 📄 __init__.py
┃ ┃ ┃ ┣ ✈️ services_use_case_implementation_model_ONE.py
┃ ┃ ┃ ┣ ✈️ services_use_case_implementation_model_TWO.py
┃ ┃ ┃ ┗ ✈️ services_use_case_implementation_model_N.py
┣ 📦 deployment [package]                   ← Archivos necesarios para empaquetar y desplegar el microservicio
┃ ┣ 📄 __init__.py
┃ ┣ 🐳 Dockerfile
┃ ┗ 🎰 Manifest.yaml
┣ 📂 logs [directory]                       ← Carpeta para logs generados durante la ejecución
┃ ┣ 📝 app.log
┃ ┣ 📝 db.log
┃ ┗ 📂 archive/
┃    ┗ 📝 app-2024-01-01.log
┣ 📊 reports [directory]                    ← Reportes generados automáticamente (cobertura, métricas, etc.)
┃ ┣ 📄 coverage.html
┃ ┗ 📂 htmlcov/
┃    ┗ 📜 main_py.html
┣ 📦 requirements [package]                ← Módulo con definiciones de dependencias por entorno
┃ ┣ 📄 __init__.py
┃ ┣ 📄 base.txt
┃ ┣ 📄 dev.txt
┃ ┣ 📄 test.txt
┃ ┗ 📄 requirements.txt
┣ 🧪 test [directory]                        ← Pruebas del sistema organizadas por tipo
┃ ┣ 📄 __init__.py
┃ ┣ 📫 api_requests [directory]             ← Peticiones manuales o simuladas de clientes (Postman, REST Client, Thunder)
┃ ┃ ┣ postman_productos_collection.json
┃ ┃ ┣ usuarios.rest
┃ ┃ ┗ README.md                             ← Instrucciones para importar o ejecutar peticiones
┃ ┣ 🎯 end-to-end [directory]               ← Pruebas E2E completas simulando flujos reales desde el cliente hasta la persistencia
┃ ┃ ┣ 📄 __init__.py
┃ ┃ ┣ test_e2e_crear_producto.py
┃ ┃ ┗ test_e2e_editar_producto.py
┃ ┣ 🧪 mock [directory]                     ← Mocks de repositorios o lógica externa para pruebas unitarias
┃ ┃ ┣ 📄 __init__.py
┃ ┃ ┣ mock_producto_repository.py
┃ ┃ ┗ mock_email_sender.py
┃ ┣ ⚡ performance [directory]              ← Pruebas de carga, estrés y concurrencia
┃ ┃ ┣ locustfile.py                        ← Script de Locust para simular carga sobre `/productos`
┃ ┃ ┗ test_load_login_users.py             ← Simulación de usuarios concurrentes sobre endpoint de login
┃ ┣ 📞 services [directory]                ← Pruebas unitarias y funcionales de los endpoints HTTP definidos en `services/`
┃ ┃ ┣ 📄 __init__.py
┃ ┃ ┣ test_endpoint_crear_producto.py
┃ ┃ ┗ test_endpoint_obtener_producto.py
┃ ┣ 🧩 usecases [directory]                ← Pruebas unitarias de lógica de negocio (casos de uso)
┃ ┃ ┣ 📄 __init__.py
┃ ┃ ┣ test_crear_producto_usecase.py
┃ ┃ ┗ test_eliminar_producto_usecase.py
┣ 📘 README.md                             ← Documentación principal del proyecto
┗ ⚠️ .gitignore                            ← Define qué archivos/directorios ignorar en control de versiones (ej. logs/, .env)
```
