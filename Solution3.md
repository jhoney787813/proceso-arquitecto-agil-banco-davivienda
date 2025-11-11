# 3) Arquitectura de Microservicios para Aplicación de Comercio Electrónico


## Planteamiento del Problema
Diseña la arquitectura de un sistema de microservicios para una aplicación de comercio electrónico que debe manejar altos volúmenes de transacciones, implementar escalabilidad dinámica y tolerancia a fallos. Utiliza un esquema que considere tanto la comunicación entre servicios como la seguridad.

---
## Propuesta de Solución

Mi propuesta para dar cumplimiento a la solución planteada consiste en:

## 1. Enfoque General

La solución está basada en un ecosistema de **microservicios desacoplados** que siguen los principios de **CQRS** y **event-driven architecture**. Cada servicio tiene responsabilidad única y se comunica con otros mediante un **broker de mensajería** (Kafka, RabbitMQ, AWS SNS/SQS).  

- CQRS separa comandos (writes) de consultas (reads), optimizando rendimiento y escalabilidad.  
- Cada microservicio maneja su propia lógica de transacciones y rollback, sin utilizar procedimientos almacenados.  
- Se garantiza tolerancia a fallos mediante **sagas, compensaciones, circuit breakers** y mensajería confiable.  
- La arquitectura es agnóstica al lenguaje de programación, permitiendo usar distintos stacks tecnológicos.

---

## 2. Microservicios Clave

| Microservicio      | Función Principal |
|-------------------|-----------------|
| **Usuarios**       | Gestión de cuentas, autenticación y roles. |
| **Catálogo**       | Gestión de productos, precios y disponibilidad. |
| **Pedidos**        | Registro y coordinación de órdenes, publica eventos de estado. |
| **Inventario**     | Actualiza stock en tiempo real, responde a eventos de pedidos. |
| **Pagos**          | Procesa pagos y publica eventos de éxito/fallo de transacción. |
| **Notificaciones** | Envía emails, SMS o push de estados de pedidos/pagos. |

---

## 3. Comunicación y Mensajería

- **Mensajería basada en eventos** para desacoplar servicios y manejar picos de carga.
  - Ejemplo: cuando **Pedidos** confirma un pedido, se publica `OrderCreated` que **Inventario**, **Pagos** y **Notificaciones** consumen.
- **API Gateway** para solicitudes externas, autenticación y enrutamiento.
- **Broker de mensajes** garantiza entrega confiable y ordenada.
- **Circuit breakers** y **retry policies** para manejar fallos entre servicios.

---

## 4. Manejo de Transacciones y Rollback

- Cada servicio gestiona su **propia base de datos y lógica de persistencia**.  
- Las transacciones distribuidas se implementan mediante **sagas**:
  - Cada comando que modifica estado tiene un **evento compensador** que revierte la acción si falla alguna etapa.
  - Ejemplo: si un pago falla después de reservar inventario, se publica `CompensateInventory` para devolver stock.
- Se evita el uso de **procedimientos almacenados**, centralizando la lógica de negocio en la capa de aplicación.

---

## 5. Escalabilidad Dinámica y Tolerancia a Fallos

- Microservicios escalables horizontalmente mediante contenedores (Docker/Kubernetes).  
- CQRS separa comandos y consultas, optimizando la escalabilidad de lectura/escritura.  
- **Replicación de bases de datos** y **multi-zona** para alta disponibilidad.  
- Monitoreo centralizado y logging distribuido para detectar y recuperarse de fallos rápidamente.

---

## 6. Seguridad

- **Autenticación centralizada** con JWT/OAuth2.  
- **Comunicación segura** entre servicios mediante TLS.  
- **Segregación de datos**: cada servicio accede únicamente a su propia base de datos.  
- **Auditoría de eventos y transacciones** para trazabilidad completa.

---

## 7. Argumentos Clave del Diseño

1. CQRS y mensajería basada en eventos permiten desacoplar servicios, escalar dinámicamente y tolerar fallos sin afectar otras partes del sistema.  
2. Rollback en la capa de aplicación asegura consistencia sin depender de procedimientos almacenados.  
3. Arquitectura agnóstica al lenguaje: los servicios pueden implementarse en cualquier lenguaje compatible con mensajería estándar y REST/GRPC.  
4. Seguridad y resiliencia integradas, con monitoreo, circuit breakers y políticas de retry.

---

## 8. Diagrama Conceptual (Mermaid)

```mermaid
graph TD
    A[Cliente / Usuario] -->|Solicita operación| API[API Gateway]

    subgraph Microservicios
        UC[Usuarios] 
        CAT[Catálogo] 
        ORD[Pedidos]
        INV[Inventario]
        PAY[Pagos]
        NOT[Notificaciones]
    end

    API --> UC
    API --> CAT
    API --> ORD

    ORD -->|OrderCreated| INV
    ORD -->|OrderCreated| PAY
    ORD -->|OrderCreated| NOT

    PAY -->|PaymentFailed / Success| ORD
    INV -->|StockCompensated| ORD

    style Microservicios fill:#f9f,stroke:#333,stroke-width:2px

```
Este diagrama representa la comunicación entre clientes, API Gateway y microservicios. Las líneas representan eventos y llamadas API, siguiendo CQRS y arquitectura basada en eventos.
