## Módulo 01 - DevOps Estratégico y GitOps

### OBJETIVO A MEDIR Y CRITERIO DE EVALUACIÓN

Identificar conceptos de **`DevOps estratégico`** y **`GitOps avanzado`**, y la evolución del **`rol DevOps`**, según las prácticas avanzadas de:
- GitOps
- DevSecOps
- Kubernetes
- Observabilidad
- IaC (Infrastructure as Code)
- FinOps
- AIOps

### CONTENIDO QUE SE REVISA (**`MÓDULO 1`**)

- DevOps moderno
- GitOps patterns
- ArgoCD avanzado
- Gestión de secretos

### OBSERVACIÓN

Las preguntas de esta sección invitan al **`análisis crítico`** y vinculan conocimientos técnicos con aplicaciones prácticas. Permiten valorar su capacidad para integrar herramientas de GitOps avanzado con criterios de seguridad, automatización y **`madurez del rol DevOps`** en escenarios empresariales complejos.

### CONTEXTO DEL CASO:

La empresa TechNova ha migrado la mayoría de sus servicios a Kubernetes y ha  adoptado un  enfoque  GitOps  para  gestionar  sus  despliegues.  Utilizan  ArgoCD  como  herramienta principal y han comenzado a integrar la gestión de secretos con Sealed Secrets y Vault.

Recientemente,  un  equipo  de  desarrollo  realizó  un  push  directo  a  la  rama  principal  del repositorio de manifiestos sin seguir el flujo de Pull Request definido por la organización. Esto desencadenó una sincronización automática en ArgoCD que desplegó una versión inestable de  un  microservicio  con  claves  expuestas  por  error.  El  incidente  afectó  temporalmente  el servicio  de  autenticación  y  levantó  dudas  en  la  dirección  técnica  sobre  los  controles implementados.

Ante la situación, el equipo DevOps ha sido convocado para presentar:

- Un análisis técnico del incidente.
- Mejoras en la arquitectura GitOps.
- Estrategias de gobernanza y remediación automatizada.
- Y una propuesta para redefinir el rol DevOps como garante de flujos seguros y escalables.

---

### PREGUNTAS DEL CASO

### 1. ¿Qué fallos de gobernanza GitOps puedes identificar en el flujo de trabajo descrito, y qué mecanismos deberían haberse implementado para prevenir el despliegue inseguro?

| Fallos de Gobernanza Identificados                                                                                                                          | Mecanismos de Prevención Requeridos                                                                                                                                                                                |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Omisión del Flujo Pull Request (PR):** Se permitió un *push* directo a la rama principal (fuente de verdad), eludiendo la auditoría y revisión por pares. | **Políticas de Protección de Rama:** Configurar la rama principal para **bloquear *push's* directos** y **requerir siempre Pull Requests (PRs) con aprobaciones**.                                                 |
| **Exposición de Secretos en Git:** El manifiesto contenía claves expuestas, violando la regla fundamental de no almacenar secretos en texto plano en Git.   | **Cifrado de Secretos End-to-End:** Asegurar que **nunca se almacenen secretos en texto plano en Git**. Usar herramientas como **Sealed Secrets** o **ESO (External Secrets Operator)**.                           |
| **Falta de Validación Previa a la Fusión:** El manifiesto inseguro se fusionó sin pasar por comprobaciones automatizadas.                                   | **Integración CI con Validadores:** Implementar *pipelines* CI que ejecuten **linters, validadores de *schema*** y, críticamente, **escáneres de secretos** (e.g., Gitleaks) en el PR antes de permitir la fusión. |
| **Sincronización Automática Riesgosa:** La sincronización automática de ArgoCD se activó sin una capa de aprobación en entornos críticos.                   | **Aprobación Manual del Despliegue:** Desactivar la política de **`Automated Sync`** para entornos sensibles o usar **Sync Hooks** para requerir una aprobación explícita antes de la sincronización.              |

---

### 2. Desde la perspectiva de ArgoCD avanzado, ¿cómo podrías configurar validaciones, bloqueos o políticas de sincronización que eviten despliegues no aprobados o no auditados?

1. **Validación Pre-Despliegue con OPA/Gatekeeper (DevSecOps):**
    - **Mecanismo:** Integrar un *Policy Engine* como **Open Policy Agent (OPA)** o **Gatekeeper** con ArgoCD.
    - **Acción:** Definir una política estricta que **rechace cualquier manifiesto** que contenga secretos en texto plano o que no cumpla con los estándares de seguridad definidos.

