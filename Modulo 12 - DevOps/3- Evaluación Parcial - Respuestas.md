# CURSO: DEVOPS SENIOR

## Integrantes
* Raul Farías
* Leonardo Oyarzun
* Sebastían Rivera
* Gabriel Castillo

# Módulo 12 - DevOps

## Evaluación Parcial

### 1. ¿Qué objetivos cumple dividir un proyecto técnico en sprints iterativos?

Dividir el proyecto en **sprints** (como se detalla en el documento: Sprint 1, 2 y 3) cumple objetivos clave, especialmente en un entorno de simulación empresarial realista.

  * **Entrega de Valor Temprana e Incremental:** Permite entregar funcionalidades y valor de negocio en bloques pequeños y frecuentes.
  * **Gestión de Riesgos y Adaptación (Agilidad):** Facilita la identificación temprana de problemas, la corrección de estructura (como se espera en el Sprint 1), y la adaptación a nuevos requisitos o restricciones de contexto comunicados entre sprints.
  * **Retroalimentación Continua:** Cada sprint incluye un ciclo de retroalimentación específico ("Retroalimentación esperada"), lo que asegura que el trabajo se alinee con las expectativas de los stakeholders simulados (CTO, Líder de Operaciones, Gerente de Producto) y con los estándares de seguridad y observabilidad.
  * **Visibilidad y Control:** Ofrece a los stakeholders una visibilidad clara del avance técnico y del **nivel de realismo**, permitiendo ajustes estratégicos antes de la finalización.

---

### 2. ¿Cómo aplicaría GitOps para garantizar trazabilidad y control de cambios en el entorno de producción?

El proyecto ya incluye la integración **GitOps** (usando herramientas como ArgoCD o FluxCD) como entregable del Sprint 1. Para asegurar **trazabilidad y control de cambios**:

  1.  **"Source of Truth" Única (Single Source of Truth - SSOT):** Definir el repositorio Git como la **única fuente de verdad** para la configuración y el estado deseado del entorno de producción.
  2.  **Mecanismo de Pull:** Usar un *controller* de GitOps (ej. ArgoCD o FluxCD) que **activamente monitorea** el repositorio Git. Cualquier cambio en la configuración (ej. manifiestos de Kubernetes) debe hacerse *solo* en el repositorio.
  3.  **Trazabilidad con Conventional Commits:** Se utilizaría el estándar **Conventional Commits**, forzando una estructura de mensaje que describe la naturaleza del cambio (`feat`, `fix`, `chore`, etc.). Esto vincula cada cambio de producción con un *commit* específico, un autor y una justificación clara.
      * **Ejemplo (ES):** `feat(k8s): Aumentar réplicas del deployment 'api-gateway' de 3 a 5 para escalabilidad`
      * **Ejemplo (EN):** `feat(k8s): Increase 'api-gateway' deployment replicas from 3 to 5 for scalability`
  4.  **Inmutabilidad y Reversión:** Dado que el cambio se aplica desde Git, la **reversión** es inherentemente simple: basta con revertir el commit o hacer *rollback* al estado anterior conocido en Git. La inmutabilidad del clúster (el controlador de GitOps corrige cualquier desviación) garantiza que el estado real siempre coincida con el *SSOT*.

---

### 3. ¿Qué herramientas implementaría para observar el comportamiento de una solución DevOps AI-Driven en tiempo real?

Para una solución **AI-Driven DevOps** (AIOps), se requiere un **Stack de Observabilidad** robusto que cubra las tres pilares de la observabilidad (**Logs**, **Metrics**, **Traces**) y lo integre con una herramienta de AIOps.

| Pilar                                        | Herramientas (Ejemplos del Doc)                           | Función                                                                                                                                                                                                             |
| -------------------------------------------- | --------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Métricas**                                 | **Prometheus** (Recolección), **Grafana** (Visualización) | Recolección de series de tiempo (CPU, memoria, latencia). Clave para el monitoreo de rendimiento y **FinOps** (OpenCost).                                                                                           |
| **Trazas (Traces)**                          | **Jaeger** o **Tempo**                                    | Seguimiento de una solicitud a través de múltiples microservicios. Esencial para diagnosticar la **respuesta lenta ante incidentes** y reducir el **MTTR** (Mean Time To Recovery).                                 |
| **Logs**                                     | Elastic Stack (ELK) o Loki                                | Captura de eventos estructurados de las aplicaciones y la infraestructura.                                                                                                                                          |
| **Correlación/AIOps**                        | **Moogsoft** o **PagerDuty AIOps**                        | **Correlaciona** alertas y eventos de los pilares de observabilidad para reducir el "ruido" (alert fatigue), identificar la causa raíz (Root Cause Analysis - RCA) y sugerir **auto-remediación** en tiempo real.   |

---

### 4. ¿Cómo manejaría la incorporación de feedback de stakeholders técnicos y no técnicos en medio del proyecto?

