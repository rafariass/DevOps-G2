# CURSO: DEVOPS SENIOR

## Integrantes
* Raul Farías
* Leonardo Oyarzun
* Sebastían Rivera
* Gabriel Castillo

## Módulo 1: DevOps Estratégico y GitOps

## Evaluación Parcial

### 1. ¿Qué es GitOps y cómo se diferencia de DevOps tradicional?

**GitOps** es una evolución de DevOps que utiliza **Git como la única fuente de verdad** para la gestión de infraestructura, configuraciones y políticas de seguridad. Es un enfoque declarativo, reproducible y auditable que automatiza la entrega y operación de sistemas complejos. Se diferencia del **DevOps tradicional** porque, aunque este promueve la automatización e IaC, GitOps centraliza rigurosamente todo en Git con mecanismos de **reconciliación activa** (como ArgoCD). Esto asegura que el estado real del clúster siempre coincida con lo declarado en Git, detectando y corrigiendo automáticamente las desviaciones.

---

### 2. ¿Cuáles son los patrones comunes en GitOps?

Los patrones comunes de GitOps nos ayudan a estandarizar los despliegues y mejorar la trazabilidad:

| Patrón                                         | Descripción                                                                                                                                                                  |
|------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Repository per Environment**                 | Un repositorio Git distinto para cada entorno (DEV, IST, UAT, STAGE, PROD), ofrece un mayor aislamiento.                                                                     |
| **Single Repository with Environment Folders** | Un único repositorio con carpetas separadas por entorno para una gestión centralizada.                                                                                       |
| **Progressive Delivery Pattern**               | Integración con herramientas como *feature flags* o *canary deployments* para lanzar cambios gradualmente y monitorear el impacto.                                           |
| **Drift Detection and Reconciliation**         | Uso de operadores (ArgoCD, FluxCD) para comparar el estado deseado en Git con el real del clúster, detectando y reconciliando desviaciones.                                  |
| **Multi-Tenant GitOps**                        | Implementación en entornos compartidos con control granular de acceso y segmentación.                                                                                        |

---

### 3. ¿Qué ventajas ofrece ArgoCD en la implementación de GitOps?

**ArgoCD** es una herramienta de despliegue continuo declarativo para Kubernetes basada en GitOps. Sus ventajas incluyen:

*   Actúa como motor de sincronización y componente central de **control, seguridad y visibilidad** del pipeline operativo.
*   Permite orquestar **entornos complejos, multicluster y con políticas estrictas**, asegurando reproducibilidad, trazabilidad y recuperación ante fallos.
*   Ofrece funcionalidades avanzadas como **App of Apps** (jerarquía modular de aplicaciones), **ApplicationSets** (generación automatizada de apps), **Sync Waves y Hooks** (control del orden de despliegue) y **Automated Sync Policies** (sincronización y auto-curación).
*   Proporciona **observabilidad y auditoría** con *audit logs* detallados y métricas de control.

---

### 4. ¿Qué consideraciones de seguridad son clave en GitOps con ArgoCD?

La seguridad en GitOps con ArgoCD es crítica y requiere un diseño robusto. Las consideraciones clave incluyen:

*   **Nunca almacenar secretos en texto plano en Git**.
*   Implementar **escáneres de secretos** para detectar fugas accidentales.
*   Aplicar **políticas de control de acceso** basadas en el principio de mínimo privilegio a secretos y recursos.
*   Integrar **OPA/Gatekeeper** para validar manifiestos antes del despliegue y **RBAC** para limitar acciones según el rol en ArgoCD.
*   Automatizar la **rotación periódica de credenciales** sensibles.
*   Usar **firmas y verificación de integridad** (cosign) para artefactos y secretos.
*   Monitorear y auditar accesos y cambios a través de los **audit logs** de ArgoCD.

---

### 5. ¿Qué herramientas se pueden usar para gestionar secretos en GitOps?

La gestión segura de secretos en GitOps es fundamental.
Las herramientas clave para gestionar secretos sin comprometer la automatización y el versionado son:

| Herramienta                            | Descripción                                                                                                                                                                          |
|----------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Sealed Secrets (Bitnami)**           | Convierte secretos de Kubernetes en manifiestos cifrados seguros para versionar en Git, solo desencriptables por el clúster destino.                                                  |
| **Vault (HashiCorp)**                  | Solución empresarial para almacenamiento, rotación y políticas de acceso de secretos, capaz de generar secretos dinámicos.                                                           |
| **External Secrets Operator (ESO)**    | Permite sincronizar secretos desde proveedores externos (AWS Secrets Manager, Azure Key Vault, Google Secret Manager, HashiCorp Vault) hacia Kubernetes.                             |
| **SOPS (Mozilla)**                     | Cifra archivos YAML/JSON con claves GPG o KMS, compatible con Helm y *pipelines* CI/CD en GitOps.                                                                                    |

---

### 6. ¿Cómo se puede implementar una estrategia de promoción entre ambientes en GitOps?

En GitOps, la promoción entre ambientes se implementa actualizando la **única fuente de verdad: Git**.
Esto se logra mediante estrategias como:

| Estrategia                                        | Descripción                                                                                                                                                               |
|---------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------  |
| **Repository per Environment**                    | Cada stage (DEV, IST, UAT, STAGE, PROD) tiene un repositorio Git distinto. La promoción implica la propagación de cambios de un repositorio a otro vía *Pull Requests*.   |
| **Single Repository with Environment Folders**    | Un único repositorio con carpetas separadas por ambiente. La promoción se gestiona con ramas y *Pull Requests* que mueven los cambios entre configuraciones.               |
| **ArgoCD ApplicationSets**                        | Automatiza la generación de aplicaciones para distintos clústeres o ambientes, usando parámetros diferenciados en las plantillas.                                         |
| **Progressive Delivery Pattern**                  | Permite despliegues graduales y controlados, monitoreando el impacto antes de la promoción completa.                                                                      |

---

### 7. ¿Cómo se sincronizan los cambios en ArgoCD y qué modos de sincronización existen?

ArgoCD sincroniza los cambios comparando el **estado deseado** definido en el repositorio Git con el **estado real** del clúster de Kubernetes.
El "**Application Controller**" de ArgoCD es el componente que ejecuta esta reconciliación.
Existen diferentes **modos y políticas de sincronización**:

*   **Sincronización manual:** El usuario inicia la sincronización de forma explícita.
*   **Automated Sync Policies:**
    *   **On commit (auto-sync):** ArgoCD sincroniza automáticamente cada vez que se detecta un nuevo *commit* en el repositorio Git.
    *   **Auto-prune:** Elimina automáticamente los recursos obsoletos en el clúster que ya no están definidos en Git.
    *   **Self-heal:** Reversa automáticamente cualquier cambio manual o desviación ("**drift**") en el clúster para restaurar el estado deseado declarado en Git, asegurando la integridad operativa.

---
