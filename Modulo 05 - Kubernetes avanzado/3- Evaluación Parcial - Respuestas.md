# CURSO: DEVOPS SENIOR

## Integrantes
* Raul Farías
* Leonardo Oyarzun
* Sebastían Rivera
* Gabriel Castillo

# Módulo 5: Kubernetes avanzado

## Evaluación Parcial

### 1. ¿Qué ventajas ofrece Helm frente al uso directo de manifiestos YAML?

Helm ofrece varias ventajas clave sobre la gestión manual de archivos YAML, convirtiéndolo en una herramienta esencial para entornos productivos. Las principales ventajas son:

* **Reutilización y paquetizado**: Helm empaqueta conjuntos de recursos de Kubernetes en unidades reutilizables llamadas *Charts*. Esto permite distribuir aplicaciones complejas como una única dependencia.
* **Gestión de configuraciones**: Permite parametrizar manifiestos YAML mediante un archivo `values.yaml`. Esto simplifica la gestión de configuraciones para distintos entornos (desarrollo, QA, producción) sin modificar los manifiestos base.
* **Ciclo de vida controlado**: Helm gestiona el ciclo de vida completo de una aplicación. El comando `helm upgrade` aplica cambios de manera controlada, impactando solo lo necesario, mientras que `helm rollback` permite revertir una actualización fallida a una versión anterior de forma automática, lo cual es ideal para producción.
* **Integración con CI/CD**: Se integra fácilmente en flujos de CI/CD y GitOps. Proporciona comandos para validación (`helm lint`), renderizado en seco (`helm template`) y comparación de versiones (`helm diff`) antes de aplicar los cambios.

---

### 2. ¿Qué es un Kubernetes Operator y en qué se diferencia de un controlador estándar?

Un **Kubernetes Operator** es una extensión del controlador de Kubernetes que utiliza **Custom Resource Definitions (CRDs)** para gestionar aplicaciones complejas como si fueran recursos nativos del clúster.

La diferencia clave con un controlador estándar (como el de un *Deployment*) es que un Operator **codifica conocimiento operativo experto y específico de una aplicación**. Mientras un controlador estándar se limita a mantener un estado deseado de recursos nativos (ej. "asegurar que 3 pods estén corriendo"), un Operator automatiza tareas complejas y personalizadas del ciclo de vida completo de una aplicación, como:
* Instalación y configuración.
* Copias de seguridad (*backups*) y recuperación ante fallos (*failover*).
* Actualizaciones de versión (*upgrades*).
* Escalamiento automático y monitoreo.

En resumen, un Operator extiende la API de Kubernetes para manejar aplicaciones con estado (*stateful*) de forma automatizada, reduciendo la intervención manual.

---

### 3. ¿Qué prácticas de seguridad básicas se deben aplicar a pods en Kubernetes?

La práctica de seguridad fundamental para los pods se centra en los **Pod Security Standards (PSS)**, que reemplazan a las ya obsoletas *PodSecurityPolicies* (PSP). Los PSS definen tres niveles de seguridad que se aplican a nivel de *namespace* mediante etiquetas:

1.  **privileged**: Sin restricciones de seguridad. Su uso debe ser muy limitado.
2.  **baseline**: Aplica una configuración mínima de seguridad.
3.  **restricted**: Es el nivel más restrictivo y recomendado. Impide la ejecución de contenedores como *root*, el acceso a volúmenes del anfitrión (`hostPath`) y el uso de otros privilegios elevados.

Para aplicar el estándar más restrictivo a un *namespace*, se usaría el comando:
`kubectl label ns <nombre-del-namespace> pod-security.kubernetes.io/enforce=restricted`.

---

### 4. ¿Cómo se gestiona el control de acceso en Kubernetes?

El control de acceso en Kubernetes se gestiona principalmente a través de **RBAC (Role-Based Access Control)**. Este mecanismo se basa en los siguientes componentes:

