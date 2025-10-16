# CURSO: DEVOPS SENIOR

## Integrantes
* Raul Farías
* Leonardo Oyarzun
* Sebastían Rivera
* Gabriel Castillo

# Módulo 8 - Platform Engineering & Internal Developer Plataforms (IDP)

## Evaluación Parcial

### 1. ¿Qué es Backstage y qué problemas busca resolver?

**Backstage** es una plataforma de desarrollo interno (IDP: Internal Developer Platform) de código abierto, creada por Spotify y mantenida por la Cloud Native Computing Foundation (CNCF).

Busca resolver los siguientes problemas:
* **Centralización de herramientas**: Su propósito es unificar herramientas, documentación, servicios y flujos DevOps en una única interfaz.
* **Reducción de la carga cognitiva**: Permite que los desarrolladores se concentren en escribir código en lugar de buscar recursos, entender integraciones o lidiar con procesos manuales.
* **Estandarización y escalabilidad**: Ayuda a las organizaciones a estandarizar la experiencia de desarrollo, acelerar el *onboarding* de nuevos miembros, mejorar la trazabilidad de los servicios y escalar sus prácticas DevOps.

---

### 2. ¿Qué es un GitOps stack y cómo se relaciona con Backstage?

Un **GitOps Stack** es una agrupación lógica de recursos (como un entorno completo, un clúster de Kubernetes o un conjunto de microservicios) que se gestiona de forma declarativa desde Git como única fuente de verdad.

El documento **no describe una relación directa o una dependencia** entre GitOps Stacks y Backstage, sino que los presenta como dos componentes distintos dentro de una estrategia de Platform Engineering. Sin embargo, se menciona que el componente **Scaffolder** de Backstage puede integrarse con GitOps para generar automáticamente repositorios y configuraciones de despliegue, que luego serían gestionados como parte de un GitOps Stack.

---

### 3. ¿Qué ventajas ofrece Backstage sobre un simple README o documentación estática?

Backstage, a través de su componente **TechDocs**, ofrece una solución de documentación superior a los archivos estáticos por las siguientes razones:
* **Documentación viva y centralizada**: En lugar de estar aislada, la documentación (escrita en Markdown) se integra directamente en la interfaz de Backstage y se aloja junto al código en los repositorios.
* **Descubrimiento y contexto**: La documentación está asociada a cada componente dentro del **Software Catalog**, lo que permite acceder a ella junto con el código fuente, los pipelines y las métricas desde un único lugar.
* **Estandarización**: Facilita el establecimiento de estándares de documentación transversales en toda la organización.
* **Integración con herramientas**: Permite integrar la documentación técnica con herramientas como Swagger, OpenAPI o Postman.

---

### 4. ¿Cómo se integran pipelines de CI/CD con Backstage?

La integración de CI/CD con Backstage se realiza a través de *plugins*. Backstage no reemplaza las herramientas de CI/CD, sino que las unifica en una única interfaz.

Esta integración permite a los desarrolladores:
* **Consultar el estado de los builds, despliegues y métricas** directamente desde la interfaz de Backstage sin necesidad de cambiar de herramienta.
* Alinear los pipelines con los metadatos del **Software Catalog** para que las definiciones de CI/CD puedan ser reutilizables y estandarizadas.

---

### 5. ¿Qué papel juega la observabilidad en Backstage y cómo se visualiza?

La observabilidad es un componente clave que Backstage centraliza para ofrecer una visión completa del ciclo de vida del software. A través de *plugins*, Backstage se integra con herramientas como **Prometheus, Grafana, Sentry y Datadog**.

El objetivo de esta integración es permitir que los desarrolladores **consulten el estado de sus métricas y alertas directamente desde la interfaz de Backstage**. El documento no detalla *cómo* se visualiza esta información, pero enfatiza que Backstage actúa como un portal unificado para acceder a ella.

---

### 6. ¿Cómo se gestiona el catálogo de servicios en Backstage y qué beneficios ofrece?

El **Software Catalog** es el núcleo de Backstage y se gestiona de manera declarativa. Cada componente (servicio, librería, etc.) se representa como una entidad definida en un **archivo YAML** con metadatos estandarizados como `name`, `owner` y `type`.

Los **beneficios** que ofrece este catálogo son:
* **Descubrimiento y visibilidad**: Permite descubrir todos los servicios activos y ver qué equipo es el responsable de cada uno.
* **Trazabilidad completa**: Facilita la navegación de dependencias y centraliza el acceso al código fuente, pipelines, documentación y métricas.
* **Reducción de la fricción**: Mejora la visibilidad organizacional y simplifica el trabajo en entornos complejos.

---

### 7. ¿Qué desafíos presenta la implementación de Backstage en una organización?

El documento menciona una serie de **"Consideraciones para producción"** que representan los principales desafíos al implementar Backstage:
* **Seguridad y Acceso**: Es necesario configurar un control de autenticación y autorización (usando OIDC, OAuth o SAML) e integrarlo con DNS y certificados TLS.
* **Mantenimiento del Catálogo**: Se deben establecer flujos de revisión o *pipelines* para mantener el catálogo de servicios actualizado.
* **Escalabilidad**: Se debe planificar la escalabilidad del *backend* y elegir una base de datos adecuada para producción (como PostgreSQL en lugar de SQLite).
* **Actualizaciones**: Requiere un mantenimiento continuo de los *plugins* y del núcleo de Backstage.
* **Gobernanza**: Es crucial integrar el uso del Scaffolder con flujos de GitOps para que la creación de nuevos recursos se realice bajo revisión y control.

---
