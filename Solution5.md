# 5) Propuesta de Arquitectura de la API RESTful

## Planteamiento del Problema

Diseña la arquitectura de una API RESTFul que permita a terceros integrar servicios a una plataforma financiera. La API debe implementar control de acceso, ser escalable y cumplir con estándares de seguridad financiera. Realiza el diagrama y explica las decisiones técnicas clave.

---
## Propuesta de Solución


La arquitectura que propongo es para la integración de servicios de terceros a una plataforma financiera. A continuación, te detallo la arquitectura, las decisiones técnicas clave, el uso del patrón Mediator, y la integración de Ocelot como un API Gateway para la gestión de seguridad y enrutamiento.

# Funcionalidades priorizadas

1. **Control de acceso robusto**
   
2. **Escalabilidad** para manejar un alto volumen de transacciones.

3. **Cumplir con estándares de seguridad financiera.**

4. **Desacoplamiento y mantenibilidad** usando el patrón Mediator.

5. **Seguridad centralizada** con Ocelot como API Gateway.

# 1. Arquitectura General


La solución se basa en un enfoque de **microservicios**, donde diferentes funcionalidades de la plataforma financiera (por ejemplo, gestión de cuentas, pagos, reportes) se agrupan en servicios independientes. La API **RESTful** servirá de interfaz para permitir que los servicios de terceros interactúen con la plataforma.

### Componentes clave:

- **API Gateway (Ocelot):**  
  Gestiona todas las solicitudes, aplica políticas de seguridad y enruta las peticiones a los microservicios correspondientes.

- **Microservicios:**  
  Cada microservicio maneja una funcionalidad específica como cuentas, transacciones, pagos, etc.

- **Base de datos:**  
  Utilizamos una base de datos distribuida para almacenar información, con particionamiento y escalabilidad horizontal.

- **Autenticación y Autorización:**  
  Implementación de **OAuth 2.0** con **JWT** para asegurar que solo usuarios autorizados puedan acceder a los servicios.

- **Patrón Mediator:**  
  Desacopla las interacciones entre las capas de la API, facilitando la escalabilidad y mantenimiento.

  <img width="1086" height="816" alt="image" src="https://github.com/user-attachments/assets/6ea200ac-d5c0-420e-a060-7ab0ca66ecd2" />




  # 2. Justificación Decisiones Técnicas 

## 2.1. Control de Acceso

**¿Por qué esta decisión?**

**Seguridad:** OAuth 2.0 es un protocolo de autenticación probado que asegura que solo los usuarios o sistemas con permisos adecuados puedan acceder a los recursos.

**Simplicidad y eficiencia:** Al usar JWT, los tokens que se generan contienen toda la información necesaria para verificar la autenticación de un usuario sin necesidad de consultas adicionales a bases de datos.

**Control granular:** A través de este sistema, podemos establecer roles y permisos que limitan el acceso a diferentes partes del sistema según las necesidades del usuario.

**Beneficio para la empresa:** Esto no solo mejora la seguridad al evitar accesos no autorizados, sino que también simplifica la gestión de usuarios y permisos de forma escalable, sin comprometer el rendimiento.

---

## 2.2. Escalabilidad

**¿Por qué esta decisión?**

**Crecimiento controlado:** Permite escalar únicamente aquellos servicios que realmente lo necesiten, en lugar de tener que aumentar la capacidad de toda la plataforma. Por ejemplo, si el sistema de pagos tiene un alto volumen de transacciones, podemos aumentar solo su capacidad sin afectar otras partes del sistema.

**Flexibilidad:** Cada microservicio puede ser optimizado y mejorado de manera independiente, lo que facilita la innovación y el mantenimiento de la plataforma.

**Rendimiento optimizado:** Gracias a herramientas como Ocelot (el API Gateway), las solicitudes se enrutan de manera eficiente, lo que asegura que el sistema maneje de forma óptima el tráfico de usuarios.

**Beneficio para la empresa:** Esta escalabilidad permitirá manejar un alto volumen de transacciones, lo cual es clave para el crecimiento de la plataforma sin necesidad de inversiones grandes e innecesarias en infraestructura.


## 2.3. Seguridad Financiera

La seguridad de la información financiera es una prioridad máxima. Hemos implementado varias capas de protección para garantizar que los datos sensibles estén siempre seguros.

