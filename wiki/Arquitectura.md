# Arquitectura distribuida

## Componentes

```mermaid
flowchart LR
    T["Cliente: tienda-web"] -->|REST lectura| Q["Query Service :8622"]
    A["Cliente: admin-web"] -->|REST lectura| Q
    T -->|REST escritura| C["Command Service :8620"]
    A -->|REST escritura| C
    C -->|Publica eventos| K[("Apache Kafka :9092")]
    K -->|Consume eventos| Q
    C -->|SQL| DB[("PostgreSQL Neon")]
    Q -->|SQL| DB
    Q -->|WebSocket broadcast| T
    Q -->|WebSocket broadcast| A
```

## CQRS
- **Command Service:** operaciones que cambian el estado: crear/editar productos y registrar pedidos.
- **Query Service:** consultas del catálogo y los pedidos; consume eventos Kafka para mantener proyecciones.
- **Persistencia:** `products_command`, `orders_command`, `products_query`, `orders_query`. Las cuatro tablas están en **una misma base PostgreSQL Neon**, pero con responsabilidades separadas.

## Secuencia de compra

```mermaid
sequenceDiagram
    actor Cliente
    participant Tienda
    participant Command
    participant Neon
    participant Kafka
    participant Query
    Cliente->>Tienda: Confirmar compra
    Tienda->>Command: POST /orders
    Command->>Neon: Guardar pedido y actualizar stock
    Command->>Kafka: Publicar evento
    Kafka-->>Query: Consumir evento
    Query->>Neon: Actualizar proyección
    Query-->>Tienda: WebSocket broadcast
    Query-->>Cliente: Datos actualizados en pantalla
```

## Tecnologías y decisiones
Consulta [Tecnologías y justificación](Tecnologias-y-justificacion.md) y [Requerimientos](Requerimientos.md).
