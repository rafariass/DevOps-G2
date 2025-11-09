## Módulo 06 - Service Mesh & Networking moderno

### Pregunta de desarrollo:

### 21. ¿Cuál es la diferencia entre Istio y Linkerd en términos de arquitectura y complejidad de uso?

| Característica | Istio | Linkerd |
| ---- | ---- | ---- |
| Arquitectura | Alto nivel de funcionalidades, con Control Plane centralizado (istiod). | Más liviano y minimalista. Utiliza proxies ultra livianos (basados en Rust). |
| Complejidad de Uso | Curva de aprendizaje elevada y complejidad operativa significativa. | Fácil de instalar, operar y entender, con bajo overhead. |

---

### 22. ¿Qué es mTLS en el contexto de un Service Mesh y por qué es importante para la seguridad entre servicios?

¿Qué es mTLS? mTLS (mutual TLS) es una extensión del protocolo TLS donde ambos extremos de la conexión (cliente y servidor) se autentican mutuamente mediante certificados digitales válidos.
Importancia en un Service Mesh:
En un Service Mesh, mTLS se implementa automáticamente mediante los proxies sidecar (como Envoy en Istio). Es importante para la seguridad porque:
* Autenticación Mutua: Garantiza la autenticidad y confidencialidad, previniendo ataques de intermediario (MITM) y suplantaciones.
* Seguridad por Defecto: Toda comunicación interna es cifrada y autenticada, lo que reduce la exposición a tráfico no autorizado.
* Base para Políticas L7: Permite la aplicación de políticas de autorización L7 basadas en la identidad de servicio (en lugar de solo direcciones IP).
* Cumplimiento Normativo: Es esencial para el cumplimiento normativo en industrias reguladas.

---

### Pregunta de integración y reflexión:

### 23. ¿Cómo mejora la gestión del tráfico en un entorno con Istio o Linkerd comparado con un balanceador de carga tradicional?

La mejora radica en la capacidad del Service Mesh para gestionar el tráfico en la Capa de Aplicación (L7), proporcionando inteligencia y control que un balanceador tradicional (L4) no ofrece:
* Despliegues Progresivos: Permite técnicas avanzadas como Canary releases (enrutamiento de tráfico a una pequeña porción de usuarios para pruebas) y Blue/Green deployments.
* Resiliencia: Incorpora mecanismos de mitigación de errores como Circuit breakers (para evitar fallos en cascada), Retries y timeouts inteligentes.
* Observabilidad L7: El Service Mesh captura automáticamente telemetría, métricas, logs y tracing del tráfico interno, permitiendo analizar latencia y tasas de error por servicio.
* Seguridad Integrada: El Service Mesh implementa mTLS automático entre servicios, asegurando la comunicación interna.

---

### 24. ¿Por qué es valiosa la observabilidad L7 (capa de aplicación) en un entorno con microservicios, y cómo contribuyen herramientas como Kiali, Grafana o Jaeger?

La observabilidad L7 (capa 7 o aplicación) es esencial en microservicios porque permite inspeccionar, medir y trazar el tráfico a nivel de protocolo HTTP. Su valor radica en poder responder a preguntas de negocio y operacionales: ¿Qué endpoints están fallando? ¿Cuál es la latencia por ruta?. Permite la identificación de cuellos de botella y anomalías con precisión.

Contribución de Herramientas:
* Grafana: Visualiza métricas L7 (Request rate, Error rate, Latencia P95/P99) que se recogen a través del Service Mesh o Prometheus.
* Jaeger (o Tempo): Proporciona la visualización y análisis de las trazas distribuidas, permitiendo rastrear cuánto tiempo consume cada microservicio en una transacción completa.
* Kiali: Proporciona observabilidad específica para Istio, permitiendo inspeccionar el flujo de tráfico entre servicios en tiempo real y visualizar la topología de la malla.
La observabilidad L7 reduce el MTTR (Mean Time to Recovery) al facilitar la identificación del punto exacto de falla.


---

[Regresar](./2-%20Evaluación%20Final%20-%20Respuestas.md)

---
