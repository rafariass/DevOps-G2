# CURSO: DEVOPS SENIOR

## Integrantes
* Raul Farías
* Leonardo Oyarzun
* Sebastían Rivera
* Gabriel Castillo

# Módulo 6: Service Mesh & Networking moderno

## Evaluación Parcial

### 1. ¿Qué es un Service Mesh y qué problemas busca resolver?

Un **Service Mesh** (Malla de Servicios) es una capa de infraestructura dedicada a gestionar la comunicación entre los distintos microservicios dentro de una aplicación distribuida, como un clúster de Kubernetes. Su función principal es abstraer, proteger y observar el tráfico de red sin necesidad de modificar el código de las aplicaciones.

Busca resolver los desafíos inherentes a las arquitecturas de microservicios, tales como:
* **Gestión de Tráfico Avanzada**: Proporciona balanceo de carga inteligente (L7), enrutamiento dinámico y despliegues controlados.
* **Seguridad**: Automatiza la seguridad en las comunicaciones entre servicios, principalmente a través de mTLS (mutual TLS) para cifrar el tráfico y autenticar las identidades.
* **Resiliencia**: Implementa patrones de resiliencia como reintentos (`retries`), tiempos de espera (`timeouts`) y cortocircuitos (`circuit breakers`) para evitar fallos en cascada.
* **Observabilidad**: Ofrece una visión profunda del comportamiento del sistema mediante la recolección automática de métricas, logs y trazas distribuidas del tráfico L7.

---

### 2. ¿Qué diferencias fundamentales existen entre Istio y Linkerd?

La elección entre Istio y Linkerd depende del equilibrio deseado entre la potencia de configuración y la simplicidad operativa.

| Característica           | Istio                                                                                                                            | Linkerd                                                                                                                          |
| :----------------------- | :------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------- |
| **Enfoque**              | Potente, altamente configurable y con un gran nivel de funcionalidades, ideal para entornos empresariales complejos.              | Ligero, simple y enfocado en el alto rendimiento. Diseñado para ser fácil de instalar y operar con un bajo consumo de recursos.  |
| **Complejidad**          | Presenta una curva de aprendizaje elevada y mayor complejidad operativa.                                                         | Es rápido de implementar y su arquitectura es minimalista, lo que reduce la sobrecarga operativa.                                |
| **Control de Tráfico**    | Ofrece control de tráfico muy avanzado y granular, permitiendo `A/B testing`, `canary releases` y `mirroring` con gran precisión. | Proporciona un control de tráfico más simple y menos granular que Istio.                                                          |
| **Rendimiento**          | Puede tener un consumo de recursos significativo, especialmente en clústeres pequeños o medianos.                                 | Utiliza proxies ultraligeros basados en Rust, lo que resulta en un bajo consumo de CPU y memoria.                                |
| **Comunidad**            | Cuenta con una comunidad muy amplia, abundante documentación y un gran ecosistema de integraciones.                              | Su comunidad es más pequeña y tiene menos integraciones nativas con herramientas externas en comparación con Istio.              |

---

### 3. ¿Qué tipos de ruteo de tráfico se pueden configurar con un Service Mesh?

Un Service Mesh permite implementar patrones de gestión de tráfico avanzados en la capa de aplicación (L7), lo que otorga un control preciso sobre cómo fluyen las solicitudes entre servicios. Los principales tipos de enrutamiento son:

* **Enrutamiento basado en reglas**: Permite dividir el tráfico entre diferentes versiones de un servicio según un porcentaje definido (p. ej., 90% a la v1 y 10% a la v2).
* **Canary Releases**: Libera una nueva versión del servicio a un pequeño subconjunto de usuarios para validar su comportamiento en producción antes de un despliegue completo.
* **Blue/Green Deployments**: Mantiene dos versiones idénticas de la aplicación (una "azul" activa y una "verde" nueva) y redirige todo el tráfico de una a otra de forma instantánea una vez que la nueva versión ha sido validada.
* **Mirroring de Tráfico (Shadowing)**: Clona el tráfico de producción y lo envía a una nueva versión del servicio sin que las respuestas de esta afecten al usuario final. Esto es útil para probar el rendimiento y el comportamiento bajo carga real.
* **Segmentación de Tráfico**: Enruta las solicitudes basándose en atributos de la capa 7, como cabeceras HTTP (`headers`), tokens JWT, geolocalización o cookies del usuario.

***

### 4. ¿Cómo mejora la observabilidad L7 en comparación con monitoreo tradicional?

La observabilidad L7 (capa de aplicación) ofrece una visión mucho más profunda y contextualizada del comportamiento del sistema que el monitoreo tradicional, que se enfoca en las capas de red L3/L4 (IPs, puertos, conexiones).

