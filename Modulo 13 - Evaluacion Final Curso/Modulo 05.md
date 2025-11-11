## Módulo 05 - Kubernetes avanzado

### Pregunta de desarrollo:

### 17. ¿Cuál es la utilidad de Helm en un entorno Kubernetes avanzado y cómo mejora el despliegue de aplicaciones?

Utilidad de Helm: Helm es el gestor de paquetes oficial para Kubernetes. Su utilidad principal es simplificar el despliegue de aplicaciones complejas en entornos productivos, asegurando consistencia, trazabilidad, versionado y personalización controlada.
Mejoras en el despliegue de aplicaciones:
* Parametrización y reutilización: Los Helm Charts permiten la instalación parametrizada, usando el mismo código (Templates YAML) con diferentes valores (values.yaml) para distintos entornos (dev, prod).
* Gestión del ciclo de vida: Facilita la actualización controlada (helm upgrade) y el Rollback automático (helm rollback) a versiones funcionales previas, lo cual es ideal para entornos de producción.
* Modularidad: Permite desplegar soluciones distribuidas completas utilizando subcharts para manejar dependencias desacopladas.
* Integración CI/CD y GitOps: Se integra en pipelines con comandos como helm lint (validación sintáctica) y helm template (renderización local), siendo la base para flujos GitOps controlados por ArgoCD o Flux.

---
### 18. ¿Qué función cumple un Operator en Kubernetes y cómo se diferencia de un Deployment tradicional?
Función de un Operator: Un Operator es una extensión del controlador de Kubernetes que utiliza Custom Resource Definitions (CRDs) para manejar aplicaciones complejas como si fueran recursos nativos. Su función es encapsular lógica operativa experta (ej. para bases de datos o Kafka), automatizando el ciclo de vida completo: instalación, configuración, monitoreo, respaldo, recuperación ante fallas y escalamiento.

Diferencia con un Deployment tradicional:
| Recurso | Función Principal | Ámbito de acción |
| ---- | ---- | ---- |
| Deployment | Gestiona el estado básico de los pods (ej. número de réplicas, imagen). | Solo gestiona recursos Kubernetes básicos y stateless. |
| Operator | Gestiona el ciclo de vida operativo completo de una aplicación compleja (stateful). | Utiliza un ciclo de reconciliación para mantener el estado deseado de un recurso personalizado (CRD), ejecutando lógica compleja de backup, failover y upgrades. |

---
### Pregunta de integración y reflexión:

### 19. ¿Qué buenas prácticas se deben aplicar para mejorar la seguridad de los pods en un clúster de Kubernetes en producción?
Las buenas prácticas se centran en el principio de menor privilegio y la validación de configuración:
* Restricción de privilegios:
  * Prohibir ejecución como root (runAsNonRoot: true).
  * Forzar sistemas de archivos de solo lectura (readOnlyRootFilesystem: true).
* Aplicación de políticas: Aplicar PodSecurity Standards (PSS) (baseline o restricted) mediante etiquetas en namespaces.
* Control de admisión: Integrar OPA/Gatekeeper o Kyverno para rechazar manifiestos inseguros que usen capacidades elevadas o accedan al host.
* Integridad de imágenes: Usar imágenes firmadas (cosign) y escaneadas (Trivy, Snyk) para asegurar su integridad y detectar vulnerabilidades.
* Gestión de secretos: No codificar secretos en manifiestos; usar Vault o External Secrets Operator.
---

### 20. ¿Cómo impacta la gestión de redes (CNI, políticas de red) en la disponibilidad y seguridad de las aplicaciones en Kubernetes?

Impacto en la Disponibilidad (CNI): El plugin CNI (Container Network Interface) es esencial para la disponibilidad, ya que implementa el modelo de red plano de Kubernetes, asegurando que:
• Cada pod tenga su propia dirección IP única.
• Los pods puedan comunicarse entre sí sin NAT, lo cual es la base para las comunicaciones internas y el descubrimiento de servicios (CoreDNS).
Impacto en la seguridad (Políticas de Red):
Las NetworkPolicies son vitales para la seguridad porque:
• Permiten definir qué pods pueden comunicarse entre sí y con el exterior.
• Facilitan la microsegmentación y el aislamiento de cargas de trabajo por namespace o etiqueta.
• Por defecto, todo el tráfico está permitido, por lo que la aplicación explícita de estas políticas es necesaria para restringir accesos innecesarios (Defensa en profundidad).

---

[Regresar](./2-%20Evaluación%20Final%20-%20Respuestas.md)

---
