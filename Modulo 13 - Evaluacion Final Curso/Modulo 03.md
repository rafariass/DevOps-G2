## Módulo 03 - Seguridad avanzada y DevSecOps

### OBJETIVO A MEDIR Y CRITERIO DE EVALUACIÓN

Define conceptos de **seguridad avanzada** y **`DevSecOps`**, según las practicas avanzadas de:
- GitOps
- DevSecOps
- Kubernetes
- Observabilidad
- IaC (Infrastructure as Code)
- FinOps
- AIOps

### CONTENIDO QUE SE REVISA (**`MÓDULO 3`**)

- DevSecOps
- Snyk, Trivy, Checkov, Vault
- Seguridad en Kubernetes

### OBSERVACIÓN

Las interrogantes que se plantean en esta sección están diseñadas para evaluar la comprensión técnica y práctica del enfoque DevSecOps aplicado a entornos de infraestructura moderna. Permiten comprobar su capacidad para integrar herramientas como Trivy, Snyk, Checkov y Vault en procesos automatizados, asegurando la detección temprana de vulnerabilidades y el manejo seguro de secretos en Kubernetes y pipelines CI/CD.

---

### PREGUNTA DE DESARROLLO

### 9. ¿Cómo contribuye Trivy a la seguridad en el ciclo DevOps y en qué etapas puede integrarse?

**Trivy** contribuye a la seguridad en el ciclo **DevOps** al ser una **herramienta de código abierto** rápida y portátil que permite el **escaneo de seguridad en múltiples capas** del *stack*. Su principal valor es la capacidad de aplicar el principio de **Shift Left**, integrándose en etapas tempranas para detectar vulnerabilidades antes del despliegue.

| Contribución de Trivy a la Seguridad | Etapas de Integración                                                                                                                                                                          |
| ------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Escaneo de Contenedores**          | **Build (CI):** Escanea **imágenes Docker** para detectar **CVEs** (Common Vulnerabilities and Exposures - Vulnerabilidades y Exposiciones Comunes).                                           |
| **Validación de IaC**                | **Planificación/Commit (CI):** Revisa **configuraciones de Kubernetes** e **IaC** (Infrastructure as Code - Infraestructura como Código) (como Terraform o Helm) para auditar políticas.       |
| **Generación de Trazabilidad**       | **Build (CI/CD):** Soporta la generación de **SBOM** (Software Bill of Materials - Lista de Materiales de Software), mejorando la **trazabilidad de componentes**.                             |
| **Portabilidad y Rapidez**           | **Integración Simple:** Su bajo tiempo de escaneo y compatibilidad con **GitOps** lo hacen ideal para ser invocado en cada paso del *pipeline*.                                                |

**`Definiciónes`**

- **CVE** (Common Vulnerabilities and Exposures - Vulnerabilidades y Exposiciones Comunes): Una lista pública de vulnerabilidades de seguridad de la información conocidas.
El concepto de Shift Left (traducido como "desplazar a la izquierda") es un principio fundamental en las metodologías DevSecOps y DevOps que promueve mover las actividades de seguridad y calidad desde las etapas finales del ciclo de vida del desarrollo de software (SDLC) hacia el inicio.

- **Shift Left** Concepto en que la seguridad, las pruebas y la validación de cumplimiento deben ser integradas y automatizadas lo más temprano posible en el pipeline de CI/CD, idealmente desde el momento en que el desarrollador escribe o hace un commit de código.

---

### 10.¿Qué función cumple Vault en una arquitectura DevSecOps y por qué es preferible frenteamanejar secretos manualmente?

**Vault** es un **sistema de gestión de secretos de nivel empresarial**, desarrollado por HashiCorp. Su función principal en **DevSecOps** es **almacenar, acceder y controlar dinámicamente credenciales, *tokens*, *API keys*, certificados y otros datos sensibles** con políticas de acceso altamente definidas en la organización.

### Función de Vault en DevSecOps
* **Almacenamiento Seguro:** Ofrece **almacenamiento seguro de secretos** con control **RBAC** (Role-Based Access Control - Control de Acceso Basado en Roles), eliminando la necesidad de codificar secretos en manifiestos o variables de entorno.

* **Rotación Dinámica:** Permite la **rotación automática de claves y credenciales** y la **emisión dinámica de certificados y *tokens* JWT**.

