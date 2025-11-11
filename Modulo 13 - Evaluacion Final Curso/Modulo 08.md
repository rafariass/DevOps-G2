## Módulo 08 - Platform Engineering & Internal Developer Plataforms (IDP)


### OBJETIVO A MEDIR Y CRITERIO DE EVALUACIÓN

EMPLEAR METODOS DE DISEÑO DE PLATAFORMAS INTERNAS EN DEVELOPERS, DE ACUERDO A LAS PRACTICAS AVANZADAS
DE GITOPS, DEVSECOPS, KUBERNETES, OBSERVABILIDAD, IAC,FINOPS Y AIOPS:
- GitOps
- DevSecOps
- Kubernetes
- Observabilidad
- IaC (Infrastructure as Code)
- FinOps
- AIOps

### CONTENIDO QUE SE REVISA (**`MÓDULO 8`**)

- Backstage;
- GitOps stacks;
- Integración con CI/CD y observabilidad

### OBSERVACIÓN
Las interrogantes que se plantean en esta sección están diseñadas para evaluar la comprensión técnica y práctica del uso de Backstage como plataforma de integración para flujos GitOps, CI/CD y observabilidad. Verifican su capacidad para centralizar la gestión de servicios, mejorar la trazabilidad de despliegues y fortalecer la colaboración técnica en entornos DevOps modernos.


---

### PREGUNTAS 
---

### 29. ¿Qué función cumple Backstage en una organización DevOps moderna y qué beneficios ofrece a los equipos de desarrollo?

### Backstage: Portal Central de Autoservicio e Ingeniería DevOps

**Backstage** cumple una función estratégica dentro de una **organización DevOps moderna**, actuando como el **portal central de autoservicio** para los equipos de desarrollo y la **plataforma de desarrollo interno (IDP: Internal Developer Platform)**.  

Su propósito principal es **centralizar herramientas, documentación, servicios y flujos DevOps** en una única interfaz autogestionada y extensible.  
Esto transforma la experiencia del desarrollador, permitiéndoles enfocarse en **escribir código** en lugar de buscar recursos, entender integraciones o lidiar con procesos manuales.

---

### Función Principal de Backstage

En una organización que busca **estandarizar y escalar sus prácticas DevOps**, Backstage se convierte en el **centro neurálgico de la ingeniería moderna**.

### 1. Centralización y Descubrimiento
Backstage unifica el acceso a todos los **activos técnicos**, actuando como una **interfaz unificada** para:

- Documentar y descubrir servicios registrados.  
- Explorar dependencias, *ownership* (ver qué equipo es responsable), despliegues y cambios.  
- Integrar métricas, estado de CI/CD, seguridad y cumplimiento.

---

### 2. Estandarización y Creación de Recursos
A través de sus componentes clave, Backstage **estandariza la forma en que se construyen y gestionan los servicios**:

- **Software Catalog:**  
  Es el núcleo operativo del sistema, un catálogo que permite **registrar, descubrir y documentar** todos los servicios, librerías, herramientas, pipelines y recursos de la organización.  
  Este catálogo mejora la **visibilidad organizacional**.

- **Scaffolder:**  
  Permite la **creación guiada y automática** de nuevos recursos (servicios, paquetes, infraestructura, bases de datos) utilizando **plantillas predefinidas** por el equipo de plataforma.

---

### 3. Unificación de Experiencias DevOps
Backstage no reemplaza las herramientas existentes, sino que **las unifica en una experiencia consolidada**.  
Se integra con:

- **Repositorios:** GitHub, GitLab  
- **Herramientas CI/CD:** Jenkins, ArgoCD, GitHub Actions  
- **Observabilidad:** Prometheus, Grafana, Sentry

---

### Beneficios para los Equipos de Desarrollo

Backstage proporciona **beneficios técnicos y organizacionales clave**, principalmente enfocados en mejorar la **eficiencia** y la **autonomía del desarrollador**.

| **Beneficio** | **Detalle y Componentes Clave** |
|----------------|----------------------------------|
| **Mayor Autonomía y Autoservicio** | Promueve la cultura de autoservicio DevOps y fomenta la autonomía de los equipos. Reduce la dependencia de operadores o equipos SRE (*Site Reliability Engineering*). |
| **Aceleración del Desarrollo (Time-to-Market)** | El componente **Scaffolder** permite generar nuevos recursos rápidamente a través de plantillas, reduciendo errores humanos y acelerando el *time-to-market*. |
| **Visibilidad Unificada** | Permite a los desarrolladores consultar el estado de sus *builds*, despliegues, métricas y alertas sin salir de Backstage. Pueden ver el estado operativo de sus servicios en tiempo real desde un único punto de acceso. |
| **Mejora del Onboarding** | La plataforma estandariza la experiencia de desarrollo y acelera el onboarding de nuevos equipos. **TechDocs** facilita esto al consolidar la documentación técnica por servicio directamente en la interfaz. |
| **Estandarización y Gobernanza** | Centraliza documentación, *ownership* y métricas. Estandariza la creación de servicios y recursos, y mejora la trazabilidad de servicios. |
| **Reducción de Fricción** | Al combinar catalogación, documentación y visibilidad, reduce la fricción al trabajar en entornos complejos, permitiendo a los desarrolladores concentrarse en escribir código. |

