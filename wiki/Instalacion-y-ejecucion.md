# Instalación y ejecución

## Requisitos
- Java 17, Maven e IntelliJ IDEA o IDE compatible.
- Apache Kafka disponible en `localhost:9092` (o configuración equivalente).
- Base PostgreSQL en Neon con las tablas requeridas.
- Navegador y servidor HTTP local para abrir las aplicaciones clientes.

## 1. Configurar PostgreSQL Neon
En los **dos** servicios configura las variables de entorno `DB_URL`, `DB_USER` y `DB_PASSWORD`. Nunca publiques los valores reales en GitHub.

## 2. Configurar Kafka
Inicia Kafka y comprueba la disponibilidad del broker. Los servicios usan los tópicos `product-events` y `order-events`. Si corresponde, configura `KAFKA_BOOTSTRAP`.

## 3. Ejecutar el servidor
- Abre `server/command-service` y ejecuta `CommandServiceApplication` (puerto **8620**).
- Abre `server/query-service` y ejecuta `QueryServiceApplication` (puerto **8622**).

## 4. Ejecutar los clientes
Sirve `clients/index.html` mediante HTTP local (por ejemplo el servidor integrado de IntelliJ). Desde allí accede a:
- `clients/tienda-web/index.html` — tienda.
- `clients/admin-web/index.html` — administración.

En IntelliJ la URL suele empezar por `http://localhost:63342/`, según la configuración del proyecto.

## 5. Comprobar
- `GET http://localhost:8622/products` debe devolver el catálogo.
- Crear o editar un producto desde administración.
- Abrir tienda en dos pestañas y comprobar las notificaciones en tiempo real.
- Registrar una compra y consultar pedidos.
- Verificar las tablas en Neon y los eventos en los registros de los servicios.

## Solución de problemas
- **Error de conexión a base:** revisa URL, usuario, contraseña y acceso de Neon.
- **Cambios no reflejados:** revisa Kafka, los consumidores y la proyección CQRS.
- **Sin broadcast:** comprueba WebSocket `/ws` y la consola del navegador.
- **Error CORS:** sirve el frontend desde el origen esperado y revisa la configuración del backend.
