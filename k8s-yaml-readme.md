# Kubernetes YAML Manifests for Ostad Project

This directory contains all the Kubernetes YAML files required to deploy the Ostad project, including backend, frontend, MongoDB, and supporting services. Below is a description of each file and its purpose.

---

## File Overview

### 1. [`namespace.yaml`](namespace.yaml)
Defines the `mehedi` namespace to logically separate all Ostad resources in the cluster.

---

### 2. [`mongo-secret.yaml`](mongo-secret.yaml)
Creates a Kubernetes Secret containing the MongoDB root username and password (base64 encoded).

---

### 3. [`mongo-configmap.yaml`](mongo-configmap.yaml)
Provides a ConfigMap with the MongoDB connection URI for use by backend services.

---

### 4. [`mongo-deployment.yaml`](mongo-deployment.yaml)
Deploys a MongoDB instance with resource requests/limits and environment variables sourced from the secret.

---

### 5. [`mongo-service.yaml`](mongo-service.yaml)
Exposes the MongoDB deployment internally within the cluster on port 27017.

---

### 6. [`mongo-express-deployment.yaml`](mongo-express-deployment.yaml)
Deploys the Mongo Express UI for MongoDB administration, using credentials from the secret.

---

### 7. [`mongo-express-service.yaml`](mongo-express-service.yaml)
Exposes Mongo Express internally within the cluster on port 8081.

---

### 8. [`ostad-backend-deployment.yaml`](ostad-backend-deployment.yaml)
Deploys the Ostad backend server (Express.js), pulling the image from Docker Hub and injecting the MongoDB URI from the ConfigMap.

---

### 9. [`ostad-backend-service.yaml`](ostad-backend-service.yaml)
Exposes the backend server as a LoadBalancer service on port 5050.

---

### 10. [`ostad-ui-deployment.yaml`](ostad-ui-deployment.yaml)
Deploys the Ostad frontend (React app), pulling the image from Docker Hub.

---

### 11. [`ostad-ui-service.yaml`](ostad-ui-service.yaml)
Exposes the frontend as a LoadBalancer service on port 5173.

---

### 12. [`ingress.yaml`](ingress.yaml)
Defines an Ingress resource for routing external HTTP(S) traffic to the backend API and Mongo Express UI, using the hostname `ostad.local`.

---

### 13. [`kind-cluster.yaml`](kind-cluster.yaml)
Configuration for creating a local Kubernetes cluster using [Kind](https://kind.sigs.k8s.io/), with port mappings for HTTP and HTTPS.

---

## Usage

1. **Create Namespace**  
   `kubectl apply -f namespace.yaml`

2. **Apply Secrets and ConfigMaps**  
   `kubectl apply -f mongo-secret.yaml`  
   `kubectl apply -f mongo-configmap.yaml`

3. **Deploy MongoDB and Mongo Express**  
   `kubectl apply -f mongo-deployment.yaml`  
   `kubectl apply -f mongo-service.yaml`  
   `kubectl apply -f mongo-express-deployment.yaml`  
   `kubectl apply -f mongo-express-service.yaml`

4. **Deploy Backend and Frontend**  
   `kubectl apply -f ostad-backend-deployment.yaml`  
   `kubectl apply -f ostad-backend-service.yaml`  
   `kubectl apply -f ostad-ui-deployment.yaml`  
   `kubectl apply -f ostad-ui-service.yaml`

5. **Deploy the app**
    `kubectl get svc -n mehedi`

6. **(Optional)Set Up Ingress**  
   `kubectl apply -f ingress.yaml`

7. **(Optional) Create Kind Cluster**  
   `kind create cluster --config kind-cluster.yaml`

---

## Notes

- All resources are deployed in the `mehedi` namespace.
- Update Docker image references as needed for your own registry.
- Secrets are base64 encoded; do not commit sensitive values to public repositories.

---