---

### 30. ¿Qué elementos conforman un GitOps stack típico y cómo se relacionan con Backstage?

### GitOps Stack y su Integración con Backstage

El concepto de **GitOps Stack** es fundamental en el **Platform Engineering moderno** y representa la **aplicación escalada de los principios GitOps** a través de la organización.  

Un **GitOps Stack** es una **agrupación lógica y técnica de recursos** gestionados enteramente desde **Git como fuente de verdad**, utilizando **archivos declarativos**, **pipelines automatizados** y **agentes de sincronización**.

---

### 1. Elementos que Conforman un GitOps Stack Típico

Los **GitOps Stacks** están definidos por una **arquitectura en capas** y un **conjunto de herramientas orquestadas** que aseguran que el estado deseado de la infraestructura y las aplicaciones se mantenga de forma **declarativa**.

### Componentes Declarativos y Estructurales

1. **Repositorio Git (Fuente de Verdad):**  
   Contiene todos los archivos necesarios para definir el estado del sistema. Estos incluyen:
   - Manifiestos **YAML** (declaraciones de Kubernetes).  
   - **Helm Charts** o **Kustomize Overlays** (para plantillas reutilizables y gestión de diferencias multientorno).  
   - Configuraciones de **Terraform** o **Pulumi** (para infraestructura fuera de Kubernetes, como redes, bases de datos y clústeres en la nube).  
   - **Políticas de seguridad** y configuraciones específicas del stack.

2. **Configuraciones Específicas:**  
   Archivos para gestionar **secretos** (`Sealed Secrets`, `SOPS`, `Vault`) y **variables específicas** por stack o entorno.

3. **Estructura Modular por Capas:**  
   Una práctica recomendada es dividir los stacks por **dominios funcionales**, lo que permite un **control desacoplado** por diferentes equipos:
   - `bootstrap/` → controladores base del clúster  
   - `infrastructure/` → red, seguridad, almacenamiento  
   - `platform/` → herramientas compartidas (ArgoCD, Prometheus, Vault)  
   - `applications/` → microservicios de negocio

---

### Herramientas de Ejecución (Agents)

1. **Agente de Sincronización:**  
   Herramientas que monitorizan el repositorio Git y aplican los cambios automáticamente al entorno de destino (generalmente un clúster de Kubernetes).  
   Las principales son **ArgoCD** y **Flux**.

2. **CI Pipelines:**  
   Utilizados para validar, construir artefactos y, crucialmente, para **actualizar el repositorio GitOps** (no aplican los cambios directamente, sino que **desencadenan el sistema GitOps**).  
   Esto incluye validaciones previas como *pre-commit hooks* y *linters*.

---

### 2. Relación e Integración de GitOps Stacks con Backstage

**Backstage** funciona como la **Plataforma de Desarrollo Interno (IDP)** que **centraliza, visualiza y orquesta** la interacción de los desarrolladores con el **GitOps Stack**.  
La relación se da en tres aspectos clave:

---

###  A. Centro de Visibilidad Unificado

Backstage actúa como el **portal donde los desarrolladores pueden ver el estado del GitOps Stack** sin necesidad de interactuar directamente con el clúster o las herramientas GitOps subyacentes (como **ArgoCD** o **Flux**).

- **Estado Operativo:**  
  Mediante plugins (como `backstage-plugin-argo-cd`), Backstage se integra con las herramientas de sincronización GitOps para mostrar el estado de sincronización de las aplicaciones, los despliegues y la versión actual.  

- **Visibilidad Completa:**  
  Permite a los desarrolladores consultar **métricas, estado de CI/CD, despliegues y alertas** sin salir de Backstage.  
  Toda la información asociada a un servicio registrado en el **Software Catalog** está centralizada, incluyendo su **estado operacional en el stack GitOps**.

---

###  B. Mecanismo de Creación Asistida (Scaffolder y Catálogo)

Backstage utiliza el componente **Scaffolder** para **iniciar el flujo GitOps**, garantizando **estandarización desde el momento de la creación**.

- **Generación Automatizada:**  
  El Scaffolder permite a los desarrolladores generar nuevos recursos (por ejemplo, un microservicio o infraestructura base) utilizando **plantillas predefinidas** alineadas con los estándares GitOps.  

