# CURSO: DEVOPS SENIOR

## Integrantes
* Raul Farías
* Leonardo Oyarzun
* Sebastían Rivera
* Gabriel Castillo

# Módulo 7 - Infraestructura como codigo avanzado

## Evaluación Parcial

### 1. ¿Qué ventajas ofrece el uso de módulos en Terraform?

El uso de módulos en Terraform ofrece múltiples ventajas para gestionar la infraestructura como código (IaC) de manera eficiente y segura. Permiten agrupar recursos en bloques lógicos y reutilizables, lo que aporta los siguientes beneficios:

* **Reutilización y Escalabilidad**: Permiten usar los mismos componentes de infraestructura en diferentes entornos (desarrollo, producción) o proyectos, asegurando consistencia.
* **Organización y Mantenibilidad**: Mejoran la estructura del código al dividir la infraestructura en partes más pequeñas y manejables, lo que evita la duplicación y facilita el mantenimiento.
* **Colaboración**: Facilitan el trabajo en equipo, ya que distintos equipos pueden desarrollar y mantener módulos de forma independiente.
* **Reducción de Errores**: Al usar componentes probados y estandarizados, se minimizan los errores humanos, especialmente en configuraciones complejas.
* **Automatización y Trazabilidad**: Son fundamentales para la automatización en pipelines de CI/CD y flujos GitOps, mejorando la trazabilidad de los cambios.

---

### 2. ¿En qué consiste el testing en Terraform y qué herramientas se pueden usar?

El testing en el contexto de Terraform es una práctica clave para asegurar la **calidad, estabilidad y seguridad** de la infraestructura antes de ser desplegada. Consiste en aplicar validaciones automáticas en diferentes niveles para detectar errores de forma temprana.

Los niveles de testing y las herramientas asociadas son:

* **Linting (Formato y Estilo)**: Asegura la coherencia y legibilidad del código.
    * **Herramientas**: `terraform fmt` (estandariza el formato), `tflint` (analiza errores comunes y malas prácticas) y `terraform-docs` (genera documentación).
* **Validación Estática y Sintáctica**: Comprueba que la configuración sea sintácticamente correcta sin desplegar recursos.
    * **Herramientas**: `terraform validate` (valida la sintaxis) y `terraform plan` (simula los cambios).
* **Análisis de Seguridad**: Escanea el código en busca de configuraciones inseguras.
    * **Herramientas**: `Checkov` y `Tfsec` (detectan vulnerabilidades) y `OPA/Rego` (definen políticas personalizadas).
* **Testing Funcional y de Integración**: Verifica que los módulos desplieguen infraestructura que funciona como se espera.
    * **Herramienta**: **Terratest**, una librería en Go que despliega la infraestructura en un entorno aislado, realiza validaciones y luego la destruye.

---

### 3. ¿Qué es Sentinel y cómo complementa a Terraform?

**Sentinel** es el motor de **Políticas como Código (Policy-as-Code)** de HashiCorp, diseñado para integrarse con Terraform Cloud y Terraform Enterprise.

Complementa a Terraform al actuar como una capa de **gobernanza automatizada** que se ejecuta *durante* el flujo de trabajo de `plan` y `apply`. Mientras que herramientas como `Checkov` o `Tfsec` analizan el código de forma estática (antes de la ejecución), Sentinel inspecciona el plan de ejecución y puede **bloquear activamente un despliegue** si no cumple con las políticas organizacionales definidas.

Su objetivo es automatizar el cumplimiento de normativas de seguridad, costos y arquitectura, garantizando que toda la infraestructura desplegada siga las reglas de la organización sin depender de revisiones manuales.

---

### 4. ¿Qué tipo de políticas se pueden definir con Sentinel?

Con Sentinel se puede definir una amplia gama de políticas personalizadas para controlar la infraestructura. Algunos ejemplos prácticos incluyen:

* **Seguridad**: Impedir la creación de buckets S3 con acceso público o bloquear configuraciones que expongan puertos inseguros.
* **Cumplimiento de Estándares**: Forzar que todos los recursos tengan etiquetas específicas, como `owner` o `cost-center`.
* **Control de Costos**: Permitir únicamente la creación de ciertos tamaños o tipos de instancias en entornos productivos.
* **Gobernanza Geográfica**: Validar que los recursos solo se desplieguen en regiones autorizadas por la organización.
* **Buenas Prácticas**: Asegurar que recursos como bases de datos o volúmenes de almacenamiento siempre se creen con el cifrado habilitado.

