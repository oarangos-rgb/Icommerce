# Tecnologías y justificación

Esta página explica **qué tecnología se usa, en qué parte del proyecto y por qué**.

| Tecnología | Lugar en iCommerce | Justificación |
|---|---|---|
| **Java + Spring Boot** | `server/command-service`, `server/query-service` | Implementa API REST, lógica de negocio, eventos y consumidores |
| **CQRS** | Servicios Command y Query | Separa escritura y lectura; los modelos se guardan en tablas distintas |
| **Apache Kafka** | Entre ambos servicios, `localhost:9092` | Transporta eventos de forma asíncrona y desacopla procesos |
| **WebSocket** | `query-service` en `/ws`, clientes web | Realiza broadcast de cambios a navegadores conectados |
| **PostgreSQL Neon** | `neondb` en nube | Conserva productos y pedidos para los dos servicios |
| **HTML, CSS, JavaScript** | `clients/tienda-web` y `clients/admin-web` | Proporcionan aplicaciones clientes independientes |
| **GitHub + Wiki** | Repositorio y documentación | Permite entrega, trazabilidad y explicación del proyecto |

## Diferencia importante: Kafka vs. WebSocket
**Kafka no es el broadcast al navegador.** Kafka comunica eventos **entre servicios**. WebSocket transmite las actualizaciones **del Query Service hacia los navegadores**. Ambas tecnologías participan en el flujo, pero cumplen funciones diferentes.

## Diferencia importante: CQRS vs. dos bases de datos
CQRS separa las responsabilidades de **escritura** y **lectura**. En este proyecto se usan tablas `*_command` y `*_query` en la **misma base de datos Neon**. Eso demuestra la separación lógica sin requerir dos instancias PostgreSQL.

## Evidencia
Consultar [Pruebas y evidencias](Pruebas-y-evidencias.md) y [Requerimientos](Requerimientos.md).
