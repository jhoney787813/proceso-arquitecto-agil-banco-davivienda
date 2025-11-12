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

