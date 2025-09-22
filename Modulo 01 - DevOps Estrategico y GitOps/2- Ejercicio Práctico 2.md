# 🚀 Módulo 1: DevOps Estratégico y GitOps

## Ejercicio Práctico 2
Simulación de Pipeline GitOps CI/CD Seguro con ArgoCD + Kustomize + External Secrets

### 🧰 Instalación de Herramientas CLI
En este primer paso, instalamos todas las herramientas de línea de comandos (CLI) necesarias para interactuar con Kubernetes, gestionar las configuraciones y comunicarnos con los diferentes componentes del ecosistema.

```bash
# Instalar kubectl CLI, para interactuar con el clúster de Kubernetes
brew install kubectl

# Instalar kustomize CLI, para gestionar las configuraciones de Kubernetes
brew install kustomize

# Instalar Helm CLI, para gestionar paquetes de Kubernetes
brew install helm

# Instalar ArgoCD CLI, para interactuar con el servidor de ArgoCD
brew install argocd

# Instalar AWS CLI (versión 2), para gestionar secretos con LocalStack de AWS
brew install awscli
```

---

### 📦 Creación y Clonación del Repositorio Git
A continuación, creamos un repositorio en GitHub que servirá como la "fuente de verdad" para nuestra configuración. ArgoCD monitoreará este repositorio para aplicar los cambios en el clúster.

```bash
# Navega a una ubicación de trabajo, como el Escritorio
cd ~/Desktop

# Crea un nuevo repositorio público en GitHub y clónalo localmente
gh repo create learning-gitops --public --clone
```

---

### 📁 Creación de la Estructura de Directorios
Ahora, definimos la estructura de carpetas que sigue el patrón de Kustomize, separando la configuración base (común a todos los entornos) de los overlays (personalizaciones para entornos específicos como `dev`, `stage` y/o `prod`).

```bash
# Entra en el directorio del repositorio recién creado
cd learning-gitops

# Crea la estructura de directorios para los entornos y aplicaciones de ArgoCD
mkdir -p argocd/applications base overlays/{dev,stage,prod}
```

---

### 🌱 Definición de la Configuración Base
Aquí creamos los manifiestos de Kubernetes que definen nuestra aplicación. Estos ficheros son la plantilla fundamental que se usará en todos los entornos.

1. **Creación del Deployment Base**

    Este fichero describe cómo se debe ejecutar nuestra aplicación: la imagen del contenedor, los puertos y las variables de entorno base, incluyendo la referencia a un secreto que será gestionado por External Secrets.

    ```bash
    cat <<EOF > ./base/deployment.yaml
    # base/deployment.yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: mi-app-nodejs
    spec:
      replicas: 1
      selector:
        matchLabels:
          app: mi-app-nodejs
      template:
        metadata:
          labels:
            app: mi-app-nodejs
        spec:
          automountServiceAccountToken: false # Desactiva el token si no se usa
          containers:
          - name: mi-app
            image: node:22-alpine # Una imagen de ejemplo
            ports:
            - containerPort: 3000
            env:
            - name: APP_ENV
              value: "base"
            - name: DB_HOST
              value: "db.example.com"
            - name: DB_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: db-credentials # Nombre del Secret que creará External Secrets
                  key: password

    EOF
    ```

2. **Creación del Service Base**

    Este manifiesto expone nuestro `Deployment` dentro del clúster, permitiendo que otras aplicaciones se comuniquen con él a través de un punto de acceso estable.

    ```bash
    cat <<EOF > ./base/service.yaml
    # base/service.yaml
    apiVersion: v1
    kind: Service
    metadata:
      name: mi-app-nodejs-service
    spec:
      selector:
        app: mi-app-nodejs
      ports:
      - protocol: TCP
        port: 80
        targetPort: 3000

    EOF
    ```

3. **Creación del Fichero `kustomization.yaml` Base**

    Este fichero es el punto de entrada para Kustomize en el directorio `base`. Simplemente lista todos los manifiestos que deben ser incluidos.

    ```bash
    cat <<EOF > ./base/kustomization.yaml
    # base/kustomization.yaml
    apiVersion: kustomize.config.k8s.io/v1beta1
    kind: Kustomization
    resources:
    - deployment.yaml
    - service.yaml

    EOF
    ```

---

### 🛠️ Creación del Overlay para el Entorno `dev`
En esta sección, definimos las personalizaciones específicas para el entorno de desarrollo (`dev`). Estos cambios se aplicarán "encima" de la configuración base.

