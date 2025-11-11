
https://structurizr.com/

1) Se piensa desarrollar una solución para ofrecer créditos en línea la cual se espera pueda ser usada en diferentes países, para esto se debe hacer conexión con algunos servicios del core bancario y se debe dejar un registro detallado de cada una de las solicitudes que hagan los clientes. 

Teniendo en cuenta la pregunta anterior plantee un diagrama de arquitectura que permita evidenciar los componentes, servicios, reglas, o consideraciones a tener en cuenta.


R) mi propuesta para dar cumplimiento de solución propuesta consiste en desarrollar una aplicación de créditos en línea basada en microservicios, separando las responsabilidades de cada servicio para facilitar escalabilidad y mantenimiento. Se implementa CQRS, separando comandos (escrituras) de consultas (lecturas), optimizando el rendimiento y permitiendo proyecciones específicas para auditoría. Cada solicitud y acción relevante se registra en tablas de auditoría internas, asegurando trazabilidad completa y cumplimiento regulatorio. La integración con el CORE bancario existente se realiza mediante APIs, evitando modificaciones en el sistema central. Finalmente, se utiliza un API Gateway para centralizar autenticación, enrutamiento y comunicación entre los microservicios, garantizando una arquitectura modular y flexible. 




Justificación de la propuesta de implementación

Arquitectura basada en microservicios

Cada función de la aplicación (solicitud de crédito, validación de documentos, notificación al cliente, auditoría) se implementa como un microservicio independiente, lo que facilita la escalabilidad, despliegue incremental y la adaptación a diferentes países con reglas locales específicas.

Esto permite minimizar cambios en el core bancario existente, limitando la integración a interfaces de servicios y APIs.

Uso de CQRS (Command Query Responsibility Segregation)

Separar el modelo de escritura (commands) del modelo de lectura (queries) para manejar de manera eficiente las solicitudes de crédito y los reportes de auditoría.

Las escrituras (nuevas solicitudes, cambios de estado) se realizan en un servicio de dominio, mientras que las consultas (visualización de estado de solicitud, reportes de auditoría) se realizan sobre proyecciones optimizadas para lectura.

Auditoría interna

Cada solicitud y acción relevante se registra en tablas de auditoría internas, de manera desacoplada de los microservicios de negocio.

Esto asegura trazabilidad completa y facilita reportes regulatorios sin impactar la lógica del core bancario.

Integración con el core bancario existente

La comunicación se hace mediante APIs o servicios de integración, evitando cambios en el core.

Los microservicios pueden orquestar llamadas a estos servicios y enriquecer la información con reglas locales de cada país.

Escalabilidad y resiliencia

Uso de colas de mensajes para desacoplar servicios y garantizar resiliencia frente a picos de solicitudes.

Microservicios independientes permiten escalar solo los módulos críticos sin afectar todo el sistema.



Dagrama de Contexto general

<img width="1872" height="572" alt="image" src="https://github.com/user-attachments/assets/388eda34-d5ef-4fab-801f-61684ef35b39" />





2) El Banco está planeando migrar un aplicación monolítica a la nube pública. El sistema actual está alojado on-premise y soporta grandes volúmenes de datos y conexiones concurrentes. Proporciona una propuesta arquitectónica para migrar este sistema, asegurando alta disponibilidad, recuperación ante desastres y eficiencia en costos.

   

4) Diseña la arquitectura de un sistema de microservicios para una aplicación de comercio electrónico que debe manejar altos volúmenes de transacciones, implementar escalabilidad dinámica y tolerancia a fallos. Utiliza un esquema que considere tanto la comunicación entre servicios como la seguridad.

   
5) Explique cómo garantizas el análisis de la calidad de código en las aplicaciones de una organización que cuenta con cientos de repositorios desarrollados en diferentes lenguajes y con diferentes frameworks

   
6) Diseña la arquitectura de una API RESTFul que permita a terceros integrar servicios a una plataforma financiera. La API debe implementar control de acceso, ser escalable y cumplir con estándares de seguridad financiera. Realiza el diagrama y explica las decisiones técnicas clave.