* **Inyección Segura:** Permite **montar secretos como variables de entorno** en aplicaciones o inyectar claves durante el *build*, integrándose con Kubernetes a través de *Sidecar Injector* o *CSI driver*.

### Preferencia sobre el Manejo Manual
Es preferible usar Vault frente al manejo manual de secretos por las siguientes razones:
1. **Eliminación del *Hardcoding*:** Evita la práctica riesgosa de **almacenar secretos en el repositorio** de código o directamente en manifiestos, previniendo la **exposición accidental**.

2. **Principio de Mínimos Privilegios:** Asegura que solo los usuarios y sistemas **autorizados** tengan acceso a los secretos, limitando la **superficie de ataque**.

3. **Auditoría y Trazabilidad:** Vault permite una **auditoría completa** de quién accedió a qué secreto y cuándo, algo casi imposible de mantener manualmente.

4. **Automatización:** Facilita la **rotación automática** y la **expiración de secretos**, una tarea que, si se hace manualmente, es lenta, propensa a errores y a menudo se omite.

---

### PREGUNTA DE INTEGRACIÓN Y REFLEXIÓN

### 11.¿Qué beneficios ofrece integrar herramientas como Checkov o Snyk en un flujo GitOps oCI/CD desde una perspectiva DevSecOps?

La integración de **`Checkov`** (validación de IaC) y **`Snyk`** (análisis de dependencias/contenedores) en flujos **`GitOps`** o **`CI/CD`** ofrece beneficios que van desde la calidad del código hasta la reducción del riesgo operativo:

* **Aplicación del *Shift Left***: Permite aplicar **controles de seguridad desde las primeras etapas** del desarrollo.
    * **Snyk** se integra en la **fase de Desarrollo/CI** para analizar dependencias en tiempo real y escanear imágenes de contenedores, deteniendo el *build* antes de que un componente vulnerable llegue al despliegue.
    * **Checkov** se ejecuta en la **fase de Planificación/Build** y en *workflows* de **GitOps**, validando **IaC** (Terraform, Kubernetes, etc.) contra **más de 1000 políticas predefinidas** tan pronto como se crea un *Pull Request* (PR).

* **Validación Bloqueante y Estandarización:**
    * Ambas herramientas permiten la **implementación de validación bloqueante** en *Pull Requests* o *builds* inseguros, asegurando que las **configuraciones y el código cumplan con políticas organizacionales** y estándares.
    * **Checkov** se integra con **OPA/Gatekeeper o Kyverno** para **rechazar manifiestos inseguros** en la etapa de *Deploy*, forzando el cumplimiento de la seguridad como código.

* **Responsabilidad Compartida y Agilidad:**
    * La automatización de estos controles integra la **seguridad sin sacrificar agilidad**, promoviendo la **responsabilidad compartida** (cultura DevSecOps) al dar *feedback* inmediato al desarrollador.

---

### 12.¿Cómo cambia la cultura de desarrollo cuando se implementan prácticas DevSecOps encomparación con un enfoque DevOps tradicional?

La implementación de prácticas **DevSecOps** (Development, Security, Operations), marca un cambio cultural significativo en comparación con el **DevOps** tradicional, (Development, Operations), donde la seguridad a menudo se manejaba como una fase separada.

| Aspecto Cultural                   | DevOps Tradicional                              | DevSecOps                                                                                               |
| ---------------------------------- | ----------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| **Responsabilidad de Seguridad**   | La seguridad es una etapa posterior.            | La seguridad es una **responsabilidad compartida** a lo largo de **todo el ciclo de vida**.             |
| **Momento de la Seguridad**        | Es una etapa **posterior** o "bolted-on".       | Es un **componente transversal y automatizado** ("Shift Left").                                         |
| **Relación entre Equipos**         | Existencia de **silos**.                        | Fomento de una **cultura colaborativa** donde Dev, Sec y Ops trabajan juntos.                           |
| **Reacción a Fallos**              | **Respuesta reactiva**.                         | **Respuesta rápida y proactiva** ante vulnerabilidades.                                                 |
| **Herramientas y Automatización**  | Automatización enfocada en *build* y *deploy*.  | Automatización enfocada en **controles de seguridad en cada etapa** del *pipeline*.                     |
| **Rol del Desarrollador**          | Se enfoca principalmente en la funcionalidad.   | Se espera que el desarrollador sea **consciente de la seguridad** e impulse la **mejora continua**.     |


---

[Regresar](./2-%20Evaluación%20Final%20-%20Respuestas.md)

---