---

### 5. ¿Cómo se puede integrar Terraform en un flujo GitOps?

La integración de Terraform en un flujo GitOps se basa en utilizar un **repositorio Git como la única fuente de verdad** para definir el estado deseado de la infraestructura. El ciclo de vida típico es el siguiente:

1.  **Cambio en una Rama**: Un ingeniero propone un cambio en la infraestructura modificando el código Terraform en una rama secundaria (por ejemplo, `feature/new-database`).
2.  **Pull Request (PR) y Validación Automática**: Al abrir un PR, se disparan pipelines de CI/CD que ejecutan una serie de validaciones automáticas, como `terraform fmt`, `validate`, `tflint`, `checkov` y, crucialmente, `terraform plan` para previsualizar el impacto.
3.  **Revisión y Aprobación**: El equipo revisa el código y el resultado del plan. Si es aprobado, el PR se fusiona (hace *merge*) a la rama principal (ej. `main` o `develop`).
4.  **Despliegue Automático**: La fusión a la rama principal activa otro pipeline que ejecuta el comando `terraform apply`, aplicando los cambios de manera automática en el entorno correspondiente.
5.  **Almacenamiento del Estado**: El estado de la infraestructura (`terraform.tfstate`) se guarda de forma segura en un backend remoto.

Este proceso se puede orquestar con herramientas como GitHub Actions, GitLab CI, Jenkins, Atlantis o Terraform Cloud.

---

### 6. ¿Qué beneficios aporta GitOps a la gestión de IaC?

La adopción de GitOps para gestionar Infraestructura como Código (IaC) con Terraform aporta beneficios clave para la operación y gobernanza:

* **Auditoría Total**: Cada cambio en la infraestructura queda registrado en el historial de Git, con autor, motivo y revisión.
* **Despliegues Confiables y Repetibles**: La automatización elimina los errores humanos de las aplicaciones manuales y garantiza que el mismo commit genere siempre el mismo resultado.
* **Rollback Simplificado**: Revertir la infraestructura a un estado anterior es tan sencillo como hacer un `git revert` a un commit previo.
* **Seguridad Fortalecida**: El acceso para modificar la infraestructura se controla a través de los permisos de Git (ej. protección de ramas), limitando quién puede aprobar y fusionar cambios.
* **Escalabilidad Organizacional**: Permite que múltiples equipos colaboren en la misma infraestructura de forma segura y sin conflictos, gracias a los flujos de revisión de código.

---

### 7. ¿Qué desafíos pueden surgir al combinar Terraform, Sentinel y GitOps?

Aunque la combinación de estas tres prácticas es muy poderosa, presenta ciertos desafíos que deben gestionarse adecuadamente:

* **Complejidad del Flujo de Trabajo**: La configuración inicial y el mantenimiento de un pipeline GitOps robusto que integre validaciones, pruebas y políticas de Sentinel puede ser complejo. Requiere una curva de aprendizaje y disciplina por parte del equipo.
* **Riesgo de "Drift"**: Si se realizan cambios manuales en la infraestructura fuera del flujo de Git, el estado real se desvinculará del código ("drift"), lo que anula los beneficios de GitOps. Es un desafío cultural y técnico evitar estas modificaciones.
* **Gestión de Políticas**: Escribir y mantener políticas de Sentinel efectivas requiere práctica. Si son demasiado restrictivas o se implementan sin un despliegue gradual (usando el modo `advisory` primero), pueden convertirse en un cuello de botella para los equipos.
* **Seguridad del Pipeline**: El pipeline de CI/CD se convierte en un sistema crítico con altos privilegios. Es fundamental asegurar las credenciales, controlar el acceso a Git y proteger el backend donde se almacena el estado de Terraform.
* **Dependencias y Versionamiento**: Es crucial gestionar de forma estricta el versionamiento de los módulos de Terraform para evitar que cambios inesperados en un módulo compartido afecten a múltiples entornos sin previo aviso.

---