**¿Por qué esta decisión?**

Cifrado: Todos los datos se cifran tanto en tránsito (cuando viajan por la red) como en reposo (cuando están almacenados). Esto asegura que los datos no puedan ser interceptados ni leídos por personas no autorizadas.

**Auditoría:** Centralizamos los registros de todas las transacciones para monitorear cualquier actividad sospechosa en tiempo real. Esto no solo ayuda a cumplir con las regulaciones, sino que también proporciona visibilidad y control total sobre las operaciones.

**Protección contra ataques DDoS:** Usamos Ocelot para aplicar limitación de tasa (Rate Limiting), lo que evita que una sobrecarga de tráfico malicioso pueda afectar el rendimiento o la disponibilidad del sistema.

**Beneficio para la empresa:** Estas medidas protegen la plataforma contra posibles ataques y aseguran que la integridad y privacidad de los datos financieros se mantengan en todo momento, lo que es crucial para ganar y mantener la confianza de los usuarios y cumplir con los estándares regulatorios..

## 2.4. Implementación del Patrón Mediator

El patrón **Mediator** desacopla las interacciones entre las diferentes capas de la aplicación. En lugar de que los controladores interactúen directamente con la lógica de negocio, usan el **Mediator** para enviar comandos y consultas.

#### Ventajas:

- **Desacoplamiento:**  
  Los controladores no dependen de servicios concretos, lo que facilita la flexibilidad y modularidad del sistema.

- **Mantenibilidad:**  
  Al centralizar las operaciones de negocio, se reduce la complejidad y se facilita la gestión de las reglas de negocio. Esto hace que el código sea más fácil de mantener y extender.

- **Escalabilidad:**  
  El patrón facilita la implementación de nuevas funcionalidades sin afectar a las existentes. Cada nueva característica puede ser añadida de manera aislada, minimizando el riesgo de errores en otras partes del sistema.


## 2.5. Implementación de Ocelot (API Gateway)

**Ocelot** es una herramienta que actuará como el "puente" entre los usuarios y los microservicios de nuestra plataforma. Su principal función será gestionar el enrutamiento de las solicitudes que vienen desde los usuarios hacia los microservicios adecuados, asegurando que la plataforma funcione de manera eficiente, segura y escalable.

**¿Por qué usamos Ocelot? (API Gateway)**


1. **Centralización y Control de Acceso**  
   Ocelot centraliza la autenticación de usuarios mediante JWT, protegiendo el acceso a los microservicios.

2. **Enrutamiento Inteligente**  
   Ocelot dirige las solicitudes de los clientes a los microservicios correspondientes de manera eficiente.

3. **Escalabilidad y Protección**  
   Ocelot aplica **Rate Limiting** para evitar sobrecargas y proteger la plataforma contra abusos.

4. **Configuración Sencilla y Mantenimiento Centralizado**  
   Ocelot facilita la configuración y mantenimiento centralizado de rutas y reglas de seguridad.

5. **Mejora la Seguridad de la Plataforma**  
   Ocelot valida de forma centralizada los tokens JWT, garantizando el acceso seguro y autorizado a los recursos.


   # Propuesta de Estructura de Paquetes en .NET 8

La estructura de paquetes en una aplicación basada en el patrón Vertical Slice y utilizando Ocelot como API Gateway, junto con el patrón Mediator, se distribuye de la siguiente manera:


# 1. Propuesta de Estructura Y Distribucion de paquetes(Proyecto NET.8.0)

