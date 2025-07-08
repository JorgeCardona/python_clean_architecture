# python_clean_architecture
This project is a base template for building Python microservices using Clean Architecture. It isolates business logic from external tech (frameworks, databases, APIs), enabling a decoupled, testable, and maintainable system.

![clean_architecture](https://raw.githubusercontent.com/JorgeCardona/python_microservice_scaffolding_clean_architecture/refs/heads/master/resources/clean_architecture.jpg)

# 🐍 SCAFFOLDING FOR CLEAN ARCHITECTURE IN MICROSERVICES
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