1. **Creación del Parche para `dev`**

    Este fichero contiene solo los cambios que queremos aplicar. En este caso, aumentamos el número de réplicas y cambiamos el valor de una variable de entorno para que sea específico de `dev`.

    ```bash
    cat <<EOF > ./overlays/dev/patch-deployment.yaml
    # overlays/dev/patch-deployment.yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      # Este nombre debe coincidir con el nombre del Deployment en la base
      name: mi-app-nodejs
    spec:
      # 1. Sobrescribe el número de réplicas
      replicas: 2
      template:
        spec:
          containers:
            # Este nombre debe coincidir con el nombre del contenedor en la base
          - name: mi-app
            # 2. Añade o sobrescribe variables de entorno
            env:
              - name: APP_ENV
                value: "development"

    EOF
    ```

2. **Creación del `kustomization.yaml` para `dev`**

    Este fichero le indica a Kustomize que debe usar el directorio `base` como punto de partida y luego aplicar el parche que acabamos de crear para personalizar la configuración final.

    ```bash
    cat <<EOF > ./overlays/dev/kustomization.yaml
    # overlays/dev/kustomization.yaml
    apiVersion: kustomize.config.k8s.io/v1beta1
    kind: Kustomization

    # 1. Hereda todo de la base
    bases:
    - ../../base

    # 2. Aplica parches específicos para 'dev'
    patches:
    - path: patch-deployment.yaml # Fichero con los cambios
      target:
        kind: Deployment
        name: mi-app-nodejs # Recurso específico a modificar

    EOF
    ```

---

### ✅ Verificación del Resultado
Finalmente, para comprobar que Kustomize está combinando la base y el parche correctamente, puedes ejecutar el siguiente comando desde la raíz del repositorio.

```bash
kustomize build overlays/dev
```

Esto imprimirá en la terminal el manifiesto final para el entorno `dev`. Podrás ver que el campo `replicas` tiene el valor `2` y la variable de entorno `APP_ENV` está establecida en "`development`", confirmando que el overlay se aplicó correctamente.

---

### 👨‍💻 Configuración de la CLI de AWS para LocalStack
Para que el External Secrets Operator pueda comunicarse con nuestro simulador de AWS (LocalStack), necesitamos configurar la CLI de AWS en nuestra máquina. Aunque no nos estamos conectando a una cuenta real de AWS, la CLI requiere una configuración básica para funcionar.

Ejecuta el siguiente comando e introduce los valores que se muestran a continuación. Usamos credenciales genéricas como `test` porque LocalStack está diseñado para aceptarlas en un entorno de desarrollo local sin necesidad de autenticación real.

```bash
aws configure
```

```Plaintext
AWS Access Key ID [None]: test
AWS Secret Access Key [None]: test
Default region name [None]: us-east-1
Default output format [None]: json
```

> Con esta configuración, la CLI de AWS está lista para interactuar con nuestro contenedor de LocalStack.

---

### ☁️ Desplegar LocalStack para Simular AWS
Ahora, levantaremos `LocalStack`, un simulador de servicios de la nube de `AWS`. Esto nos permite desarrollar y probar nuestra integración con `AWS Secrets Manager` sin necesidad de una cuenta real de `AWS`. Definimos su configuración en un fichero `docker-compose.yaml` para que se ejecute como un contenedor `Docker`.

```bash
cat <<EOF > ./docker-compose.yaml
# docker-compose.yaml
services:
  localstack:
    # versión localstack https://hub.docker.com/r/localstack/localstack/tags
    image: localstack/localstack:3.0.2
    ports:
      - "4566:4566" # Puerto principal para servicios AWS
    environment:
      SERVICES: secretsmanager
      DEBUG: 1

EOF
```

Una vez definido, iniciamos `LocalStack` en segundo plano (`-d`).

```bash
docker-compose up -d
```

Verificamos que el contenedor esté corriendo.

```bash
docker ps
```

Deberías ver un contenedor con un nombre similar a [nombre_de_tu_carpeta]-localstack-1 en estado Up.

> ¡Perfecto! Ya tienes tu simulador de AWS Secrets Manager esperando para guardar secretos.

---

### ⚙️ Iniciar un Clúster Local con Minikube de Kubernetes
Para comenzar, necesitamos un entorno de Kubernetes donde desplegar nuestras aplicaciones. Usaremos `Minikube`, una herramienta que nos permite ejecutar un clúster de `Kubernetes` de un solo nodo en nuestra `máquina local`. El siguiente comando inicia el clúster utilizando `Docker` como el motor de contenedores.

