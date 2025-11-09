## Módulo 11 - Soft Skills para roles DevOps senior


### Pregunta de desarrollo:
### 41. ¿Por qué es importante adaptar el lenguaje técnico al comunicarse con stakeholders no técnicos?

Es crucial porque la comunicación efectiva con stakeholders es una competencia estratégica clave.
* Superar barreras de conocimiento: No todos los stakeholders (ej. líderes de negocio, gerencias, finanzas) manejan conceptos técnicos como pods, pipelines o mTLS.
* Traducción a valor empresarial: El lenguaje debe traducir los términos técnicos a términos comprensibles, relevantes y accionables. En lugar de enfocarse en tareas, la comunicación debe enfocarse en impactos (orientación a valor), como "reducir el tiempo de carga de dashboards críticos" en lugar de "optimizar queries en Prometheus".
* Alineación y toma de decisiones: Los stakeholders necesitan claridad, transparencia y contexto adecuado para tomar decisiones, justificar inversiones y alinear prioridades. Se debe usar métricas de negocio, como DORA metrics, MTTR o ahorros estimados, como soporte del relato.

---

### 42. ¿Qué elementos clave debe tener una estrategia de gestión del cambio en un proyecto tecnológico?

En entornos DevOps, la gestión del cambio debe ser ágil, segura y automatizada.
* Flujos automatizados y declarativos: El cambio debe gestionarse mediante Infraestructura como Código (IaC) y GitOps, donde toda modificación es versionada y rastreada.
* Validación y políticas: Las validaciones técnicas deben estar embebidas en los pipelines (pruebas, escaneos de seguridad). Se deben usar aprobaciones dinámicas basadas en política y riesgo, en lugar de revisiones manuales lentas.
* Despliegues progresivos y reversibles: Usar estrategias como blue/green, canary o feature flags para introducir cambios gradualmente, monitorear su comportamiento y poder deshacerlos si hay degradación.
* Auditoría y trazabilidad: Documentación automática de cada cambio: autor, propósito, estado previo y posterior, y métricas de impacto.
* Categorización del cambio: Definir niveles de cambio (estándar, normal, urgente) y su tratamiento respectivo para optimizar la gobernanza sin frenar la entrega continua.

---

### Pregunta de integración y reflexión:

### 43. ¿Cómo influye el liderazgo técnico en la aceptación de un cambio tecnológico dentro de un equipo o una organización?

El líder técnico no necesariamente tiene autoridad jerárquica, pero sí influencia decisiva sobre arquitectura y procesos.
* Generación de confianza: El líder técnico tiene el dominio técnico profundo para evaluar tecnologías y guiar al equipo en decisiones complejas. Esto refuerza la confianza en la necesidad y viabilidad del cambio.
* Facilitación y bridge: Actúa como puente entre desarrollo, operaciones y negocio, traduciendo la necesidad del cambio en una solución viable.
* Mentoría activa: El líder forma y transfiere conocimiento (Mentor), elevando el nivel técnico del equipo para que puedan adoptar la nueva práctica (ej. GitOps, SRE).
* Gestión del desacuerdo: Sabe gestionar desacuerdos técnicos de forma constructiva y justificar con claridad la nueva visión, defendiendo la calidad técnica y la escalabilidad.

---

### 44. ¿Qué estrategias usarías para mantener alineados a distintos stakeholders durante un cambio que afecta la infraestructura o procesos técnicos?

La alineación de stakeholders (negocio, finanzas, producto, seguridad) requiere:
* Adaptación y orientación a Valor: Adaptar el lenguaje técnico y enfocar la comunicación en el valor y el impacto en el negocio (ej. reducción de MTTR, mejora de disponibilidad).
* Transparencia consistente: Usar métricas clave (DORA, MTTR, ahorros FinOps) como soporte del relato. Comunicar tanto logros como riesgos, limitaciones o deudas técnicas de forma proactiva.
* Canales de comunicación Estratégicos: Utilizar dashboards ejecutivos (Grafana, PowerBI), reportes técnicos periódicos, y documentación viva (Backstage/TechDocs) según la audiencia y la complejidad del mensaje.
* Gestión de expectativas: Gestionar expectativas realistas sobre tiempos de entrega y el impacto operacional de las nuevas prácticas (ej. canary releases). La confianza se construye con consistencia y honestidad.

---


[Regresar](./2-%20Evaluación%20Final%20-%20Respuestas.md)

---