- **Integración Scaffolder-GitOps:**  
  El Scaffolder puede integrarse con GitOps al generar automáticamente los **repositorios**, las **pipelines de CI/CD** y la **configuración de despliegue** (los manifiestos iniciales del Stack).  

- **Registro en el Catálogo:**  
  Una vez creado un servicio (y su configuración GitOps asociada), este se registra automáticamente en el **Software Catalog** de Backstage como una entidad.  
  Los servicios se declaran mediante un archivo `catalog-info.yaml` dentro del repositorio GitOps, permitiendo a Backstage detectarlos y actualizar su estado.

---

###  C. Desencadenamiento y Control del Flujo

Aunque el CI/CD realiza la construcción, **Backstage puede servir como el punto de inicio para la visualización o monitoreo de los cambios** que ocurren en el GitOps Stack.

- Backstage se integra con las **pipelines de CI/CD**, permitiendo monitorear los resultados del *build* y del **despliegue GitOps**.  
- El objetivo final es que **Backstage sea el centro de comando DevOps unificado**, donde la estructura modular del GitOps Stack se refleje en la interfaz para **simplificar las operaciones diarias**.

---

 ### 31. ¿Cómo mejora la trazabilidad y el control operativo al integrar CI/CD y observabilidad en Backstage?
 
### Integración de CI/CD y Observabilidad en Backstage

La **integración de CI/CD (Integración/Entrega Continua)** y **Observabilidad** en **Backstage** mejora drásticamente la **trazabilidad** y el **control operativo**, al transformar el portal en el **centro neurálgico unificado** donde el estado de la entrega se conecta directamente con el comportamiento en tiempo real de los servicios en producción.  

Esta integración permite pasar de un proceso de despliegue meramente técnico a un proceso **seguro, trazable y medible**.

---

### I. Mejora en la Trazabilidad

La trazabilidad se mejora al establecer un **vínculo auditable e inmutable** entre el cambio (el código que se entrega) y el efecto (el estado operativo).

1. **Trazabilidad del Comienzo al Fin:**  
   La integración con observabilidad garantiza la trazabilidad completa del ciclo, desde el commit de código hasta el contenedor en producción.  
   Esto significa que cada cambio —sea código, infraestructura o configuración— queda completamente trazado, medido y monitoreado.

2. **Visibilidad Unificada y Catálogo de Servicios:**  
   Backstage centraliza el acceso a todos los activos técnicos. El **Software Catalog (Catálogo de Servicios)** es el núcleo que hace posible esta trazabilidad.  
   Cada servicio o componente en el catálogo permite al desarrollador acceder a:
   - El código fuente, los pipelines (CI/CD) y las métricas de observabilidad desde un único lugar.  
   - El estado de sus builds, despliegues, métricas y alertas sin salir de la plataforma.  
   - La documentación y el ownership (quién es el responsable).

3. **Metadatos de Seguimiento:**  
   Los pipelines de CI/CD modernos se conectan con los sistemas de observabilidad (como **Prometheus**, **Grafana**, **Jaeger**) para inyectar **etiquetas y metadatos** en los logs y trazas generadas para cada build.  
   Esto crea una *huella observable* que facilita la correlación entre los cambios de código y los eventos operacionales.

4. **Aceleración de Onboarding:**  
   Backstage mejora la trazabilidad de servicios y acelera el onboarding de nuevos equipos al unificar la experiencia de desarrollo.

---

### II. Mejora en el Control Operativo

La integración con observabilidad proporciona un **lazo de retroalimentación crítica** al proceso de entrega, permitiendo a la plataforma tomar **decisiones de control basadas en datos**.

1. **Validación Post-Despliegue y Control Basado en Datos:**  
   El control operativo se eleva al incluir **validaciones automáticas post-deploy** y *health checks*.  
   Las decisiones de despliegue se basan en **datos y métricas reales**, no en percepciones.

2. **Instrumentación y Registro Automático:**  
   El flujo de CI/CD automatiza la instrumentación, garantizando que los nuevos servicios estén correctamente monitoreados desde el momento de su creación:
   - Registra los nuevos servicios en el catálogo de Backstage.  
   - Inyecta anotaciones en el despliegue para activar la recolección de métricas y trazas (por ejemplo: `prometheus.io/scrape: true`).  
   - Configura alertas iniciales (Uptime, CPU, errores) asociadas al servicio desplegado.