```plaintext
/ProjectName
├── /Core                   # Modelos y lógica de dominio (independiente de tecnología)
│   ├── /Entities           # Entidades del dominio (e.g., Account, Transaction)
│   ├── /ValueObjects       # Objetos de valor (e.g., Money, AccountId)
│   └── /DomainEvents       # Eventos de dominio (e.g., AccountCreated, TransactionProcessed)
├── /Application            # Capa de servicios de la aplicación
│   ├── /Commands           # Comandos para crear o modificar entidades
│   ├── /Queries            # Consultas para obtener datos
│   ├── /Handlers           # Manejadores de comandos y consultas (Patrón Mediator)
│   └── /Services           # Servicios de la aplicación (e.g., PaymentService, AccountService)
├── /Infrastructure         # Capa de infraestructura (acceso a datos, servicios externos)
│   ├── /Data               # Implementación de repositorios y acceso a base de datos
│   ├── /Messaging          # Integración con colas de mensajes (e.g., RabbitMQ, Azure Service Bus)
│   ├── /Identity           # Implementación de autenticación y autorización (OAuth, JWT)
│   ├── /API                # Implementación de Ocelot como API Gateway
│   └── /ExternalServices   # Integración con servicios externos (e.g., PaymentGateways, ThirdParty APIs)
├── /API                    # Capa de presentación, contiene controladores y rutas de API
│   ├── /Controllers        # Controladores para manejar las peticiones de los clientes
│   ├── /Filters            # Filtros para validar y modificar las peticiones/respuestas (e.g., Authorization, Logging)
│   └── /DTOs               # Objetos de transferencia de datos (Data Transfer Objects) para la comunicación con clientes
├── /Shared                 # Código común compartido entre las capas
│   ├── /Exceptions         # Excepciones personalizadas
│   ├── /Middleware         # Middleware reutilizables (e.g., Logging, Exception Handling)
│   ├── /Utilities         # Utilidades comunes (e.g., Helpers, Validaciones)
│   └── /Constants          # Constantes compartidas (e.g., Configuración global)
└── /Tests                  # Tests unitarios y de integración
    ├── /ApplicationTests   # Tests para la capa de servicios (comandos, consultas, manejadores)
    ├── /ApiTests           # Tests para los controladores y rutas de API
    └── /InfrastructureTests # Tests para la capa de infraestructura (repositorios, integración con servicios externos)
```

# Explicación de la Estructura

1. **/Core**  
   Contiene los elementos clave del dominio de la aplicación, como las entidades, objetos de valor y eventos de dominio.  
   Está completamente desacoplado de la infraestructura y la lógica de la aplicación, asegurando que el dominio sea independiente de las tecnologías utilizadas en la infraestructura.

2. **/Application**  
   Esta capa contiene la lógica de negocio, implementando comandos, consultas y los manejadores que ejecutan las acciones sobre el dominio.  
   Implementa el patrón Mediator, donde los controladores se comunican con esta capa para ejecutar operaciones, sin interactuar directamente con la base de datos ni otros servicios.

3. **/Infrastructure**  
   Implementa las interacciones con la infraestructura externa, como bases de datos, servicios de terceros y la configuración de Ocelot.  
   Integra servicios como RabbitMQ o Azure Service Bus para la comunicación asíncrona entre microservicios.

4. **/API**  
   Contiene los controladores de la API RESTful que exponen los endpoints de la plataforma.  
   Los controladores reciben las peticiones, las validan y delegan las acciones correspondientes a la capa de Application.

5. **/Shared**  
   Contiene utilidades comunes y middleware utilizadas por otras capas, como excepciones personalizadas, filtros y helpers.  
   Este paquete facilita la reutilización de código y mantiene la consistencia en todo el proyecto.

6. **/Tests**  
   Esta carpeta incluye los tests unitarios y de integración para cada capa de la aplicación.  
   Garantiza la calidad del código y valida que la API cumpla con los requisitos funcionales y de seguridad.



# Implementación de Ocelot

Dentro de la capa **/Infrastructure/API**, implementaremos **Ocelot** como el API Gateway para gestionar el enrutamiento, la autenticación y la autorización de las solicitudes.  
Ocelot permite la integración centralizada de seguridad, rate limiting y logging, y enruta las solicitudes de los clientes hacia los microservicios correspondientes.


# Justificación del Diseño

Este enfoque basado en **Vertical Slice** permite tratar cada funcionalidad del sistema como un conjunto autónomo de características, fáciles de escalar, modificar y mantener.  
El patrón Mediator separa la lógica de la infraestructura, facilitando la expansión y evolución de la plataforma sin crear dependencias fuertes entre capas.

 **Ocelot** actúa como una capa centralizada de seguridad y enrutamiento, simplificando el control de acceso y permitiendo manejar políticas como Rate Limiting y Autenticación OAuth 2.0 de manera coherente.  
Este diseño modular y escalable garantiza que la plataforma financiera sea flexible, segura y fácil de mantener a medida que crece e integra nuevos servicios.


