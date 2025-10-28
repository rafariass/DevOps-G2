# CURSO: DEVOPS SENIOR

## Integrantes
* Raul Farías
* Leonardo Oyarzun
* Sebastían Rivera
* Gabriel Castillo

# Módulo 10 - AIOps & Incident MAnagement

## Evaluación Parcial

## 1. ¿Qué es AIOps y cuál es su propósito principal?

**AIOps** (Artificial Intelligence for IT Operations) es un enfoque que combina el **big data**, **algoritmos de aprendizaje automático** (Machine Learning) y la **integración operativa**.

Su **propósito principal** es automatizar la detección, correlación, priorización y respuesta ante eventos e incidentes operativos mediante inteligencia artificial y aprendizaje automático. Esto es fundamental en entornos de gran escala donde el volumen de datos de logs, métricas y trazas supera la capacidad humana de análisis en tiempo real.

---

## 2. ¿Qué ventajas ofrece Moogsoft como plataforma AIOps?

Moogsoft es una plataforma de AIOps especializada en la detección temprana de incidentes a partir de eventos masivos y dispersos. Sus ventajas centrales se basan en:

  * **Correlación automatizada de alertas**: Aplica modelos estadísticos y de aprendizaje no supervisado para agrupar eventos relacionados en "incidentes" significativos. Esto ayuda a eliminar el ruido y la duplicidad de alertas.
  * **Detección de anomalías**: Detecta cambios anómalos mediante entrenamiento continuo con datos históricos y patrones de comportamiento, incluso si no están definidos como umbrales en herramientas de monitoreo tradicionales.
  * **Workflows de remediación**: Permite automatizar respuestas, como el reinicio de pods o servicios específicos, la ejecución de scripts/playbooks predefinidos (Ansible, Terraform), la notificación a equipos y el escalamiento automático.
  * **Integración con ecosistemas DevOps**: Se conecta nativamente con herramientas como Prometheus, Datadog, Splunk, Jira, Slack, y ServiceNow, orquestando la detección y respuesta en la plataforma técnica.

---

## 3. ¿Cómo mejora PagerDuty AIOps el ciclo de respuesta ante incidentes?

PagerDuty AIOps extiende la gestión de incidentes con funcionalidades avanzadas de inteligencia artificial para reducir el **Tiempo Medio de Detección (MTTD)** y **Tiempo Medio de Resolución (MTTR)**. Lo mejora a través de:

  * **Event Intelligence**: Analiza datos en tiempo real, suprime alertas duplicadas o irrelevantes, y prioriza eventos en función del impacto y correlaciones históricas.
  * **Alert Grouping**: Agrupa automáticamente alertas con sintomatología o causalidad compartida, presentando un único incidente al operador con contexto enriquecido.
  * **Intelligent Triage**: Sugiere la probable causa raíz, provee contexto adicional (métricas, errores recientes, cambios en producción) y recomienda a la mejor persona o equipo para resolverlo.
  * **Automation Actions**: Permite ejecutar acciones automatizadas (restart, scale, mute, kill) desde la consola, soportando la integración con *runbooks* externos.
  * **Postmortem Automation**: Genera automáticamente borradores de *postmortem* con *timelines*, alertas, resoluciones y participantes, transformando el incidente en aprendizaje estructurado.

---

## 4. ¿Qué es auto-remediación y cuándo debería aplicarse?

La **auto-remediación** (*Auto Remediation*) es el proceso por el cual un sistema o plataforma corrige automáticamente una condición anómala, error o incidente, sin intervención humana directa. Es una práctica clave en entornos DevOps avanzados para reducir el tiempo de inactividad (**MTTR**), mitigar errores repetitivos y mejorar la resiliencia.

Debería aplicarse como una **capa automatizada de respuesta táctica**, siendo especialmente eficaz para **incidentes conocidos o repetitivos**.

**Ejemplos de casos de uso típicos** donde se aplica la autorremediación incluyen:
  * Reinicio automático de *pods* en *crashloop* o con fallas de salud en Kubernetes.
  * Reversión de despliegue (*rollback*) ante fallos detectados post-deploy (*canary failback*).
  * Autoescalamiento horizontal cuando la carga supera ciertos umbrales.
  * Rotación automática de secretos expirados o comprometidos.

---

## 5. ¿Qué significa MTTR y por qué es un indicador crítico en operaciones TI?

**MTTR** significa **Mean Time To Recovery** (Tiempo Medio de Recuperación). Es una métrica clave en la gestión de confiabilidad operativa.

Es un indicador **crítico** porque representa el **tiempo promedio que tarda un sistema o servicio en recuperarse tras una falla o interrupción**. La reducción de MTTR es un objetivo estratégico, ya que:
  * Indica una **mayor capacidad de respuesta**.
  * Genera un **menor impacto al usuario**.
  * Demuestra una **mayor resiliencia general del sistema**.

---

## 6. ¿Cómo ayuda la correlación de alertas a reducir la fatiga del equipo de respuesta?

La correlación de alertas es un componente fundamental de AIOps y ayuda a reducir la **fatiga de alertas** (*alert fatigue*) en los equipos de SRE, DevOps o NOC.

Ayuda al[cite: 45, 46, 69, 179]:
  * **Agrupar eventos relacionados** en un solo "incidente" significativo.
  * **Eliminar ruido y duplicidad de alertas** (por ejemplo, evitar que una falla de red genere 100 alertas separadas).
  * Presentar un **solo incidente al operador con contexto enriquecido**.
  * **Enlazar eventos que comparten una raíz común** (*root cause analysis*).

Al suprimir alertas redundantes y agrupar automáticamente las que comparten sintomatología o causalidad, se entrega **menos ruido y más contexto** al equipo, lo que se traduce en un menor tiempo de diagnóstico y mejora el bienestar del equipo de operaciones[cite: 94, 181].

---

## 7. ¿Qué riesgos existen al implementar auto-remediación sin un proceso controlado?

La autorremediación, si bien es poderosa, conlleva riesgos si no se implementa con un proceso controlado:

  * **Acciones incorrectas por mal diagnóstico**: El sistema puede apagar el servicio en lugar de corregirlo, lo que agrava la situación.
  * **Escalada de fallos**: Si el sistema actúa en cadena sin supervisión, puede provocar una escalada de fallos.
  * **Falsa sensación de seguridad**: Puede ocultar problemas estructurales del sistema al solo aplicar la corrección superficial.
  * **Dificultades en *troubleshooting***: Esto ocurre si las acciones no están adecuadamente documentadas o trazadas.
  * **Fallo en la acción de corrección**: Si el proceso no incluye validación y *fallback* (escalamiento o acción manual), la corrección podría fallar y perpetuar el problema.

Por ello, se recomienda comenzar con remediaciones **supervisadas o semi-automáticas** hasta que la lógica esté validada y estabilizada.

---
