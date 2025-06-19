## Requisitos Previos

- Docker Desktop
- Minikube
- kubectl
- Git Bash o PowerShell


## Estructura del Proyecto

devops-proyecto/
├── web-content/ #Archivos estáticos (index.html, server.js, etc)
├── k8s-manifests/
│ ├── deployment.yaml # Despliegue de Kubernetes (Nginx)
│ └── service.yaml # Servicio para exponer el sitio

## Pasos para Desplegar

# Iniciar Minikube

minikube start --driver=docker

(Asegurate de tener Docker Desktop ejecutándose)

# Crear el directorio en el contenedor de Minikube

minikube ssh

sudo mkdir -p /mnt/data/static-site

exit

# Empaquetar y copiar los archivos del sitio

tar -cvf web-content.tar web-content/

minikube cp web-content.tar minikube:/home/docker/web-content.tar

# Extraer archivos en Minikube

minikube ssh

cd /mnt/data/static-site

sudo tar -xvf /home/docker/web-content.tar -C .

sudo mv web-content/* .

sudo rm -rf web-content

exit

# Aplicar los manifiestos de Kubernetes

cd k8s-manifests

kubectl apply -f deployment.yaml

kubectl apply -f service.yaml

# Acceder al sitio

minikube service static-site-service
