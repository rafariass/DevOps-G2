# CURSO: DEVOPS SENIOR

## Integrantes
* Raul Farías
* Leonardo Oyarzun
* Sebastían Rivera
* Gabriel Castillo

# Módulo 2: Automatización con IA en DevOps

## Evaluación Parcial

### 1. ¿Cuál es el rol de ChatGPT en flujos de desarrollo inteligente?

**ChatGPT** actúa como un **asistente cognitivo** en los flujos de desarrollo inteligente, diseñado para potenciar la eficiencia operativa en diversas fases del ciclo DevOps. Sus roles principales incluyen:

*   **Generación de Código y Scripts**: Facilita la creación de plantillas para Infraestructura como Código (IaC) como Terraform, Ansible o Dockerfiles, y la configuración de manifiestos de Kubernetes.
*   **Análisis y Depuración**: Ayuda a interpretar trazas de errores y logs complejos de sistemas como CI/CD, contenedores o microservicios, sugiriendo posibles soluciones.
*   **Automatización de Tareas**: Asiste en la generación de *pipelines* de CI/CD para herramientas como Jenkins, GitLab CI o GitHub Actions y sugiere flujos de trabajo para GitOps.
*   **Documentación Técnica**: Automatiza la redacción de `READMEs`, `changelogs`, comentarios en el código y políticas de seguridad.
*   **Soporte Interno**: Puede ser desplegado como un "DevOps Copilot" a través de una API para que los equipos de desarrollo resuelvan dudas técnicas de forma autónoma.

---

### 2. ¿Qué ventajas ofrece GitHub Copilot para desarrolladores?

**GitHub Copilot** es un asistente de codificación contextual que se integra directamente en el IDE y ofrece ventajas significativas para acelerar y mejorar el desarrollo.

*   **Autocompletado Inteligente**: Sugiere líneas de código completas o incluso funciones enteras en múltiples lenguajes como Python, YAML, Go y JavaScript.
*   **Generación a partir de Lenguaje Natural**: Es capaz de generar código a partir de comentarios que describen la lógica deseada.
*   **Reducción de Código Repetitivo**: Optimiza y automatiza la escritura de código repetitivo (*boilerplate*), permitiendo al desarrollador enfocarse en la lógica de negocio.
*   **Creación de Pruebas**: Facilita la generación de pruebas unitarias y funciones auxiliares.
*   **Aplicaciones DevOps**: Es especialmente útil en la creación de scripts de IaC, pipelines de CI/CD y manifiestos de Kubernetes o Helm.

---

### 3. ¿Cómo se pueden integrar APIs personalizadas en un flujo CI/CD?

Las APIs personalizadas que utilizan modelos de lenguaje se pueden integrar en un flujo CI/CD para actuar como un componente más del *pipeline*. La integración se realiza mediante llamadas a un *backend* (creado con herramientas como FastAPI, Flask o Express) desde el orquestador de CI/CD.

Por ejemplo, dentro de un *pipeline* en **GitHub Actions, GitLab CI o Jenkins**, se puede añadir un *job* o *step* que invoque a la API para:
*   **Revisar el código** y emitir sugerencias de seguridad o estilo.
*   **Analizar logs** de una etapa de despliegue para detectar anomalías.
*   **Generar plantillas de configuración** dinámicamente según los parámetros del *pipeline*.

---

### 4. ¿Qué desafíos presenta el uso de IA en tareas de desarrollo?

El uso de IA en el desarrollo presenta desafíos estratégicos y de seguridad que deben ser gestionados por un perfil senior. Los principales son:

*   **Seguridad y Privacidad**: Es fundamental no enviar datos sensibles, secretos o código propietario a los modelos de IA sin aplicar técnicas de sanitización o anonimización previas.
*   **Dependencia Operacional**: La IA debe ser una herramienta de apoyo, no un decisor autónomo. En entornos críticos, siempre se requiere supervisión humana para validar sus resultados.
*   **Precisión y Calidad**: El código o las configuraciones generadas por la IA no siempre son óptimos o correctos. Es indispensable que toda salida pase por procesos de revisión, pruebas automatizadas y validación cruzada.
*   **Control y Gobernanza**: Es necesario definir políticas claras sobre el uso ético y efectivo de la IA generativa y auditar las decisiones tomadas por estos sistemas.

---

### 5. ¿Qué precauciones deben tomarse al usar ChatGPT o Copilot en entornos productivos?

Al utilizar herramientas como ChatGPT o Copilot en entornos productivos, se deben adoptar las siguientes precauciones críticas:

*   **Proteger Datos Sensibles**: Nunca se deben exponer secretos, credenciales o información confidencial de la empresa en los *prompts* o el código analizado por el modelo.
*   **Supervisión Humana Obligatoria**: La IA debe ser considerada un asistente y no un decisor autónomo. Un profesional debe revisar y validar todo el código, *scripts* o configuraciones antes de aplicarlos en producción.
*   **Validación y Pruebas**: Todo artefacto generado por IA (código, Dockerfiles, etc.) debe ser sometido a los mismos controles de calidad que el código escrito manualmente, incluyendo pruebas automatizadas y revisiones de pares.
*   **Conocer sus Limitaciones**: El equipo debe entender que los modelos pueden cometer errores o generar código inseguro. Es crucial no confiar ciegamente en sus sugerencias.

---

### 6. ¿Cómo puede una API personalizada beneficiarse de la inteligencia artificial?

Una API personalizada se beneficia de la IA al incorporar capacidades cognitivas que transforman tareas manuales en procesos automatizados e inteligentes. Sus principales beneficios son:

*   **Creación de Asistentes Internos**: Permite desarrollar *chatbots* de soporte técnico que respondan preguntas frecuentes sobre la infraestructura o los procesos de la organización.
*   **Generación de Código Estandarizado**: Puede generar automáticamente componentes de código o plantillas de configuración (ej. un microservicio base) que sigan las mejores prácticas y estándares internos.
*   **Análisis y Validación Automatizada**: La API puede analizar código para detectar incumplimientos de políticas de estilo, seguridad o rendimiento, emitiendo sugerencias de mejora.
*   **Interpretación de Datos no Estructurados**: Es capaz de analizar y clasificar logs o alertas, generando resúmenes, reportes automatizados o sugerencias de acción para la resolución de incidentes.

---

### 7. ¿Qué herramientas permiten automatizar flujos CI/CD con integración de IA o APIs externas?

La automatización de flujos CI/CD con IA se logra combinando plataformas de orquestación estándar con las APIs de los modelos de lenguaje. Las herramientas clave son:

*   **Plataformas de CI/CD**: **GitHub Actions, GitLab CI, Jenkins y Argo Workflows** son los orquestadores donde se definen los *pipelines* que pueden invocar a los servicios de IA.
*   **APIs de Modelos de IA**: La **OpenAI API, Azure Cognitive Services o los modelos de HuggingFace Transformers** proporcionan el motor de inteligencia que se integra en los flujos.
*   **Frameworks de Orquestación de IA**: Herramientas como **LangChain** permiten construir lógicas complejas y agentes de IA que luego son expuestos a través de un *backend*.
*   **Backends para la API**: Frameworks como **FastAPI, Flask o Express** se utilizan para crear la API que encapsula la lógica de IA y que es llamada desde el *pipeline* de CI/CD.

---
