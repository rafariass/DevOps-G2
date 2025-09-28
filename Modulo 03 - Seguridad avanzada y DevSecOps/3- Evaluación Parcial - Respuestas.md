# CURSO: DEVOPS SENIOR

## Integrantes
* Raul Farías
* Leonardo Oyarzun
* Sebastían Rivera
* Gabriel Castillo

# Módulo 3: Seguridad avanzada y DevSecOps

## Evaluación Parcial

## 1. ¿Qué es DevSecOps y cómo se diferencia de DevOps tradicional?

**DevSecOps** es una práctica que integra la seguridad en cada etapa del ciclo de vida del desarrollo de software. A diferencia del DevOps tradicional, que se enfoca en la colaboración entre desarrollo y operaciones, **DevSecOps** incorpora la seguridad como responsabilidad compartida desde el inicio, automatizando controles y pruebas de seguridad.

---

## 2. ¿Qué tipo de análisis realiza Trivy y en qué etapas puede integrarse?

**Trivy** realiza análisis de vulnerabilidades en imágenes de contenedores, dependencias de aplicaciones, archivos de configuración como `Dockerfile` y `Kubernetes YAML`, y código fuente. Puede integrarse en etapas tempranas del ciclo de desarrollo, como en el pipeline de `CI/CD`.

---

## 3. ¿Cómo ayuda Checkov a mejorar la seguridad de la infraestructura como código?

**Checkov** analiza archivos de infraestructura como código (`IaC`), como `Terraform`, `CloudFormation` y `Kubernetes`, para detectar configuraciones inseguras o que no cumplen con buenas prácticas. Esto permite corregir problemas antes de aplicar cambios en la infraestructura.

---

## 4. ¿Qué ventajas ofrece Vault frente a otras formas de gestionar secretos?

**Vault** ofrece almacenamiento seguro y centralizado para secretos, con control de acceso granular, auditoría y rotación automática de credenciales. Esto reduce el riesgo de exposición y mejora la gestión de credenciales en entornos dinámicos.

---

## 5. ¿Qué riesgos se mitigan al evitar que los secretos estén en manifiestos YAML o variables de entorno?

Se evita la exposición accidental de secretos en repositorios de código, logs o auditorías. También se reduce el acceso no autorizado y se facilita la rotación de credenciales sin modificar archivos de configuración.

---

## 6. ¿Cómo se puede asegurar un clúster Kubernetes frente a configuraciones por defecto?

Aplicando políticas `RBAC`, habilitando autenticación/autorización, restringiendo el acceso a la API, usando namespaces para aislar recursos, aplicando políticas de red, y auditando configuraciones con herramientas como `kube-bench` o `Trivy`.

---

## 7. ¿Cómo integrarías Snyk en el ciclo de desarrollo para proyectos Node.js o Python?

**Snyk** puede integrarse mediante plugins para IDEs, comandos en el pipeline de `CI/CD` y monitoreo de repositorios. Analiza dependencias en busca de vulnerabilidades y sugiere actualizaciones o parches para corregir problemas antes del despliegue.

---
