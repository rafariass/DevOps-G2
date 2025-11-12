## Módulo 10 - AIOps & Incident MAnagement

### OBJETIVO A MEDIR Y CRITERIO DE EVALUACIÓN

Aplica métodos de **`gestión de incidentes`**, de acuerdo a las prácticas avanzadas de:
- GitOps
- DevSecOps
- Kubernetes
- Observabilidad
- IaC (Infrastructure as Code)
- FinOps
- AIOps

### CONTENIDO QUE SE REVISA (**`MÓDULO 10`**)

- AIOps tools (Moogsoft, PagerDuty AIOps)
- auto-remediation
- Reducción de MTTR

### OBSERVACIÓN

Las preguntas que se plantean a continuación se han formulado para evaluar la comprensión técnica y aplicada del uso de herramientas AIOps como Moogsoft o PagerDuty AIOps. Permiten comprobar su capacidad para diseñar estrategias de auto-remediación y aplicar automatización inteligente orientada alareducción del MTTR, optimizando así la gestión de incidentes enentornos complejos.

---

### PREGUNTA DE DESARROLLO

### 37. ¿Cómo utiliza una plataforma AIOps la correlación de eventos para reducir elMTTR en la gestión de incidentes?

Las plataformas **`AIOps`** básicamente usan inteligencia artificial para juntar miles de alertas y encontrar qué cosas están realmente relacionadas entre sí. En lugar de que me lleguen cien notificaciones por el mismo problema, AIOps las agrupa y me muestra un solo incidente con toda la info que necesito.

Por ejemplo, si se cae un servicio y eso genera alertas en la red, la base de datos y los logs, **`AIOps`** entiende que todo viene del mismo error y me lo muestra como un solo caso. Así no pierdo tiempo buscando la causa en mil lugares distintos.

Con eso, el tiempo de recuperación (**MTTR**) baja mucho, porque paso directo de ver el síntoma a entender la causa y aplicar una solución rápida o incluso automática.

---

### 38. Expliquen qué es la auto-remediación en el contexto de AIOps y den un ejemplo de cómose aplicaría en un clúster de Kubernetes.

La `auto-remediación` es cuando el sistema se arregla solo sin que tenga que meter mano. En **`AIOps`** esto se logra con reglas y automatizaciones que actúan apenas se detecta un problema.

Por ejemplo, en un clúster de **Kubernetes**, si un `pod` entra en `crashloop`, el sistema puede detectar eso con `Prometheus` o `Moogsoft` y ejecutar un `rollback automático` a la versión anterior. También puede reiniciar el `pod`, verificar que se levante bien y avisar por Slack, Temas o Jira que ya se resolvió.

La idea es que los errores comunes se arreglen solos y uno solo intervenga cuando realmente es algo nuevo o más grave.

---

### PREGUNTA DE INTEGRACIÓN Y REFLEXIÓN

### 39. ¿Por qué la Observabilidad es un pilar fundamental para que las herramientas AIOpspuedan tomar decisiones de remediación efectivas?

Porque sin observabilidad **`AIOps`** estaría ciego.

Estas herramientas necesitan datos (`logs`, `métricas`, `trazas`) para entender qué pasa en tiempo real. Si no tengo eso bien configurado, **`AIOps`** no puede detectar anomalías ni decidir qué acción tomar.

Con una buena observabilidad (tipo `Prometheus`, `Datadog`, `OpenTelemetry`, etc.), **`AIOps`** puede ver patrones raros, encontrar la causa raíz y validar si la solución funcionó.

En resumen, la observabilidad es como los ojos y oídos del sistema: sin ella, la **“inteligencia”** no sirve de nada.

---

### 40. ¿De qué manera una estrategia de AIOps que reduce el MTTR puede impactarpositivamente en las prácticas de FinOps y DevSecOps?

Cuando bajás el **MTTR** con **`AIOps`**, ganás por todos lados.

En **FinOps**, te ahorrás dinero. Menos tiempo caído = menos pérdida de dinero, menos sobrecarga de recursos, y menos necesidad de tener servidores de más **“por si acaso”**.

En **`DevSecOps`**, ganás seguridad. Si **`AIOps`** detecta y corrige rápido un problema o una brecha (por ejemplo, un contenedor comprometido), el riesgo baja considerablemente. Además, todo queda registrado, así que después podés analizar y mejorar las políticas.

En pocas palabras, **`AIOps`** hace que todo el ecosistema sea más inteligente, más barato y más seguro.

---

[Regresar](./2-%20Evaluación%20Final%20-%20Respuestas.md)

---
