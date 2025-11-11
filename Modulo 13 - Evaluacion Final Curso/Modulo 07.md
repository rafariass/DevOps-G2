## Módulo 07 - Infraestructura como codigo avanzado


### OBJETIVO A MEDIR Y CRITERIO DE EVALUACIÓN

CREAR INFRAESTRUCTURAS MODULARES, SEGURAS Y FUNCIONAL CON **`TERRAFORM`** AVANZADA, SEGUN LAS PRACTICAS AVANZADAS DE **`GITOPS`**, DEVSECOPS,KUBERNETES, OBSERVABILIDAD, IAC, FINOPS Y AIOPS.:
- GitOps
- DevSecOps
- Kubernetes
- Observabilidad
- IaC (Infrastructure as Code)
- FinOps
- AIOps

### CONTENIDO QUE SE REVISA (**`MÓDULO 1`**)

- Terraform modular
- Testing; Sentinel
- Integración IaC + GitOps

### OBSERVACIÓN
Las preguntas que se plantean en esta sección han sido elaboradas para evaluar la comprensión técnica y práctica del uso de infraestructura como código modular y segura. Permiten comprobar su capacidad para integrar Terraform, pruebas automatizadas, políticas Sentinel y flujos GitOps en entornos reales, asegurando despliegues confiables, trazables y alineados con buenas prácticas organizacionales.
Las preguntas de esta sección invitan al **`análisis crítico`** y vinculan conocimientos técnicos con aplicaciones prácticas. Permiten valorar su capacidad para integrar herramientas de GitOps avanzado con criterios de seguridad, automatización y **`madurez del rol DevOps`** en escenarios empresariales complejos.


---

### PREGUNTAS 

### 25. ¿Qué ventajas ofrece el uso de módulos en Terraform y cómo favorecen la escalabilidad en entornos multi-entorno (dev, prod, etc.)?

El uso de módulos en **Terraform** es considerado una práctica indispensable en entornos **DevOps** para gestionar infraestructura dinámica y de gran escala.

**Ventajas del Uso de Módulos en Terraform**
La modularización consiste en dividir la infraestructura en bloques reutilizables llamados módulos. Cada módulo encapsula recursos específicos, como redes, instancias, bases de datos, buckets o clústeres.

1.**Reutilización y Eficiencia Operativa:** Permite a los equipos construir componentes de infraestructura que pueden ser instanciados múltiples veces y **reutilizar** bloques de infraestructura como componentes independientes. Esto reduce la duplicación de código (el módulo vpc se usa una sola vez) y mejora la eficiencia operativa.

2.**Organización y Mantenibilidad:** El uso de módulos mejora la **organización del código** y la mantenibilidad, al romper con la práctica de definir toda la configuración en un solo archivo plano (main.tf), lo que se vuelve inmanejable a medida que el sistema crece.

3.**Consistencia y Control de Errores:** Permiten definir configuraciones consistentes y aplicar principios de **trazabilidad**. Al parametrizar y versionar los componentes, se aplican cambios controlados y se reduce la probabilidad de errores humanos.

4.**Colaboración y Escalabilidad Organizacional:** Facilitan la colaboración entre equipos, ya que cada equipo puede mantener su propio conjunto de **componentes reutilizables** bajo control de versiones.

5.**Testing y Calidad:** Al ser unidades lógicas y cohesionadas, es más fácil aplicar **pruebas automatizadas** y validaciones específicas para cada componente. Los módulos pueden ser testeados y versionados (por ejemplo, con herramientas como Terratest), lo que mantiene la confianza en la infraestructura.

El enfoque **modular** es fundamental para la **escalabilidad** y la gestión de múltiples entornos (como desarrollo, staging o producción).
La escalabilidad y consistencia se logran de la siguiente manera:

