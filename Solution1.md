# 1) Desarrollo de Solución de Créditos en Línea 

## Planteamiento del Problema
Se piensa desarrollar una solución para ofrecer créditos en línea, la cual se espera pueda ser usada en diferentes países. Para esto:

- Se debe hacer conexión con algunos servicios del core bancario.
- Se debe dejar un registro detallado de cada una de las solicitudes que hagan los clientes.

Teniendo en cuenta lo anterior, se plantea la necesidad de un **diagrama de arquitectura** que permita evidenciar los componentes, servicios, reglas y consideraciones a tener en cuenta.

## Propuesta de Solución
Mi propuesta para dar cumplimiento a la solución planteada consiste en:

- Desarrollar una **aplicación de créditos en línea basada en microservicios**, separando las responsabilidades de cada servicio para facilitar escalabilidad y mantenimiento.
- Implementar **CQRS (Command Query Responsibility Segregation)**, separando:
  - **Comandos (escrituras):** para crear nuevas solicitudes o cambiar estados.
  - **Consultas (lecturas):** para visualizar el estado de las solicitudes y generar proyecciones específicas para auditoría, optimizando el rendimiento.
- Registrar cada solicitud y acción relevante en **tablas de auditoría internas**, asegurando:
  - Trazabilidad completa.
  - Cumplimiento regulatorio.
- Integrar con el **CORE bancario existente** mediante **APIs**, evitando modificaciones en el sistema central.
- Utilizar un **API Gateway** para centralizar:
  - Autenticación.
  - Enrutamiento.
  - Comunicación entre microservicios, garantizando una arquitectura modular y flexible.


## Justificación de la Propuesta de Implementación

 **Arquitectura basada en microservicios**
Cada función de la aplicación (solicitud de crédito, validación de documentos, notificación al cliente, auditoría) se implementa como un microservicio independiente, lo que facilita:

- Escalabilidad.
- Despliegue incremental.
- Adaptación a diferentes países con reglas locales específicas.

Esto permite minimizar cambios en el core bancario existente, limitando la integración a interfaces de servicios y APIs.

## Uso de CQRS (Command Query Responsibility Segregation)
Se separa el modelo de escritura (commands) del modelo de lectura (queries) para manejar de manera eficiente:

- **Escrituras:** Nuevas solicitudes, cambios de estado, realizadas en un servicio de dominio.
- **Consultas:** Visualización de estado de solicitud y reportes de auditoría, realizadas sobre proyecciones optimizadas para lectura.

## Auditoría interna
Cada solicitud y acción relevante se registra en tablas de auditoría internas, desacopladas de los microservicios de negocio. Esto garantiza:

- Trazabilidad completa.
- Reportes regulatorios sin impactar la lógica del core bancario.

## Integración con el core bancario existente
La comunicación se realiza mediante APIs o servicios de integración, evitando cambios directos en el core. Los microservicios pueden:

- Orquestar llamadas a estos servicios.
- Enriquecer la información con reglas locales de cada país.

## Escalabilidad y resiliencia
Se utilizan colas de mensajes para desacoplar servicios y garantizar resiliencia frente a picos de solicitudes. Además:

- Los microservicios independientes permiten escalar solo los módulos críticos sin afectar todo el sistema.


## Componentes y Consideraciones Clave

- **API Gateway:** Centraliza autenticación, autorización, enrutamiento y control de tráfico.
- **Microservicios:** Cada módulo (solicitud, validación, notificación) es independiente, facilitando escalabilidad y mantenimiento.
- **CQRS:** Separación de comandos y consultas para optimizar rendimiento y auditoría.
- **Auditoría Interna:** Registro detallado de cada solicitud y acción relevante, garantizando trazabilidad.
- **Integración con Core Bancario:** Solo mediante APIs, sin cambios en el sistema central.
- **Escalabilidad y Resiliencia:** Uso de colas de mensajes (no representadas en el diagrama) para desacoplar servicios y soportar picos de carga.




Dagrama de Contexto general

<img width="1872" height="572" alt="image" src="https://github.com/user-attachments/assets/388eda34-d5ef-4fab-801f-61684ef35b39" />

Diagrama de contenedores

<img width="1786" height="1057" alt="image" src="https://github.com/user-attachments/assets/954dfae2-17f9-46bf-ba40-7469cf03c4a7" />


Diagrama de componentes

<img width="1342" height="1056" alt="image" src="https://github.com/user-attachments/assets/30bad884-936d-4d0a-8f9b-4d9227e55634" />
Componete Commands:
<img width="1870" height="932" alt="image" src="https://github.com/user-attachments/assets/e73f1a98-7bc8-4f8d-b41c-ed852762ab32" />

Componete Querys:
<img width="1802" height="950" alt="image" src="https://github.com/user-attachments/assets/a2fb24f5-e34b-4b99-a56b-fbe44a2558e6" />
