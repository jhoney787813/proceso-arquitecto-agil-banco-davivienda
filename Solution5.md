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
