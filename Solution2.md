# 2) Propuesta de Migración de Aplicación Monolítica a la Nube Pública

## Planteamiento del Problema


El Banco cuenta con una aplicación monolítica alojada actualmente **on-premise** que soporta grandes volúmenes de datos y múltiples conexiones concurrentes. La institución busca trasladar esta aplicación a la **nube pública** para mejorar la disponibilidad, garantizar la recuperación ante desastres y optimizar costos, sin necesidad de reconstruirla completamente desde cero.

---
## Propuesta de Solución
Mi propuesta para dar cumplimiento a la solución planteada consiste en:

## 1. Enfoque de Migración

Se propone realizar una migración **“desplezar y levantar servicios”**, trasladando el monolito completo a la nube tal como está. Esto implica:

- Mantener la aplicación existente sin modificar su estructura interna.
- Ejecutarla en servidores en la nube, utilizando recursos escalables según demanda.
- Minimizar riesgos y tiempo de implementación, evitando la reescritura completa del sistema.

---

## 2. Componentes de la Solución

### Alta Disponibilidad
- Desplegar la aplicación en **múltiples instancias** en diferentes zonas de disponibilidad dentro de la nube.
- Usar un **balanceador de carga** que distribuya el tráfico y asegure continuidad ante fallas de algún servidor.

### Recuperación ante Desastres

- Mantener una **copia espejo** de la aplicación y la base de datos en otra región de la nube.
- Permitir activación rápida de la copia espejo en caso de fallo total de la región principal.

### Persistencia de Datos

- Implementar **bases de datos replicadas** y respaldos automáticos.
- Garantizar que los datos estén siempre disponibles y consistentes, incluso en situaciones críticas.

### Eficiencia en Costos

- No se requiere reconstrucción del sistema, reduciendo gastos de desarrollo.
- Pagar solo por los recursos utilizados en la nube y ajustar capacidad según la demanda.
- La réplica para recuperación ante desastres tiene un costo adicional, pero evita pérdidas mucho mayores en caso de fallo.

---

## 4. Arquitectura Conceptual

# Arquitectura de Migración del Monolito a la Nube

<img width="1760" height="863" alt="image" src="https://github.com/user-attachments/assets/32b8b800-430c-401a-8086-6457379916e1" />

**Explicación:**
- Los usuarios acceden a la aplicación a través de un balanceador que distribuye la carga entre varias instancias.
- La base de datos está replicada para asegurar disponibilidad y consistencia de datos.
- Existe una copia espejo en otra región de la nube para garantizar continuidad del negocio ante desastres.

**Componentes**
1. **Cliente / Usuario**: usuario final que solicita operaciones bancarias.  
2. **Balanceador de carga**: distribuye las solicitudes entre Servidor 1 y Servidor 2.  
3. **Servidores de aplicación (Monolito)**: mantienen la aplicación existente sin necesidad de reescribirla.  
4. **Módulos internos**:  
   - **Negocio**: lógica central de reglas de negocio bancaria  1 a N.
5. **Base de datos replicada**: asegura alta disponibilidad y consistencia de los datos.  
6. **Región de respaldo**: copia espejo para recuperación ante desastres.  





---

## 5. Beneficios de la Propuesta

1. **Rápida implementación:** La aplicación se traslada tal como está, sin necesidad de rediseño.
2. **Alta disponibilidad:** Las instancias múltiples y el balanceo de carga aseguran que el sistema permanezca operativo.
3. **Recuperación ante desastres:** La copia espejo protege frente a fallas graves.
4. **Optimización de costos:** Pago por uso de recursos y capacidad escalable según demanda, evitando inversiones innecesarias.
5. **Bajo riesgo:** Se minimiza el impacto de migración al mantener el funcionamiento actual de la aplicación.

---

 
Considero a alto nivel que esta estrategia permite al Banco aprovechar los beneficios de la nube (escalabilidad, resiliencia y costos controlados) sin alterar el sistema existente, garantizando continuidad del negocio y preparación para futuras mejoras. **"TIEMPOS MENORES DE IMPLEMENTACIÓN"**, a comparación si los desarrollaramos como Microservicios o capacidades independientes.

