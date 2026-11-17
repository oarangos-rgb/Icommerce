# Pruebas y evidencias

## Casos de prueba
| Caso | Procedimiento | Resultado esperado |
|---|---|---|
| Crear producto | Registrar producto en administración | Aparece en catálogo y persistencia |
| Modificar precio | Editar desde administración | Catálogo recibe el cambio |
| Comprar | Añadir producto al carrito y confirmar | Se registra pedido y actualiza stock |
| CQRS | Comparar tablas command y query | Se observa separación de lectura/escritura |
| Kafka | Consultar registros de publicación/consumo | Evento se transmite al Query Service |
| Broadcast | Abrir dos pestañas y editar producto | Ambas reciben actualización sin recargar |

## Capturas del proyecto
Estas capturas documentan las interfaces y consultas realizadas durante el desarrollo. No sustituyen la prueba específica de broadcast simultáneo ni la revisión de logs de Kafka.

![Catálogo de tienda](imagenes/Tienda-catalogo.png)
![Panel de pedidos](imagenes/Administracion-pedidos.png)
![Formulario de administración](imagenes/Formulario-administracion.png)
![Carrito de compras](imagenes/Carrito-compras.png)
![Productos en Neon](imagenes/Neon-productos.png)
![Pedidos en Neon](imagenes/Neon-pedidos.png)

> **Seguridad:** revisar que ninguna captura revele claves, tokens o cadenas de conexión privadas antes de publicar las imágenes.
