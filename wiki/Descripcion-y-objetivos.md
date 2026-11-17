# Descripción y objetivos

## Problema
Una tienda necesita registrar productos, pedidos y cambios de inventario, y reflejar las modificaciones en las interfaces sin recargar la página.

## Objetivo general
Diseñar un prototipo académico de comercio electrónico distribuido que integre CQRS, mensajería asíncrona, conexiones broadcast y persistencia en nube.

## Objetivos específicos
- Separar la escritura de las consultas mediante Command Service y Query Service.
- Publicar y consumir eventos de productos y pedidos con Apache Kafka.
- Enviar cambios a los clientes conectados mediante WebSocket.
- Implementar dos clientes web independientes: tienda y administración.
- Persistir productos y pedidos en PostgreSQL Neon.
- Documentar la arquitectura, API, requerimientos, instalación y pruebas en GitHub.

## Alcance
El proyecto permite administrar productos, consultar catálogo, gestionar carrito y registrar pedidos. Es un prototipo académico local; no incluye autenticación, cobros reales ni despliegue público.