```bash
minikube start --driver=docker
kubectl get nodes
```

---

### 🚀 Instalar ArgoCD en el Clúster
Finalmente, instalaremos `ArgoCD`, la herramienta de `GitOps` que se encargará de sincronizar el estado de nuestro repositorio `Git` con el clúster de `Kubernetes`. Primero, creamos un `namespace` dedicado para aislar los componentes de `ArgoCD` y luego aplicamos el manifiesto de instalación oficial.

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl get pods -n argocd -w
```

Con el último comando, observamos en tiempo real (`-w`) el estado de los Pods en el `namespace argocd`.

> Cuando veas que todos los pods están en estado Running, la instalación estará completa. Presiona Ctrl + C para salir del modo de observación.

¡Genial! ArgoCD ya vive dentro del clúster.

---

### 🧩 Instalar External Secrets Operator
Ahora que Argo CD gestiona nuestros manifiestos, necesitamos una forma segura de manejar los secretos (como contraseñas o claves de API). External Secrets Operator es la solución: extiende Kubernetes para que pueda obtener secretos de almacenamientos externos (como AWS Secrets Manager, que simulamos con LocalStack) y sincronizarlos como `Secrets` nativos de Kubernetes.

1. **Añadir el repositorio de Helm**

    Primero, agregamos el repositorio oficial de charts de Helm para External Secrets, lo que nos permitirá instalarlo fácilmente.

    ```bash
    helm repo add external-secrets https://charts.external-secrets.io
    ```

    > Deberías ver el mensaje: `"external-secrets" has been added to your repositories`.

2. **Crea un fichero `values.yaml` para LocalStack**

    Para personalizar la instalación de Helm, creamos un fichero `values.yaml`. Este fichero nos permite sobrescribir la configuración por defecto del chart de External Secrets Operator (ESO). En este caso, lo usamos para inyectar las variables de entorno necesarias para que el operador se comunique directamente con nuestro contenedor de LocalStack en lugar de la nube pública de AWS.

    ```bash
    cat <<EOF > ./eso-values.yaml
    # eso-values.yaml
    image:
      repository: ghcr.io/external-secrets/external-secrets
      tag: latest-aws

    env:
      - name: AWS_ACCESS_KEY_ID
        value: test
      - name: AWS_SECRET_ACCESS_KEY
        value: test
      - name: AWS_REGION
        value: us-east-1
      - name: AWS_ENDPOINT_URL
        value: "http://host.minikube.internal:4566"

    EOF
    ```

3. **Instalar el chart**

    A continuación, instalamos el operador en su propio `namespace` para mantenerlo aislado y organizado.

    ```bash
    helm install external-secrets external-secrets/external-secrets \
      -n external-secrets \
      --create-namespace \
      -f eso-values.yaml
    ```

4. **Verificar que el operador esté funcionando**

    Finalmente, comprobamos que los Pods del operador se hayan iniciado correctamente y estén en estado `Running`.

    ```bash
    kubectl get pods -n external-secrets
    ```

    > Deberías ver uno o más pods de `external-secrets` en estado `Running`.

---

### 🔐 Gestión de Secretos con AWS CLI y LocalStack

En esta sección, realizaremos el ciclo de vida completo de un secreto utilizando la CLI de AWS apuntando a nuestro entorno local de LocalStack. Esto incluye crear, verificar y eliminar un secreto.

1. **Crear un Secreto en Texto Plano**

    Primero, creamos un secreto para la contraseña de la base de datos de nuestra aplicación `mi-app-nodejs` en el entorno de `dev`. Usamos una estructura jerárquica en el nombre (`entorno/aplicacion/recurso`) para mantener nuestros secretos organizados.

    ```bash
    aws secretsmanager create-secret \
      --name dev/mi-app-nodejs/db-credentials \
      --secret-string 'PasswordSuperSeguroParaDev123' \
      --endpoint-url=http://localhost:4566
    ```

    > Deberías recibir una respuesta en formato JSON confirmando la creación del secreto, incluyendo su `ARN` (Amazon Resource Name).

    > ARN: Es un identificador único y estandarizado para los recursos en los servicios de AWS.

2. **Listar los Secretos Almacenados**

    A continuación, verificamos que el secreto se haya creado correctamente. Este comando nos devuelve una lista con los metadatos de todos los secretos almacenados, pero por seguridad, no muestra sus valores.

    ```bash
    aws secretsmanager list-secrets \
      --endpoint-url=http://localhost:4566
    ```

3. **Visualización del Contenido del Secreto**

    A diferencia de `list-secrets`, que solo muestra metadatos, el comando `get-secret-value` se utiliza para obtener el contenido real y sensible de un secreto.

    - `get-secret-value`: Es la acción específica para leer el valor almacenado.
    - `--secret-id`: Identifica el secreto que deseas consultar por su nombre.

    Al ejecutarlo, la respuesta incluirá el campo SecretString, que contiene la información sensible que guardaste.

    ```bash
    aws secretsmanager get-secret-value \
      --secret-id dev/mi-app-nodejs/db-credentials \
      --endpoint-url=http://localhost:4566
    ```

4. **Eliminar el Secreto de Forma Inmediata**

    Finalmente, para limpiar nuestro entorno, eliminamos el secreto. Usamos el flag `--force-delete-without-recovery` para borrarlo permanentemente al instante, una práctica común en entornos de desarrollo donde no se necesita el período de recuperación de seguridad de AWS.

    ```bash
    aws secretsmanager delete-secret \
      --secret-id dev/mi-app-nodejs/db-credentials \
      --force-delete-without-recovery \
      --endpoint-url=http://localhost:4566
    ```

---

### 🔌 Conectar External Secrets con LocalStack
Ahora que tenemos External Secrets Operator y LocalStack funcionando, el siguiente paso es conectarlos. Crearemos un `SecretStore`, que le dice al operador cómo autenticarse y comunicarse con nuestro proveedor de secretos (en este caso, LocalStack).

1. **Definir el `SecretStore`**

    El `SecretStore` es un recurso de Kubernetes que define la conexión a un sistema de gestión de secretos externo. Aquí le indicamos que queremos usar AWS Secrets Manager, apuntamos a la URL de nuestro LocalStack y le decimos cómo encontrar las credenciales de autenticación dentro del clúster.

    ```bash
    cat <<EOF > ./secret-store.yaml
    # secret-store.yaml
    apiVersion: external-secrets.io/v1
    kind: SecretStore
    metadata:
      name: aws-secret-store
    spec:
      provider:
        aws:
          service: SecretsManager
          region: us-east-1
          # La URL de nuestro LocalStack
          serviceConfig:
            url: http://localhost:4566
          auth:
            # En un caso real, aquí irían credenciales seguras.
            # Para LocalStack, usamos credenciales de prueba.
            secretRef:
              accessKeyIDSecretRef:
                name: awssm-secret
                key: accessKeyID
              secretAccessKeySecretRef:
                name: awssm-secret
                key: secretAccessKey

    EOF
    ```

2. **Crear el Secreto de Autenticación en Kubernetes**

    El `SecretStore` anterior hace referencia a un secreto de Kubernetes llamado `awssm-secret` para obtener las credenciales. Ahora creamos ese secreto con los valores `test`, que son los que LocalStack espera.

    ```bash
    kubectl create secret generic awssm-secret \
      --from-literal=accessKeyID='test' \
      --from-literal=secretAccessKey='test'
    ```

3. **Definir el Recurso `ExternalSecret` para `dev`**

    Con la conexión ya establecida, creamos un `ExternalSecret`. Este recurso le dice a External Secrets Operator: "Busca el secreto llamado `dev/mi-app-nodejs/db-credentials` en AWS Secrets Manager y crea un `Secret` nativo de Kubernetes llamado `db-credentials` con su valor".

    Este fichero se crea dentro del overlay `dev` porque es específico de ese entorno.

    ```bash
    cat <<EOF > ./overlays/dev/external-secret.yaml
    # overlays/dev/external-secret.yaml
    apiVersion: external-secrets.io/v1
    kind: ExternalSecret
    metadata:
      # Este será el nombre del Secret de Kubernetes generado
      name: db-credentials
    spec:
      # Referencia al store que creamos antes
      secretStoreRef:
        name: aws-secret-store
        kind: SecretStore
      target:
        # Define cómo se debe llamar el Secret de k8s
        name: db-credentials
      data:
      - secretKey: password # La "key" que espera nuestro deployment.yaml
        remoteRef:
          key: dev/mi-app-nodejs/db-credentials # El nombre del secreto en AWS Secrets Manager

    EOF
    ```

4. **Actualizar Kustomize para Incluir el ExternalSecret**

    Finalmente, debemos decirle a Kustomize que gestione el recurso `ExternalSecret` que acabamos de crear. Para ello, lo añadimos a la lista de `resources` en el `kustomization.yaml` del entorno `dev`.

    El siguiente bloque debe ser añadido al fichero `kustomization.yaml` existente en `overlays/dev/`.

    ```Plaintext
    # Añadimos el ExternalSecret a los recursos de este entorno.
    # Ten cuidado de no sobreescribir el fichero, sino de añadir el recurso.
    resources:
    - external-secret.yaml
    ```

    La versión final del fichero `kustomization.yaml` para `dev` debería lucir así, incluyendo la base, el parche y el nuevo recurso.

    ```bash
    cat <<EOF > ./overlays/dev/kustomization.yaml
    # overlays/dev/kustomization.yaml
    apiVersion: kustomize.config.k8s.io/v1beta1
    kind: Kustomization

    # 1. Hereda todo de la base
    bases:
    - ../../base

    # 3. Se añade el ExternalSecret a los recursos de este entorno
    resources:
    - external-secret.yaml

    # 2. Aplica parches específicos para 'dev'
    patches:
    - path: patch-deployment.yaml # Fichero con los cambios
      target:
        kind: Deployment
        name: mi-app-nodejs # Recurso específico a modificar

    EOF
    ```

---

### 🚢 Creación de la Aplicación en ArgoCD
Este es el paso final donde conectamos todo. Crearemos un recurso de tipo `Application` en ArgoCD. Este manifiesto le dice a ArgoCD qué repositorio de Git debe monitorear, qué carpeta dentro de ese repositorio contiene los manifiestos (`nuestro overlays/dev`), en qué clúster y `namespace` debe desplegarlo, y qué rama usar.

1. **Definir el Manifiesto de la Aplicación**

    Creamos un fichero YAML que define la aplicación para nuestro entorno de `dev`. Este es el corazón de GitOps: a partir de ahora, cualquier cambio que hagamos en la rama `develop` en la carpeta `overlays/dev` será detectado y aplicado automáticamente por ArgoCD.

    ```bash
    cat <<EOF > ./argocd/applications/app-dev.yaml
    # argocd/applications/app-dev.yaml
    apiVersion: argoproj.io/v1alpha1
    kind: Application
    metadata:
      name: mi-app-nodejs-dev
      namespace: argocd
    spec:
      project: default
      source:
        repoURL: 'https://github.com/rafariass/learning-gitops'
        path: overlays/dev
        targetRevision: develop
      destination:
        server: 'https://kubernetes.default.svc'
        namespace: dev # Desplegaremos la app en un nuevo namespace llamado 'dev'
      syncPolicy:
        automated:
          prune: true
          selfHeal: true
        # Crea el namespace 'dev' si no existe
        syncOptions:
        - CreateNamespace=true

    EOF
    ```

    > Nota: Asegúrate de cambiar la `repoURL` por la URL de tu propio repositorio de GitHub.

2. **Subir los Cambios al Repositorio Git**

    Ahora que toda nuestra configuración está definida en código, la subimos al repositorio. Creamos una nueva rama `develop` para el entorno de desarrollo y enviamos todos nuestros ficheros. Una vez que estos cambios estén en el repositorio, ArgoCD podrá encontrarlos y empezar el proceso de sincronización.

    ```bash
    git switch -c develop
    git add .
    git commit -m "feat: configure external secrets and argocd app for dev"
    git push origin develop
    ```

---

### ✅ Despliegue de la Configuración en el Clúster
Con todos los ficheros de configuración listos en nuestro repositorio local, el último paso es aplicarlos a nuestro clúster de Kubernetes.

1. **Aplicar el `SecretStore`**

    Primero, creamos el `SecretStore`. Este es un paso que generalmente se hace una sola vez por cada proveedor de secretos. Al aplicar este manifiesto, le damos al **External Secrets Operator** las instrucciones y credenciales para conectarse a nuestro LocalStack.

    ```bash
    # Aplica el SecretStore (una sola vez)
    kubectl apply -f secret-store.yaml
    ```

2. **Aplicar la Definición de la Aplicación en ArgoCD**

    Finalmente, aplicamos el manifiesto de la aplicación de ArgoCD. Este comando le dice a ArgoCD: "Empieza a monitorear la aplicación `mi-app-nodejs-dev`". ArgoCD leerá la configuración de este manifiesto, se conectará a tu repositorio de Git y desplegará todo lo que encuentre en la ruta `overlays/dev`. A partir de este momento, ArgoCD tomará el control y mantendrá tu clúster sincronizado con el repositorio.

    ```bash
    # Aplica la definición de la aplicación en ArgoCD
    kubectl apply -f argocd/applications/app-dev.yaml
    ```

---