* **Role / ClusterRole**: Definen un conjunto de permisos (verbos como `get`, `list`, `create`) sobre un conjunto de recursos (`pods`, `services`, etc.). Un `Role` se aplica a un *namespace* específico, mientras que un `ClusterRole` aplica a todo el clúster.
* **RoleBinding / ClusterRoleBinding**: Asocian un `Role` o `ClusterRole` a un "sujeto" (`subject`), que puede ser un usuario, un grupo o una *ServiceAccount*.

El objetivo de RBAC es aplicar el **principio de menor privilegio**, garantizando que cada sujeto solo tenga los permisos estrictamente necesarios para realizar sus funciones.

---

### 5. ¿Qué mecanismos existen para aislar redes entre pods o namespaces?

El mecanismo principal para aislar redes en Kubernetes son las **NetworkPolicies**. Estos recursos permiten definir reglas de firewall a nivel de capa 3/4 para controlar el tráfico entre pods.

Por defecto, en Kubernetes todo el tráfico entre pods está permitido. Las *NetworkPolicies* permiten implementar una segmentación de red estricta (o microsegmentación) basada en:
* **Selectores de Pods y Namespaces**: Las reglas se aplican a los pods que coinciden con ciertas etiquetas (`labels`).
* **Puertos y Protocolos**: Permiten especificar qué puertos y protocolos (TCP/UDP) están permitidos para el tráfico de entrada (`ingress`) y salida (`egress`).
* **Reglas explícitas**: Es necesario crear políticas explícitas para restringir la comunicación. Por ejemplo, se puede crear una política que deniegue todo el tráfico entrante por defecto y luego añadir reglas para permitir comunicaciones específicas.

---

### 6. ¿Qué ventajas ofrece el uso de un Service Mesh como complemento a la gestión de red en Kubernetes?

Un **Service Mesh** (como Istio o Linkerd) añade una capa de infraestructura de red dedicada sobre Kubernetes, ofreciendo capacidades avanzadas que no están presentes de forma nativa. Sus principales ventajas son:

* **Seguridad mejorada**: Automatiza la encriptación de todo el tráfico entre servicios mediante **mTLS (mutual TLS)**, implementando un modelo de *Zero Trust* dentro del clúster.
* **Control de tráfico avanzado**: Permite definir reglas sofisticadas de enrutamiento, reintentos automáticos (*retries*), políticas de tiempo de espera (*timeouts*) y disyuntores (*circuit breakers*).
* **Observabilidad profunda**: Ofrece telemetría detallada del tráfico de red, incluyendo métricas, *logs* y trazas distribuidas, sin necesidad de modificar el código de la aplicación.
* **Desacoplamiento**: Libera a los desarrolladores de tener que implementar lógica de red (como reintentos o encriptación) en el código de sus aplicaciones, ya que el *Service Mesh* se encarga de ello a través de *sidecars*.

---

### 7. ¿Cómo manejaría una actualización de versión en un chart Helm sin afectar el entorno productivo?

Para realizar una actualización segura de un *chart* de Helm en producción, seguiría un proceso controlado y validado, integrado en un *pipeline* de CI/CD:

1.  **Validación estática**: Antes de cualquier despliegue, validaría la sintaxis del *chart* con el comando `helm lint`.
2.  **Revisión de cambios (Dry-Run)**: Generaría los manifiestos YAML resultantes sin aplicarlos al clúster usando `helm template`. Adicionalmente, utilizaría `helm diff` para visualizar las diferencias exactas entre la versión actual y la nueva. Esto permite auditar los cambios antes de que impacten el entorno.
3.  **Despliegue controlado**: Ejecutaría la actualización con el comando `helm upgrade mi-release ./mi-chart`, que aplica los cambios de forma incremental.
4.  **Plan de contingencia (Rollback)**: Si la nueva versión presenta problemas, ejecutaría inmediatamente `helm rollback mi-release <VERSION_ANTERIOR>` para revertir la aplicación a su estado funcional previo. Esta capacidad es una de las funcionalidades clave de Helm para entornos productivos.

Este proceso se apoyaría en el uso de archivos de valores específicos por entorno (ej. `values-prod.yaml`) para asegurar que la configuración es la correcta para producción.

---