- **Uso de la Misma Lógica con Diferentes Parámetros:** La definición modular permite que múltiples entornos (dev, staging, prod) utilicen la misma lógica de infraestructura. Esto se logra mediante la entrada de diferentes parámetros (variables de entrada, como el bloque CIDR o el tamaño de instancia) para cada instancia del módulo.
- **Modularización por Dominio y Reutilización:** Una práctica avanzada es modularizar por dominio (red, cómputo, seguridad) y luego reutilizar estos módulos en distintos entornos. La estructura típica se organiza de modo que cada entorno (dev, prod) utiliza los mismos módulos, lo que garantiza una consistencia y gobernanza rigurosa.
- **Gestión de Entornos en GitOps:** El enfoque modular se integra perfectamente en flujos GitOps, donde se puede gestionar la separación clara entre entornos mediante ramas o carpetas. Cada rama o carpeta puede consumir los mismos módulos pero con distintos archivos de variables (.tfvars), asegurando consistencia con flexibilidad.
- **Escalabilidad Segura:** La modularidad permite a los equipos escalar su infraestructura de forma segura, mantenible, reutilizable y auditable. Cuando se aplica correctamente, la modularización es un pilar estratégico para el control de versiones, la automatización y el cumplimiento normativo en organizaciones complejas.

---

### 26. ¿Qué rol cumple Sentinel en el ciclo de vida de Terraform y qué tipo de políticas puede validar?

**Sentinel** es una herramienta fundamental en el ciclo de vida de **Terraform**, ya que introduce una capa de gobernanza automatizada y políticas como código (Policy-as-Code).

**Sentinel** es el **motor de políticas como código** desarrollado por HashiCorp. Su función principal es permitir que las organizaciones apliquen **reglas** personalizadas y **automáticas** para gobernar el uso de Infraestructura como Código (IaC).

**Complemento a Terraform:**
1. **Validación en Tiempo de Ejecución:** Sentinel actúa como un sistema de validación de políticas organizacionales. Se diferencia de las herramientas de análisis estático previo porque se ejecuta durante la ejecución de Terraform (es decir, contra el terraform plan o apply).
2. **Control de Flujo:** Su rol principal es impedir que una operación avance si no cumple con las políticas definidas.
3. **Gobernanza Centralizada:** Permite a los equipos de plataforma y seguridad definir políticas centralizadas que se aplican de manera transversal. Esto automatiza el cumplimiento normativo, sin necesidad de depender de revisiones manuales.
4. **Integración:** Sentinel se integra con Terraform Cloud o Terraform Enterprise. Las políticas se agrupan en Policy Sets que se asocian a uno o varios workspaces.
5. **Niveles de Rigidez:** Las políticas pueden aplicarse con distintos niveles de rigidez, lo que permite una implementación progresiva:
    - **Advisory:** Permite la ejecución, pero muestra una advertencia visible.
    - **Soft-mandatory:** Detiene la ejecución, pero permite a usuarios autorizados forzar el avance.
    - **Hard-mandatory:** Impide completamente que la operación continúe.
    
**Tipo de Políticas que Puede Validar Sentinel**
Las políticas de Sentinel inspeccionan el contenido del plan, estado o configuración de Terraform (mediante módulos como tfplan, tfstate o tfconfig), permitiendo la definición de reglas lógicas complejas.
Sentinel puede validar y aplicar políticas relacionadas con la seguridad, el cumplimiento normativo (compliance-as-code) y la arquitectura. Las aplicaciones prácticas incluyen:

| **Categoría de Validación** | **Ejemplos de Políticas Válidas (Tipo de validación)** |
|------------------------------|---------------------------------------------------------|
| **Seguridad y Compliance** | - Impedir la creación de buckets S3 con acceso público.<br>- Forzar la creación de recursos con cifrado habilitado.<br>- Bloquear el uso de claves o contraseñas sin rotación automática.<br>- Prohibir crear buckets sin versionado activado. |
| **Gobernanza y Etiquetado (Tagging)** | - Rechazar cualquier recurso que no tenga ciertas etiquetas requeridas, por ejemplo, la etiqueta `owner`. |
| **Arquitectura y Costos** | - Permitir solo ciertos tamaños de instancias en producción.<br>- Validar que los recursos solo se creen en regiones autorizadas.<br>- Validar atributos, configuraciones y tamaños de recursos. |


**Sentinel** proporciona un lenguaje declarativo expresivo que permite inspeccionar estructuras anidadas dentro de un plan de Terraform. El objetivo es automatizar el cumplimiento de estas normas organizacionales.




---

### 27. ¿Cómo mejora la integración entre IaC y GitOps la trazabilidad y gobernanza de la infraestructura?

