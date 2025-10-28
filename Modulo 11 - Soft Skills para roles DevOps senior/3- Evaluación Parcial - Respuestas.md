# CURSO: DEVOPS SENIOR

## Integrantes
* Raul Farías
* Leonardo Oyarzun
* Sebastían Rivera
* Gabriel Castillo

# Módulo 11 - Soft Skills para roles DevOps senior

## Evaluación Parcial

## 1. ¿Qué se entiende por stakeholder en un proyecto técnico?

En el contexto DevOps, los **stakeholders** (partes interesadas) son todas las partes, internas o externas, que **influyen, se ven afectadas o participan en la toma de decisiones** sobre los productos, plataformas o servicios que se desarrollan y operan.

Estos incluyen comúnmente:

  * **Líderes de negocio:** Gerencias, direcciones, CEO, CPO.
  * **Equipos de producto:** *Product managers*, *owners*, diseñadores UX.
  * **Áreas de cumplimiento y seguridad:** Legal, ciberseguridad, auditoría.
  * **Clientes:** Internos o externos, como usuarios de APIs, portales o integraciones.
  * **Equipos transversales:** QA, analítica, ventas, soporte técnico.
  * **Finanzas:** Responsables de presupuesto, FinOps.
  * **Comités estratégicos o *sponsors* del proyecto**.

Cada uno de ellos requiere un nivel diferente de profundidad técnica, pero todos necesitan **claridad, transparencia y contexto** para tomar decisiones.

---

## 2. ¿Por qué es importante adaptar el lenguaje técnico al comunicar con stakeholders no técnicos?

Es crucial adaptar el lenguaje para **alinear expectativas, reducir fricciones, gestionar prioridades, justificar inversiones y fortalecer la confianza** entre los equipos técnicos y las unidades de negocio.
La clave es **traducir el lenguaje técnico a términos comprensibles, relevantes y accionables**, sin perder precisión o impacto. Esto se basa en los siguientes principios:

  * **Adaptar el lenguaje:** Traducir términos técnicos (*pods, pipelines, mTLS*) en función del **valor** que representan. Por ejemplo, en lugar de "deploy automático en GitOps", se explica como la "capacidad de liberar versiones de forma segura sin intervención manual, reduciendo errores y acelerando el *time-to-market*".
  * **Orientación a valor:** Enfocarse en los **impactos** de las acciones técnicas en lugar de las tareas en sí. Por ejemplo, en lugar de "optimizamos las *queries* en Prometheus", se comunica "reducimos el tiempo de carga de *dashboards* críticos en 45%, mejorando la toma de decisiones en tiempo real".

---

## 3. ¿Qué habilidades caracterizan a un líder técnico efectivo?

Un líder técnico efectivo posee las siguientes competencias clave:

  * **Dominio técnico profundo:** En sus áreas específicas (Kubernetes, GitOps, IaC, seguridad, CI/CD).
  * **Pensamiento sistémico:** Para comprender las interdependencias entre servicios.
  * **Capacidad analítica:** Para evaluar *trade-offs* técnicos y anticipar impactos.
  * **Comunicación efectiva:** Tanto con perfiles técnicos como con *stakeholders* no técnicos.
  * **Mentoría activa:** Para formar al equipo y sostener el crecimiento técnico colectivo.
  * **Apertura al *feedback* y mejora continua:** Evitando la rigidez o el dogmatismo.

El líder técnico es un **facilitador de la excelencia técnica compartida**, no un "gurú solitario".

---

## 4. ¿Cómo puede un líder técnico generar confianza durante una transformación organizacional?

Aunque el documento no tiene una sección específica sobre "transformación organizacional", el liderazgo técnico genera confianza a través de:

  * **Transparencia y sinceridad:** Comunicando tanto logros como **riesgos, limitaciones o deudas técnicas** de forma proactiva, no solo resultados exitosos.
  * **Consistencia y honestidad:** Gestionando expectativas realistas sobre tiempos de entrega, disponibilidad (*SLAs*) y limitaciones técnicas. La confianza se construye con **consistencia, honestidad y evidencia**.
  * **Trazabilidad y métricas:** Usando indicadores clave (*DORA metrics, MTTR, ahorros estimados, OpenCost*) para **soportar el relato** y justificar las decisiones.
  * **Defensa de la calidad y visión de largo plazo:** El líder actúa como una "brújula" que **defiende la calidad técnica**, piensa en escalabilidad, y ve más allá del *sprint* actual, conectando tecnología, negocio y personas.

---

## 5. ¿Qué es un mapa de stakeholders y cómo se utiliza?

El documento no define ni describe explícitamente un **mapa de stakeholders** (en inglés: *Stakeholder Map*).

Sin embargo, sí identifica quiénes son los *stakeholders* en entornos DevOps (líderes de negocio, equipos de producto, seguridad, etc.) y enfatiza que **cada uno requiere un nivel distinto de profundidad técnica**, pero todos necesitan claridad y contexto adecuado para tomar decisiones. Esto sugiere implícitamente la necesidad de clasificar a estas partes interesadas para **seleccionar el canal y adaptar el mensaje estratégicamente** según la audiencia, urgencia y complejidad.

---

## 6. ¿Qué elementos deben considerarse para una comunicación efectiva del cambio?

En el contexto de la **Gestión de Cambio en DevOps**, la comunicación efectiva se logra mediante:

  * **Automatización y versionamiento:** La gestión del cambio se basa en **mecanismos programáticos, confiables y auditables** que se integran al flujo DevOps.
  * **Trazabilidad y auditoría completa:** Cada cambio debe documentarse automáticamente, incluyendo: **autor, propósito, fecha, estado previo y posterior, *logs* de ejecución, validaciones y métricas de impacto**.
  * **Transparencia con herramientas:** Usar **observabilidad post-despliegue** para medir el impacto real, y **herramientas de gestión de *tickets* y seguimiento** (como Jira o ServiceNow) con trazabilidad.
  * **Comunicación de riesgos y mitigación:** Los **cambios urgentes sin documentación** deben registrarse y evaluarse en *postmortems*, y la **falta de *rollback* automático** debe mitigarse implementando despliegues reversibles y pruebas de validación.
  * **Impacto Estratégico:** La comunicación debe resaltar cómo la gestión de cambio reduce incidentes, mejora el *time-to-market* y aumenta la **transparencia entre equipos técnicos y áreas de negocio**.

---

## 7. ¿Cuál es el rol del liderazgo técnico en la gestión del cambio?

El rol del liderazgo técnico en la gestión del cambio es fundamentalmente **habilitador y garante de la calidad**.

  1.  **Definición de estándares:** Definir estándares de **IaC (Infraestructura como Código)**, **CI/CD** (Integración y Despliegue Continuos), seguridad y monitoreo, que son la base de un cambio seguro y automatizado.
  2.  **Abogar por la automatización y calidad:**
      * Promueve la **automatización con propósito**, evitando modas.
      * **Eleva la calidad del código y de la infraestructura**.
      * Sostiene una cultura de **postmortems** y **alertas accionables** que permiten aprender de los cambios y mitigar riesgos.
      * Facilita la adopción de prácticas como **GitOps** y **SRE** (Site Reliability Engineering), que son el motor de la gestión de cambio moderna.
  3.  **Balancear y facilitar:**
      * Actúa como **puente** entre desarrollo, operaciones, seguridad y producto para traducir las necesidades de negocio en soluciones viables.
      * Debe **equilibrar la presión por resultados con estándares técnicos sostenibles**, y abordar la deuda técnica sin bloquear la entrega de valor, lo cual es central en la gestión de cambio.

---