La clave es la **comunicación diferenciada** y el **registro estructurado de decisiones técnicas**.

  * **Stakeholders Técnicos (CTO simulado, Líder de Operaciones simulado, Equipo Instructor):**
      * **Mecanismo:** El feedback se maneja en las retroalimentaciones específicas de Sprint (ej. revisión de **buenas prácticas de seguridad**, **análisis de desempeño**).
      * **Formato:** Se requiere una **bitácora de decisiones** y un **Registro de Decisiones Técnicas** (utilizando **ADRs** - Architecture Decision Records) para documentar el **por qué** y el **cómo** de las soluciones técnicas adoptadas, asegurando la trazabilidad.

  * **Stakeholders No Técnicos (Gerente de Producto simulado, Comité Directivo):**
      * **Mecanismo:** El feedback se centra en el valor de negocio, la claridad y los entregables de gobernanza y costos (FinOps).
      * **Formato:** Usar el **board Kanban** para simular el flujo ágil y traducir el progreso técnico a hitos de negocio comprensibles. El feedback del Gerente de Producto se enfoca en la **claridad y el valor**, no en el código.

---

### 5. ¿Qué estrategia usaría para presentar resultados técnicos a un comité directivo no técnico?

La estrategia debe centrarse en la **traducción del valor técnico a métricas de negocio**, tal como se espera en el Sprint 3 con la **Presentación Ejecutiva**.

  * **Elaboración del Informe Ejecutivo:** Crear un **Informe ejecutivo de proyecto** que coloque los **Anexos Técnicos** al final. La parte principal debe enfocarse en:
      1.  **Problemas Resueltos (Dolores de Negocio):** Conectar la solución directamente con los problemas iniciales (escalabilidad, respuesta lenta ante incidentes, altos costos).
      2.  **Métricas Clave (KPIs):** Usar **KPIs** (Key Performance Indicators - Indicadores Clave de Rendimiento) que resuenen con la junta (ej. Reducción de Costos en la Nube, Reducción de MTTR, Aumento de Tasa de Despliegue).
      3.  **Visualización Estratégica:** Utilizar **Grafana** (o *mockups*) para presentar *dashboards* de **FinOps** y **Observabilidad** que muestren el impacto de la solución en la eficiencia y la resiliencia.

  * **Estrategia de Comunicación (Video de Presentación):** El video de máximo 10 minutos debe seguir la regla de oro: **Comenzar con el "Por Qué" (Valor) y el "Qué" (Resultados), antes del "Cómo" (Tecnología).** Esto demuestra **liderazgo técnico** al alinear la tecnología con los objetivos de negocio.

---

### 6. ¿Cómo aseguraría que su solución sea resiliente ante fallos e incidentes inesperados?

La resiliencia se construye en múltiples capas, muchas de las cuales son entregables directos del proyecto:

  * **Infraestructura como Código (IaC):** Utilizando Terraform (o similar), la infraestructura debe ser **inmutable** y **re-desplegable**. Esto permite la **auto-remediación** rápida al garantizar que los componentes pueden ser destruidos y recreados a partir de una configuración validada.
  * **Diseño Cloud Native:** El diseño debe basarse en patrones de **alta disponibilidad (HA)**, **autoescalado** (para resolver el problema de escalabilidad) y **tolerancia a fallos** (ej. *ReplicaSets* de Kubernetes).
  * **Auto-Remediación (AIOps):** Implementar **políticas de auto-remediación** (ej. un *webhook* que reinicia un *pod* automáticamente al detectar alta latencia correlacionada) basado en las alertas del sistema **AIOps**.
  * **Validación de Seguridad y Performance:** El Sprint 3 incluye una **Validación final de seguridad, performance y eficiencia**, que asegura que la solución final cumple con los requisitos de robustez antes de la entrega ejecutiva.

---

### 7. ¿Qué indicadores clave (KPIs) utilizaría para evaluar el éxito de su proyecto integrador DevOps?

Basado en los desafíos iniciales (escalabilidad, respuesta lenta, altos costos, falta de visibilidad), los **KPIs** (Key Performance Indicators - **I**ndicadores **C**lave de **R**endimiento) se centrarían en métricas de rendimiento de la solución, costos y eficiencia operativa (basado en el marco **DORA** y **FinOps**):

| KPI                                                                                        | Métrica a Medir                                                                                               | Relación con el Desafío Inicial                                                                                   |
| ------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| **MTTR** (Mean Time To Recovery - **T**iempo **P**romedio de **R**ecuperación)             | Tiempo promedio desde la detección de un incidente hasta la restauración del servicio.                        | Aborda la **respuesta lenta ante incidentes**.                                                                    |
| **Coste por Unidad (FinOps)**                                                              | Coste de infraestructura normalizado por unidad de negocio (ej. por usuario, por transacción).                | Mide la efectividad de las prácticas de **FinOps** para reducir los **altos costos en la nube**.                  |
| **Frecuencia de Despliegue**                                                               | Cantidad de veces que una organización despliega código a producción.                                         | Mide la madurez del **CI/CD** y la **agilidad** del equipo.                                                       |
| **SLO Compliance** (Service Level Objective - **O**bjetivo de **N**ivel de **S**ervicio)   | Porcentaje de tiempo que la latencia/disponibilidad del servicio se mantiene dentro de los límites definidos. | Aborda la **escalabilidad** y la **falta de visibilidad operativa** (a través de la **observabilidad**).          |

---
