# 🛒 iCommerce — Sistemas Distribuidos

**Odin Arango · Dilan Villota**  
**Ingeniería de Sistemas · Profesor: Giovnni Briceños · 2026**

Prototipo académico de comercio electrónico distribuido con **Spring Boot (Java 17), CQRS, Kafka, WebSocket y PostgreSQL Neon**.

## Componentes
- `server/command-service`: escritura REST (8620)
- `server/query-service`: lectura REST (8622) y WebSocket (`/ws`)
- `clients/index.html`: inicio
- `clients/tienda-web`: tienda, buscador y carrito
- `clients/admin-web`: inventario, administración y pedidos
- `wiki/`: documentación del proyecto

## Inicio rápido
1. Inicia Kafka en `localhost:9092`.
2. Configura `DB_URL`, `DB_USER`, `DB_PASSWORD` en **ambos** servicios; opcional `KAFKA_BOOTSTRAP`.
3. Ejecuta `CommandServiceApplication` y `QueryServiceApplication` en IntelliJ.
4. Abre `clients/index.html` mediante servidor HTTP (por ejemplo el servidor de IntelliJ).

## Documentación
[Inicio de la documentación](wiki/Home.md) · [Requerimientos](wiki/Requerimientos.md) · [Tecnologías y justificación](wiki/Tecnologias-y-justificacion.md)

> ⚠️ Prototipo académico sin autenticación ni pagos reales. No publiques credenciales de Neon. Si alguna clave se expuso en capturas, cámbiala antes de compartir el repositorio.
