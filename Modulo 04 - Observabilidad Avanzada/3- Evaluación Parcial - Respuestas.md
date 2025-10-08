# CURSO: DEVOPS SENIOR

## Integrantes
* Raul Farías
* Leonardo Oyarzun
* Sebastían Rivera
* Gabriel Castillo

# Módulo 4: Observabilidad Avanzada

## Evaluación Parcial

## 1. ¿Cuál es la diferencia entre monitoreo y observabilidad?

Podríamos considerar que el monitoreo se enfoca en revisar si algo conocido está fallando con procesos manuales y herramientas aisladas, la observabilidad va más allá nos permite entender donde y porque de la falla, usando servicios integrados para la obtención de métricas, logs y trazas.

---

## 2. ¿Qué rol cumple Prometheus en un sistema observable?

Extrae métricas desde endpoint definidos, en un formato de Prometheus exposition, esto lo hace mediante un motor de extracción de datos, almacena y expone métricas, tiene un administrador de alertas y un motor especializado en almacenamiento de series temporales.

---

## 3.  ¿Para qué se utiliza Grafana y cómo se relaciona con Prometheus?

Grafana se usa para la observabilidad y visualización multiplataforma, es altamente extensible mediante plugins y dashboards personalizados. Prometheus es una fuente de datos nativa de Grafana.

---

## 4.  ¿Qué es OpenTelemetry y por qué es relevante en entornos distribuidos?

Es un estándar abierto para la instrumentación de aplicaciones, en una arquitectura distribuida es capaz de capturar métricas, trazas y logs. Está soportado por múltiples SDKs, tiene un collector para procesas y distribuir información a múltiples backends.

---

## 5. ¿Cuál es la utilidad de una traza distribuida en el diagnóstico de errores?

Permite seguir una única solicitud a través de múltiples servicios, midiendo latencias, errores, cuellos de botella y dependencia entre componentes. Por ejemplo, rastrear demoras en servicios o detectar fallas esporádicas asociadas a componentes específicos.

---

## 6. ¿En qué casos usarías Jaeger o Tempo y cuál es su diferencia principal?

Ambas herramientas se alimentan de OpenTelemetry asegurando que las aplicaciones capturen trazas necesarias para ser consumidas por cualquiera de los backends. Las usaría para rastrear demoras, diagnosticar errores o análisis forenses (por la capacidad de correlacionar métricas, logs y trazas). También para observabilidad. Su principal diferencia radica en la arquitectura de almacenamiento, por ejemplo Tempo no quiere base de datos, utilizas servicios de almacenamiento de objetos (S3, GCS, entre otros) y otro punto es el enfoque operativo, por ejemplo Jaeger integra formatos como OpenTraicing, OpenTelemetry, Zipkin y Tempo tiene perfecta integración con Grafana y Loki (logs).

---

## 7. ¿Qué ventajas ofrece el uso conjunto de Prometheus, Grafana y OpenTelemetry en una arquitectura moderna?

Ofrece ventajas críticas en una arquitectura moderna, este stack es la base para lograr la observabilidad integral del sistema, las principales ventajas serían, observabilidad integrada y unificada, diagnóstico eficiente, integración en el ecosistema devops avanzado.

---