3. **Automatización de Políticas y Despliegues Condicionales (Gates):**  
   El sistema de observabilidad puede condicionar la ejecución del pipeline, aplicando un control de seguridad avanzado.  
   Los pipelines pueden implementar **gates condicionales** y **hooks** que:
   - Bloquean nuevos despliegues si un servicio tiene incidentes activos.  
   - Invalidan el paso a producción si no se emiten trazas o si falla un escaneo de seguridad.  
   - Analizan el comportamiento post-despliegue (por ejemplo, latencia *p95* o incremento de errores *5xx*).  
   - Activan un rollback automático si el umbral de error supera el límite definido.

4. **Visualización en Tiempo Real:**  
   Al conectar Backstage con herramientas de observabilidad como **Prometheus** y **Grafana**, los desarrolladores pueden ver el **estado operativo de sus servicios en tiempo real** desde un único punto de acceso, lo cual mejora la **capacidad de respuesta** y **mitiga problemas antes de que escalen**.
   
 ---
 
### 32. ¿Qué impacto tiene el uso de un Developer Portal como Backstage en la cultura de DevOps y la colaboración entre equipos?
# Impacto de un Developer Portal como Backstage en la Cultura DevOps y la Colaboración

El uso de un **Portal de Desarrolladores (Developer Portal)** como **Backstage** tiene un **impacto transformador y estratégico** en la cultura de **DevOps** y en la **colaboración entre equipos**, principalmente al **centralizar la experiencia de ingeniería** y **promover la autonomía**.  

Backstage no es solo una herramienta de visualización, sino una **plataforma estratégica de aceleración del desarrollo interno**, convirtiéndose en el **centro neurálgico de la ingeniería moderna**.

---

### Impacto en la Cultura DevOps

El impacto en la cultura DevOps se centra en la **estandarización**, la **autonomía** y el **enfoque en el valor**, eliminando la fricción operativa.

1. **Promoción de la Cultura de Autoservicio DevOps:**  
   Backstage promueve activamente la cultura de autoservicio DevOps.  
   Al funcionar como un portal central de autoservicio, permite a los desarrolladores:
   - Crear servicios, paquetes, infraestructura y bases de datos desde plantillas prediseñadas.  
   - Generar nuevos recursos de forma automática y guiada mediante el **Scaffolder**.  
   - Consultar el estado de sus builds, despliegues, métricas y alertas sin salir de Backstage.

2. **Estandarización y Gobernanza:**  
   La plataforma estandariza la creación de servicios y recursos.  
   Este enfoque facilita que los equipos de plataforma definan un conjunto de flujos genéricos adaptables, garantizando la **gobernanza** y la **trazabilidad de cambios**.

3. **Aceleración y Enfoque:**  
   El propósito principal de Backstage es permitir a los desarrolladores **concentrarse en escribir código** en lugar de buscar recursos, entender integraciones o lidiar con procesos manuales.  
   Esto se traduce en un **aumento de la productividad** y una **reducción del tiempo de entrega**.

4. **Reducción de Dependencia Operacional:**  
   Al automatizar y estandarizar tareas mediante plantillas (**Scaffolder**), Backstage ayuda a **reducir la dependencia** de operadores o equipos **SRE (Site Reliability Engineering)**.

---

### Impacto en la Colaboración y Autonomía de Equipos

Backstage mejora la **colaboración** al proporcionar una **fuente de verdad única** y al hacer explícitas las responsabilidades.

1. **Visibilidad Organizacional y Reducción de Fricción:**  
   El **Software Catalog** es un componente clave.  
   Permite a los desarrolladores:
   - Descubrir todos los servicios activos.  
   - Ver qué equipo es responsable (*ownership*).  
   - Navegar dependencias.  
   - Acceder al código fuente, pipelines, documentación y métricas desde un único lugar.  
   Esta centralización mejora la **visibilidad organizacional** y **reduce la fricción** al trabajar en entornos complejos.

2. **Centralización de la Documentación:**  
   Backstage centraliza **documentación, ownership y métricas**.  
   A través de **TechDocs**, la documentación técnica se consolida por servicio o componente y es visible directamente desde la interfaz.  
   Esto **facilita el onboarding** de nuevos desarrolladores y establece **estándares documentales transversales** en toda la organización.

3. **Unificación de la Experiencia:**  
   Backstage unifica las herramientas existentes (como **GitHub**, **Jenkins**, **ArgoCD**, **Prometheus**, **Grafana**) en una **única experiencia integrada**.  
   Los desarrolladores pueden ver el **estado operativo de sus servicios en tiempo real**.  
   Esta interfaz unificada garantiza que todos los equipos utilicen los mismos flujos y accedan a la misma información.

4. **Fomento de la Colaboración en Infraestructura:**  
   El modelo de **GitOps Stacks** (que Backstage visualiza) promueve la **colaboración en la infraestructura como código**, siguiendo el mismo modelo de trabajo que se usa para el **desarrollo de software**.

---


[Regresar](./2-%20Evaluación%20Final%20-%20Respuestas.md)

---