La mejora radica en su capacidad para responder preguntas complejas sobre el rendimiento de la aplicación, como:
* ¿Qué `endpoints` específicos están fallando y con qué códigos de error HTTP (404, 500, etc.)?
* ¿Cuál es la latencia exacta de cada ruta de una API?
* ¿Qué microservicio está causando un cuello de botella en una transacción que involucra a múltiples servicios?

Para lograrlo, la observabilidad L7 se compone de tres pilares:
1.  **Métricas**: Datos numéricos agregados como la tasa de peticiones (RPS), la tasa de errores y las latencias P95/P99 por servicio y `endpoint`.
2.  **Logs Estructurados**: Registros detallados de cada petición con contexto de la aplicación, como la ruta, el método HTTP, el ID de usuario y el ID de la traza.
3.  **Trazas Distribuidas**: Visualización del ciclo de vida completo de una petición a medida que viaja a través de los diferentes microservicios, mostrando dónde se acumula la latencia y dónde ocurren los errores.

---

### 5. ¿Qué herramientas se integran comúnmente con Istio para visualizar trazas y métricas?

Istio se integra nativamente con un ecosistema de herramientas de código abierto muy populares para construir una plataforma de observabilidad completa. Las principales son:
* **Prometheus**: Para la recolección y almacenamiento de métricas de series temporales.
* **Grafana**: Para la creación de dashboards y la visualización de las métricas recolectadas por Prometheus.
* **Jaeger**: Para el almacenamiento y la visualización de trazas distribuidas, permitiendo analizar el flujo de las peticiones entre servicios.
* **Kiali**: Una herramienta de observabilidad específica para Istio que ofrece una topología del service mesh, mostrando cómo se comunican los servicios en tiempo real y permitiendo visualizar configuraciones de tráfico y seguridad.

---

### 6. ¿Cómo funciona la seguridad mTLS dentro de un Service Mesh?

**Mutual TLS (mTLS)** es un protocolo que garantiza que tanto el cliente como el servidor en una comunicación se autentiquen mutuamente usando certificados digitales, además de cifrar el tráfico. Dentro de un Service Mesh, este proceso es completamente transparente y automático para las aplicaciones.

El funcionamiento es el siguiente:
1.  **Provisión de Identidad**: El plano de control del Service Mesh (ej. `Istiod` en Istio) actúa como una Autoridad Certificadora (CA) y emite un certificado X.509 único y su clave privada para cada microservicio.
2.  **Inyección del Proxy**: Estos certificados son montados en el proxy `sidecar` (ej. Envoy) que se ejecuta junto a cada aplicación.
3.  **Handshake mTLS**: Cuando un servicio A quiere comunicarse con un servicio B, sus proxies `sidecar` realizan un "saludo" (handshake) mTLS:
    * El proxy de A presenta su certificado al proxy de B.
    * El proxy de B valida la identidad de A y responde presentando su propio certificado.
    * El proxy de A valida la identidad de B.
4.  **Conexión Segura**: Si ambas validaciones son exitosas, se establece un canal de comunicación cifrado y mutuamente autenticado entre los dos proxies.

Este mecanismo previene ataques de suplantación de identidad (`spoofing`) y de intermediario (`Man-in-the-Middle`), asegurando que solo los servicios autorizados puedan comunicarse dentro del clúster.

---

### 7. ¿Qué impacto puede tener un Service Mesh en el rendimiento y cómo mitigarlo?

La introducción de un Service Mesh puede tener un impacto en el rendimiento debido a la sobrecarga (`overhead`) que generan los proxies `sidecar`. Los principales impactos son:

* **Aumento del consumo de recursos**: Los proxies consumen CPU y memoria adicionales en cada `pod`, lo que puede ser significativo en clústeres grandes o con recursos limitados. El proceso de cifrado y descifrado de mTLS, en particular, aumenta el uso de CPU.
* **Incremento de la latencia**: Cada llamada de red entre servicios ahora pasa por dos proxies (el del cliente y el del servidor), lo que añade un pequeño salto de red y, por tanto, una latencia adicional.

Para **mitigar** este impacto, se pueden tomar las siguientes medidas:
* **Elegir el Service Mesh adecuado**: Para entornos donde el rendimiento y la simplicidad son críticos, **Linkerd** es una opción preferible, ya que está diseñado para ser ultraligero y tener un bajo consumo de recursos.
* **Optimizar la configuración**: En herramientas como Istio, se pueden ajustar los recursos asignados a los proxies y limitar el alcance del `mesh` solo a los `namespaces` que lo requieran.
* **Adoptar arquitecturas modernas**: Istio está evolucionando hacia un modelo de **Ambient Mesh**, donde los `sidecars` se vuelven opcionales y se reemplazan por un agente a nivel de nodo, reduciendo significativamente la carga operativa y el consumo de recursos por `pod`.
---
