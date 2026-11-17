# 🛒 iCommerce — Wiki del proyecto

**Sistemas Distribuidos · Ingeniería de Sistemas · 2026**  
**Estudiantes:** Odin Arango y Dilan Villota  
**Profesor:** Giovnni Briceños

iCommerce es un prototipo académico de comercio electrónico que integra **CQRS, Apache Kafka, WebSocket broadcast, aplicaciones clientes independientes y PostgreSQL Neon en la nube**.

## Contenido de la Wiki
1. [Descripción y objetivos](Descripcion-y-objetivos.md)
2. [Requerimientos y criterios de aceptación](Requerimientos.md)
3. [Tecnologías: dónde se usan y por qué](Tecnologias-y-justificacion.md)
4. [Arquitectura distribuida y diagramas](Arquitectura.md)
5. [Instalación y ejecución](Instalacion-y-ejecucion.md)
6. [API REST y eventos](API.md)
7. [Pruebas y evidencias](Pruebas-y-evidencias.md)

## Componentes
- **Cliente 1:** `clients/tienda-web/index.html` — catálogo y compras.
- **Cliente 2:** `clients/admin-web/index.html` — gestión de productos y pedidos.
- **Inicio:** `clients/index.html` — navegación.
- **Servidor:** `command-service` (`8620`) y `query-service` (`8622`).
- **Mensajería:** Kafka (`9092`). **Broadcast:** WebSocket (`/ws`).
- **Persistencia:** PostgreSQL Neon.

> **Alcance:** prototipo académico local; no tiene autenticación ni cobros reales. No publicar credenciales.