# Integración de IaC y GitOps: Trazabilidad y Gobernanza Organizacional

La integración de **Infraestructura como Código (IaC)**, particularmente con herramientas como **Terraform**, y el enfoque de **GitOps** transforma la manera en que se administra la infraestructura, mejorando significativamente la **trazabilidad** y la **gobernanza** organizacional.  
Este modelo implementa una estrategia **robusta, repetible y auditable** para la provisión y mantenimiento de entornos.

**1. Mejora de la Trazabilidad (Traceability)**
La **trazabilidad** se refiere a la capacidad de rastrear cada cambio realizado en la infraestructura.  
**GitOps** garantiza que el **código fuente de la infraestructura** sea la única **fuente de verdad** (*Single Source of Truth*), logrando una auditoría completa.

**Mejoras clave en trazabilidad**

- **Repositorio como Fuente Única de Verdad:**  
  El repositorio Git es el único lugar autorizado para describir el estado deseado de la infraestructura.  
  Los cambios se declaran mediante módulos Terraform y configuraciones (`.tf`, `.tfvars`) almacenados y versionados en Git.

- **Auditoría Total:**  
  Cada cambio en la infraestructura tiene un registro explícito en el historial de *commits*.  
  Cada modificación cuenta con autor, motivo, fecha y una revisión previa.

- **Registro Completo de Ejecución:**  
  En el flujo GitOps, el `terraform apply` es ejecutado por un pipeline CI/CD que se activa ante un *merge* en Git.  
  De esta manera, toda la ejecución queda registrada en el historial de Git, en la salida del pipeline y en los logs del proveedor cloud, lo que proporciona una trazabilidad completa.

- **Rollback Simplificado:**  
  La trazabilidad histórica en Git permite la reversión instantánea de cualquier cambio fallido o no deseado, simplemente volviendo a un *commit* anterior.

---

 **2. Fortalecimiento de la Gobernanza (Governance)**

La **gobernanza** se logra al imponer control sobre cómo y cuándo se realizan los cambios, asegurando que se cumplan las políticas de seguridad y arquitectura definidas.

**Mejoras en la gobernanza**

- **Revisión Colaborativa y Controlada:**  
  Antes de que cualquier modificación se aplique, debe pasar por una revisión colaborativa (como un *pull request* o *merge request*).  
  Este proceso obliga a que los cambios sean validados por el equipo.

- **Validación Automatizada en el Pipeline:**  
  Se ejecutan pruebas automatizadas durante la revisión del código (por ejemplo:  
  `terraform fmt`, `terraform validate`, `tflint`, `checkov`, y `terraform plan`).  
  Esto evita errores humanos y garantiza que la infraestructura declarada sea válida.

- **Gobernanza con Políticas como Código (PaC):**  
  Herramientas como **Sentinel** se integran en el flujo GitOps para bloquear ejecuciones si no se cumplen las políticas organizacionales (como *tags* obligatorios, restricciones de tamaño o límites de costos).  
  Esto eleva el nivel de seguridad y calidad de toda la infraestructura como código.

- **Seguridad Fortalecida:**  
  La integración GitOps permite limitar el acceso para aplicar cambios a través de control de acceso a Git.  
  El cambio solo puede ser aplicado por la **cuenta de servicio del pipeline** tras la aprobación, lo que fortalece el control.

- **Consistencia y Cumplimiento Normativo:**  
  Al aplicar la infraestructura de forma programática y reproducible, se mantiene la consistencia entre el estado declarado y el entorno real.  
  Este enfoque modular y basado en GitOps se convierte en un **pilar estratégico** para el cumplimiento normativo y la gestión de múltiples entornos.

---


---

### 28. ¿Qué importancia tiene el testing en infraestructura como código y qué herramientas pueden usarse para implementarlo?

# Testing en Infraestructura como Código (IaC) con Terraform

El **testing** en la **Infraestructura como Código (IaC)**, particularmente con **Terraform**, es una práctica esencial y estratégica para garantizar la **calidad**, la **seguridad**, la **estabilidad** y la **mantenibilidad** de los recursos desplegados.  
El **Módulo 7** aborda el testing como una parte fundamental de la Infraestructura como Código avanzada.

---

## Importancia del Testing en IaC

