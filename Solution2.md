# 2) Propuesta de Migración de Aplicación Monolítica a la Nube Pública

## Planteamiento del Problema


El Banco cuenta con una aplicación monolítica alojada actualmente **on-premise** que soporta grandes volúmenes de datos y múltiples conexiones concurrentes. La institución busca trasladar esta aplicación a la **nube pública** para mejorar la disponibilidad, garantizar la recuperación ante desastres y optimizar costos, sin necesidad de reconstruirla completamente desde cero.

---
## Propuesta de Solución
Mi propuesta para dar cumplimiento a la solución planteada consiste en:

## 1. Enfoque de Migración

Se propone realizar una **migración “lift-and-shift”**, trasladando el monolito completo a la nube tal como está. Esto implica:

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

