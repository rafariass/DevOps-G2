## Módulo 04 - Observabilidad Avanzada

### Pregunta de desarrollo:

### 13. ¿Qué papel cumple OpenTelemetry en un sistema distribuido y cómo se relaciona con Jaeger o Tempo?

* Papel de OpenTelemetry (OTel): OpenTelemetry es un estándar abierto y modular para la instrumentación de aplicaciones, cuyo propósito es capturar métricas, trazas y logs en una arquitectura distribuida. OTel proporciona los SDKs por lenguaje y un Collector que recibe y procesa los datos. Su papel es crucial para lograr la observabilidad completa.
* Relación con Jaeger o Tempo: Jaeger y Tempo son herramientas de Distributed Tracing (Trazabilidad Distribuida). OTel se relaciona con ellos al servir como la capa de instrumentación universal que exporta los datos de trazas (spans) a estos backends.
* Jaeger: Herramienta de trazabilidad que visualiza cada trace para analizar latencias y errores.
* Tempo: Solución de tracing de Grafana Labs, conocida por su alta escalabilidad y perfecta integración con Grafana y Loki.

---
### 14. ¿Cuál es la diferencia entre Prometheus y Grafana y cómo trabajan juntos en una solución de observabilidad?

* **Diferencias:**

| Herramienta | Función Principal | Arquitectura |
| ---- | ---- | ---- | 
| Prometheus | Recolección de métricas de series temporales. | Sistema de monitoreo pull-based que almacena datos en su TSDB. Incluye Alertmanager para la gestión de alertas. |
| Grafana | Visualización avanzada y plataforma de observabilidad. | Frontend multiplataforma y extensible que soporta más de 50 fuentes de datos. |

* **Trabajo en conjunto:**
Prometheus y Grafana forman el estándar de monitoreo moderno. Prometheus es la fuente de datos (Data Source) para las métricas. Grafana es la herramienta de visualización que consulta los datos de Prometheus mediante el lenguaje PromQL para crear dashboards temáticos. Esta combinación es esencial para el monitoreo proactivo y la trazabilidad completa del sistema.

---
### Pregunta de integración y reflexión:

### 15. ¿Qué ventajas ofrece una solución de observabilidad completa (métricas, trazas, logs) frente a sistemas de monitoreo tradicionales?
La observabilidad completa (basada en Métricas, Logs estructurados y Trazas distribuidas) permite entender no solo qué está fallando, sino por qué y dónde, a diferencia del monitoreo tradicional, que solo ofrecía una vista superficial.
Ventajas Clave:
* Diagnóstico de alta precisión: La integración de los tres pilares permite diagnósticos de alta precisión en sistemas distribuidos complejos.
* Análisis de causa raíz (Troubleshooting): El Distributed Tracing (trazas) permite seguir una única solicitud a través de múltiples servicios, midiendo latencias y detectando cuellos de botella, lo cual es imposible solo con métricas.
* Correlación de eventos: Facilita la correlación de métricas, logs y trazas para análisis forense o postmortem.
* Toma de decisiones informada: Permite a los equipos responder con base en evidencia observable, y no en suposiciones.

---
### 16. ¿Cómo influye la trazabilidad distribuida en la toma de decisiones técnicas dentro de equipos DevOps o SRE?

La trazabilidad distribuida permite a los equipos DevOps y SRE:
* Optimización de rendimiento: Analizar la latencia real por segmento y servicio (span). Esto permite identificar los cuellos de botella y rastrear demoras en servicios críticos para enfocar esfuerzos de optimización.
* Priorización de fallas: Al visualizar la ruta completa de una transacción, se pueden detectar fallas esporádicas asociadas a componentes específicos.
* Validación de arquitectura: Correlacionar el comportamiento de los servicios para validar si un cambio de arquitectura o despliegue impactó negativamente el rendimiento (por ejemplo, relacionar trazas con eventos de despliegue).
* Mejora de la capacidad de respuesta: Provee contexto inmediato para el diagnóstico, reduciendo así el Time to Diagnose y el MTTR.

---

[Regresar](./2-%20Evaluación%20Final%20-%20Respuestas.md)

---
