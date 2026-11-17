# Requerimientos del sistema iCommerce

**Proyecto:** iCommerce · **Asignatura:** Sistemas Distribuidos · **Programa:** Ingeniería de Sistemas  
**Estudiantes:** Odin Arango y Dilan Villota · **Profesor:** Giovnni Briceños · **Año:** 2026

## 1. Alcance
Prototipo académico de comercio electrónico distribuido. Incluye dos clientes web independientes, dos servicios Spring Boot, comunicación de eventos con Apache Kafka, broadcast por WebSocket y persistencia PostgreSQL alojada en Neon.

## 2. Requerimientos funcionales: ubicación, tecnología y justificación

| ID | Requerimiento | ¿Dónde se implementa? | Tecnología y por qué se usa | Evidencia / verificación |
|---|---|---|---|---|
| RF-01 | Crear productos (nombre, descripción, precio, stock) | `clients/admin-web/index.html` → `CommandController` y `CommandService` | Formulario HTML y `POST /products` en Spring Boot: centraliza las escrituras y reglas de negocio | Crear producto y verificar inventario |
| RF-02 | Editar productos y existencias | Admin → `PUT /products/{id}` | API REST de Command: actualiza el modelo de escritura | Modificar precio y observar catálogo |
| RF-03 | Consultar catálogo y stock | `clients/tienda-web/index.html` → `QueryController` | `GET /products`: las lecturas se realizan sobre una proyección CQRS | Abrir catálogo y buscar producto |
| RF-04 | Registrar compras | Tienda → `POST /orders` → `CommandService` | Solicitud HTTP para validar cantidad, registrar pedido y ajustar stock | Confirmar compra y revisar pedido |
| RF-05 | Consultar pedidos | Admin → `GET /orders` en Query Service | Modelo de lectura independiente: facilita las consultas | Revisar historial de pedidos |
| RF-06 | Notificar cambios a varios clientes (broadcast) | `BroadcastHandler`, `WebSocketConfig` en Query Service; scripts de ambas webs | WebSocket `/ws`: conexión persistente para enviar eventos a múltiples navegadores sin recargar | Abrir dos pestañas y cambiar precio |
| RF-07 | Propagar cambios entre servicios | Command Service → Kafka → `ProjectionService` | Tópicos `product-events` y `order-events`: mensajería asíncrona y desacoplamiento | Verificar logs y proyecciones |
| RF-08 | Mantener vistas de lectura sincronizadas | `ProjectionService` y tablas `products_query`, `orders_query` | CQRS y consumidores Kafka: reconstruyen la información consultable | Comparar tablas command/query en Neon |
| RF-09 | Buscar productos y gestionar carrito | `clients/tienda-web/index.html` | JavaScript en cliente: interacción inmediata; compra envía un pedido por producto | Buscar, sumar unidades y comprar |
| RF-10 | Acceder a los clientes desde un inicio común | `clients/index.html` | HTML y enlaces relativos: punto de entrada a tienda y administración | Abrir página principal |

## 3. Requerimientos de arquitectura y no funcionales

| ID | Requisito del curso | Ubicación exacta | ¿Por qué se usa allí? | Criterio de aceptación |
|---|---|---|---|---|
| RNF-01 | **CQRS** | `server/command-service` y `server/query-service` | Separar operaciones de modificación de las de consulta; los datos se guardan en tablas command y query diferenciadas | POST/PUT a `:8620`; GET a `:8622` |
| RNF-02 | **Pila de mensajería** | Productores de Command, Kafka `:9092`, consumidores de Query | Desacoplar microservicios y transportar eventos asíncronos | Evento llega al consumidor y actualiza vista |
| RNF-03 | **Conexiones broadcast** | Query Service `/ws` y conexiones WebSocket de clientes | Difundir un mismo cambio a múltiples clientes conectados | Dos pestañas reciben cambio sin recarga |
| RNF-04 | **Aplicación servidora** | Dos proyectos Spring Boot en `server/` | Exponer endpoints y ejecutar lógica de negocio/proyecciones | Ambos servicios arrancan |
| RNF-05 | **Aplicaciones clientes separadas** | `clients/tienda-web` y `clients/admin-web` | Roles distintos y comunicación por red con el servidor | Cada interfaz se abre por separado |
| RNF-06 | **BD PostgreSQL en nube** | Neon `neondb` y `application.yml` de ambos servicios | Persistencia accesible para ambos servicios, sin instalar PostgreSQL local para la demostración | Consultas SQL muestran productos y pedidos |
| RNF-07 | **GitHub y Wiki** | Repositorio GitHub, `README.md` y Wiki | Versionar el código y explicar arquitectura, requerimientos, instalación y evidencias | Enlaces accesibles para el docente |
| RNF-08 | Diseño adaptable | CSS de ambos clientes | Permitir visualización en escritorio y pantallas pequeñas | Revisar ancho móvil y escritorio |
| RNF-09 | Consistencia eventual | Kafka + `ProjectionService` | La vista de lectura se actualiza después del evento; puede existir demora breve | Cambio aparece tras procesar evento |

## 4. Recorrido de una compra
1. El cliente selecciona productos en `tienda-web`.
2. La tienda envía `POST http://localhost:8620/orders` con `productId`, `quantity` y `customer`.
3. Command Service valida, persiste y descuenta stock en el modelo de escritura.
4. Command Service publica eventos en Kafka.
5. Query Service consume eventos y actualiza las tablas de lectura de Neon.
6. Query Service transmite `PRODUCT_CHANGED` y `ORDER_CREATED` por WebSocket a los navegadores conectados.
7. Tienda y administración muestran los cambios sin recargar, siempre que las conexiones estén activas.

## 5. Pruebas de aceptación sugeridas
- **PA-01:** Crear un producto desde administración y verificar que aparezca en la tienda.
- **PA-02:** Abrir tienda en dos pestañas y modificar el precio desde administración; verificar actualización en ambas.
- **PA-03:** Comprar un producto; comprobar pedido, importe y decremento de stock.
- **PA-04:** Verificar `products_command`, `products_query`, `orders_command`, `orders_query` en Neon.
- **PA-05:** Detener Query Service y comprobar que la interfaz indica reconexión; restaurarlo y verificar recuperación de WebSocket.
- **PA-06:** Mostrar en logs la publicación/consumo de eventos de Kafka.

Las capturas disponibles respaldan formularios, catálogo, carrito, pedidos y consultas a Neon. Las pruebas de difusión simultánea y recuperación deben ejecutarse y documentarse antes de afirmarlas como verificadas.

## 6. Limitaciones y seguridad
- No hay autenticación ni autorización; el panel administrativo es únicamente para desarrollo local.
- No existe pasarela de pago real ni transacción única para un carrito de varios productos: se crea un pedido por producto.
- No se documentan pruebas automatizadas ni despliegue público con HTTPS.
- No subir credenciales de Neon, contraseñas, `.env` reales ni capturas que revelen secretos.
- Los dos modelos CQRS están en **tablas separadas de la misma base Neon**; no afirmar que son bases de datos físicas independientes.
