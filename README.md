# Seminario DevOps - Miniapp PHP

Este proyecto consiste en una miniaplicación PHP diseñada para ser contenerizada con Docker y desplegada en Minikube utilizando Kubernetes.

---

## 📁 Estructura del proyecto

```
seminario-devops/
├── Dockerfile
├── deploy-seminario-devops.yaml
└── src/
    └── index.php
```

---

## 📜 Contenido de los archivos

### `src/index.php`

```php
<?php
echo "Seminario Devop! Bienvenido a mi repo ".gethostname()."\n";
?>
```

### `Dockerfile`

```Dockerfile
FROM php:fpm-alpine
RUN mkdir -p /var/www/html
COPY --chown=nobody src/ /var/www/html/
WORKDIR /var/www/html
CMD ["php", "-S", "0.0.0.0:80", "-t", "/var/www/html/"]
```

### `deploy-seminario-devops.yaml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: seminario-devops
  name: seminario-devops
spec:
  replicas: 1
  selector:
    matchLabels:
      app: seminario-devops
  template:
    metadata:
      labels:
        app: seminario-devops
    spec:
      containers:
      - image: seminario-devops:latest
        name: seminario-devops
        imagePullPolicy: Never
        resources: {}
```

---

## 🚀 Pasos realizados

1. Se creó el archivo PHP dentro de `src/index.php`.
2. Se construyó la imagen con:

   ```bash
   docker build -t seminario-devops .
   ```

3. Se cargó la imagen en Minikube:

   ```bash
   minikube image load seminario-devops
   ```

4. Se creó el deployment con:

   ```bash
   kubectl apply -f deploy-seminario-devops.yaml
   ```

5. Se expuso el servicio con:

   ```bash
   kubectl expose deployment/seminario-devops --type=NodePort --port 80
   ```

6. Se accedió a la aplicación vía navegador usando la IP de Minikube y el puerto asignado:

   ```bash
   minikube service seminario-devops --url
   ```

---

## ✅ Resultado

Al acceder al servicio, se muestra un mensaje como:

```
Seminario Devop! Bienvenido a mi repo minikube
```

donde `minikube` es el hostname del contenedor.

---

## 🔗 Autor

M. Eugenia Descalzo – Junio 2025