El testing cobra una relevancia especial cuando se trabaja con **módulos reutilizables**, ya que un error o una mala práctica en un módulo puede replicarse en múltiples entornos o sistemas.  

La importancia radica en que permite:

1. **Asegurar la Calidad y Estabilidad:**  
   Es crucial para asegurar la calidad, la estabilidad, la seguridad y la mantenibilidad de los recursos desplegados mediante Terraform.

2. **Detección Temprana de Errores:**  
   Permite detectar errores antes del despliegue y antes de que afecten entornos reales (como producción).

3. **Validación de Expectativas:**  
   Permite validar que los módulos cumplen con las expectativas y que las salidas generadas son consistentes.

4. **Mantener la Confianza:**  
   El testing y la validación son necesarios para mantener la confianza en la infraestructura y en los cambios aplicados.

5. **Cumplimiento de Políticas:**  
   Ayuda a aplicar políticas de seguridad y a verificar que las configuraciones de los recursos sean correctas.

6. **Validación de Módulos Reutilizables:**  
   Cuando se usa la modularización, es fundamental aplicar pruebas automatizadas y validaciones específicas para cada componente.  
   Los módulos críticos deben ser sometidos a **testing de integración**.

---

**Herramientas y Niveles de Testing**

El testing en Terraform admite distintos **niveles de pruebas** que, en conjunto, ofrecen una validación exhaustiva para entornos productivos y **pipelines CI/CD**.  
Las herramientas se clasifican según el nivel de validación:

---

**1. Linting (Estilo y Buenas Prácticas)**

Estas herramientas se enfocan en la **coherencia**, **legibilidad** y **formato del código**, y pueden integrarse fácilmente en **pipelines automatizados**.

- `terraform fmt`: Estandariza el formato de los archivos `.tf`.  
- `tflint`: Analiza errores comunes, aplica reglas de estilo y detecta el uso incorrecto de recursos o variables mal tipadas.  
- `terraform-docs`: Genera automáticamente documentación estructurada de los módulos.

---

**2. Validación Estática (Sintaxis y Planificación)**

Estas validaciones se ejecutan sin aplicar cambios reales, asegurando que la configuración sea **teóricamente correcta**.

- `terraform validate`: Asegura que la sintaxis de la configuración es válida.  
- `terraform plan`: Muestra las acciones que Terraform realizaría, ayudando a identificar problemas de ejecución antes del `apply`.

---

**3. Análisis de Seguridad y Cumplimiento**

Se utilizan herramientas especializadas para inspeccionar las configuraciones y detectar posibles **vulnerabilidades** o **incumplimientos de políticas organizacionales**.

- **Checkov:** Escanea módulos Terraform para detectar configuraciones inseguras (como puertos abiertos, claves hardcodeadas o roles mal definidos).  
- **Tfsec:** Realiza análisis estático enfocado en buenas prácticas de seguridad específicas por proveedor (por ejemplo, AWS, Azure, GCP).  
- **OPA / Rego:** Permite definir **políticas organizacionales** que deben cumplirse antes de la aplicación, como la prohibición de crear buckets públicos.

---

**4. Testing Funcional y de Integración**

Para validar que los recursos se crean y funcionan según lo esperado en un entorno aislado, se utilizan herramientas de **prueba automatizada**:

- **Terratest:**  
  Es una librería de testing robusta escrita en **Go** y la herramienta más utilizada para pruebas automatizadas sobre módulos.  
  Permite:
  - Aplicar un módulo Terraform completo en un entorno aislado.  
  - Verificar que los recursos fueron creados correctamente (por ejemplo, validar si un servicio existe o si una URL responde con **HTTP 200**).  
  - Consultar *outputs* (salidas) para verificaciones posteriores.

- **InSpec:**  
  Se menciona junto a Terratest como una herramienta que puede usarse para el testeo de infraestructura.

---


El **testing** es vital para el **control de calidad**, la **seguridad** y la **consistencia** de la infraestructura.  
Herramientas como **Terratest** son clave para las **pruebas funcionales** de módulos críticos, garantizando confianza, estabilidad y cumplimiento normativo en la infraestructura como código.



---
---

[Regresar](./2-%20Evaluación%20Final%20-%20Respuestas.md)

---
