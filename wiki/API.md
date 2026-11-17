# API REST y eventos

## Command Service — http://localhost:8620

| Método | Ruta | Descripción |
|---|---|---|
| POST | `/products` | Crear producto |
| PUT | `/products/{id}` | Actualizar producto |
| POST | `/orders` | Registrar pedido |

**Producto:** `name`, `description`, `price`, `stock`.

**Pedido:** `productId`, `quantity`, `customer`.

## Query Service — http://localhost:8622

| Método | Ruta | Descripción |
|---|---|---|
| GET | `/products` | Listar productos |
| GET | `/products/{id}` | Consultar producto |
| GET | `/orders` | Listar pedidos |

## Mensajería y broadcast
- Kafka: `product-events` y `order-events`, habitualmente en `localhost:9092`.
- WebSocket: `ws://localhost:8622/ws`.
- Notificaciones: `PRODUCT_CHANGED` y `ORDER_CREATED`.

> La actualización de Query puede ser eventual, ya que depende de consumir eventos Kafka. Los endpoints son para desarrollo local y no cuentan con autenticación.