# Atributos de Calidad y Acuerdos de Desarrollo para la API RESTful Financiera

## 1. Atributos de Calidad

Los atributos de calidad definen cómo se espera que la API cumpla con los requisitos no funcionales, garantizando seguridad, rendimiento y mantenibilidad:

### **1.1 Seguridad**
- **Autenticación y Autorización:** OAuth 2.0 con JWT para asegurar que solo usuarios y servicios autorizados accedan a la API.
- **Seguridad centralizada:** Implementada en Ocelot, asegurando coherencia en el control de acceso entre todos los microservicios.
- **Cifrado:** TLS/SSL para datos en tránsito y cifrado en reposo para datos sensibles.
- **Protección contra ataques:** Rate Limiting y políticas anti-DDoS a través de Ocelot.
- **Auditoría:** Logging y trazabilidad de transacciones financieras, cumpliendo con estándares regulatorios.

### **1.2 Escalabilidad**
- Arquitectura basada en **microservicios**, lo que permite escalar individualmente los servicios más críticos.
- Ocelot permite **enrutamiento dinámico**, facilitando la expansión de servicios sin interrupciones.
- Bases de datos distribuidas y particionadas para manejo eficiente de grandes volúmenes de transacciones.

### **1.3 Mantenibilidad**
- Aplicación de patrón **Mediator** para desacoplar controladores de la lógica de negocio.
- Uso de **Vertical Slice Architecture**, organizando la funcionalidad por características, reduciendo dependencias y complejidad.
- Capas claramente separadas: Core, Application, Infrastructure, API, Shared.

### **1.4 Disponibilidad y Fiabilidad**
- Microservicios independientes permiten tolerancia a fallos en servicios no críticos.
- Monitoreo y alertas centralizadas para detectar fallos y degradación de servicios.
- Integración con colas de mensajes (RabbitMQ, Azure Service Bus) para resiliencia y procesamiento asíncrono.

### **1.5 Consistencia y Confiabilidad de Datos**
- Implementación de transacciones ACID para operaciones críticas.
- Uso de patrones de compensación o sagas para operaciones distribuidas.
- Logs y eventos de dominio para garantizar trazabilidad de transacciones.

---

## 2. Acuerdos de Desarrollo

Los acuerdos de desarrollo aseguran que todos los equipos mantengan coherencia y calidad durante el ciclo de vida del API.

### **2.1 Convenciones de Código**
- Uso de **C# 12 / .NET 8**.
- Nomenclatura consistente: `CommandHandler`, `QueryHandler`, `DTO`, `Service`.
- Estandarización de rutas RESTful y códigos de respuesta HTTP.
- Documentación de endpoints con **Swagger / OpenAPI**.

### **2.2 Versionamiento**
- Versionamiento semántico de la API (`v1`, `v2`).
- Compatibilidad hacia atrás garantizada para clientes existentes.

### **2.3 Pruebas y Calidad**
- **Pruebas unitarias**: Cada handler de MediatR debe estar cubierto por tests.
- **Pruebas de integración**: Validar el flujo completo desde la API hasta la base de datos y servicios externos.
- **CI/CD**: Integración continua con pipelines que incluyen linting, tests y análisis de seguridad.

### **2.4 Gestión de Dependencias**
- Paquetes NuGet versionados y revisados regularmente (ej: MediatR, Ocelot).
- Evitar dependencias circulares entre capas.
- Uso de **Inyección de Dependencias (DI)** para desacoplar servicios y facilitar pruebas.

### **2.5 Seguridad y Cumplimiento**
- Revisión de código para asegurar prácticas seguras.
- Cumplimiento de estándares financieros (PCI-DSS si aplica) y normativas locales.
- Configuración de auditoría centralizada para trazabilidad.

### **2.6 Despliegue y Operación**
- Contenerización de servicios con **Docker** para facilitar despliegues escalables.
- Configuración de Ocelot para manejo de **enrutamiento, autenticación y rate limiting** de manera centralizada.
- Logging centralizado y monitoreo (Ej: ELK Stack, Prometheus, Grafana).


"La combinación de estos atributos de calidad y acuerdos de desarrollo garantiza que la API RESTful financiera sea segura, escalable, mantenible y confiable, cumpliendo con los estándares de la industria y facilitando la integración de terceros de manera controlada y eficiente."