2. **Control Fino de Sincronización:**
    - **Modo Manual en Producción:** Para servicios críticos como la autenticación, deshabilitar la política **`Automated Sync`** y configurarla en modo **`Manual Sync`** para forzar la intervención humana después de la aprobación del PR.
    - **Self-Heal y Detección de Desviación:** Mantener activa la política **`Self-heal`** para revertir inmediatamente cualquier cambio manual realizado directamente en el clúster que no esté en Git, asegurando la integridad operativa. Esto es clave para la **Drift Detection and Reconciliation**.

3. **Control del Orden de Despliegue:**
    - Utilizar **`Sync Waves` y *Hooks* personalizados** (`PreSync`, `PostSync`) para controlar la secuencia de despliegue, asegurando que los componentes de seguridad y la infraestructura (como el gestor de secretos) estén operativos antes de que se desplieguen las aplicaciones.

---

### 3. Proponga una solución técnica que refuerce la gestión segura de secretos en este entorno, considerando herramientas compatibles con GitOps y las capacidades de ArgoCD.

La solución propuesta es el desacoplamiento de secretos utilizando **HashiCorp Vault** como fuente de verdad central, sincronizada con Kubernetes mediante el **External Secrets Operator (ESO)**.

1.  **HashiCorp Vault (Fuente de Verdad y Rotación):**
    * **Rol:** Almacenamiento centralizado, cifrado, **rotación periódica de credenciales sensibles** y control de acceso basado en políticas (ACL).
    * **Beneficio:** Vault permite la generación de **secretos dinámicos** (e.g., credenciales de base de datos temporales), aumentando la seguridad.

2.  **External Secrets Operator (ESO) (Integración GitOps):**
    * **Rol:** ESO se instala en el clúster de Kubernetes y se encarga de sincronizar secretos desde proveedores externos (como Vault) a *Secrets* nativos de Kubernetes.
    * **Flujo Seguro:** En el repositorio Git, el equipo versiona un manifiesto **`ExternalSecret`** que **solo declara la ubicación** del secreto en Vault. ArgoCD sincroniza este manifiesto *no-sensible* al clúster. ESO, autenticado con Vault, recupera el valor y lo inyecta como un *Secret* en el *namespace* destino.
    * **Resultado:** **El secreto nunca reside en Git**, mitigando completamente el riesgo de exposición accidental en el repositorio.

---

### 4. ¿Cómo ha evolucionado el rol del equipo DevOps frente a este tipo de incidentes, y qué competencias deben reforzarse para asumir un liderazgo en flujos declarativos, seguridad y automatización continua?

El incidente cataloga la evolución del rol de DevOps de ser un simple automatizador (*Automation Engineer*) a un **Arquitecto de la Gobernanza y la Resiliencia Operacional**.

| Evolución del Rol                                                                                                                                                                                                                     | Competencias Clave a Reforzar (Perfil Senior)                                                                                                                                                       |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **De Técnico a Arquitecto de Gobernanza:** Liderar el diseño de la arquitectura GitOps, definir **estrategias de escalamiento** y las políticas de revisión de cambios (PR).                                                          | **Gobernanza GitOps Avanzada:** Diseñar arquitecturas seguras, escalables y auditables. Definición de políticas de control de acceso (RBAC) y flujos.                                               |
| **De Operaciones a DevSecOps:** La seguridad se convierte en una responsabilidad primaria (*shift-left*), requiriendo la **integración de políticas de seguridad desde el inicio**.                                                   | **DevSecOps Estratégico:** Dominio de herramientas de *shift-left security*, como OPA/Gatekeeper y la auditoría de secretos.                                                                        |
| **De Reactivo a Proactivo con Observabilidad:** No solo restaurar, sino medir continuamente el impacto de las prácticas DevOps (métricas DORA) y asegurar la **trazabilidad completa** del sistema.                                   | **Observabilidad Distribuida y Auditoría:** Integración de Prometheus/Grafana con ArgoCD para métricas de fallas y detección de desviaciones. Diseño de un **registro de accesos a secretos**.      |
| **De Implementador a Garante de la Resiliencia:** Capacidad para diseñar un plan de **Disaster Recovery (DR)** que utilice **GitOps como soporte para la restauración de la configuración** en entornos modernos (Cloud, Kubernetes). | **Gestión de la Resiliencia (DRP):** Establecer y probar planes de DR que cumplan con RTO/RPO realistas, usando herramientas de IaC y automatización.                                               |

---

[Regresar](./2-%20Evaluación%20Final%20-%20Respuestas.md)

---
