# Kubernetes Nginx Deployment Project

This project contains a collection of Kubernetes YAML manifests for deploying an Nginx web server with various resource types, including persistent storage and high availability configurations.

## Overview

The project demonstrates different Kubernetes workload resources and storage management. It includes:

- **Pod**: A basic single container pod
- **Deployment**: Manages replica sets with rolling updates
- **ReplicaSet**: Ensures a specified number of pod replicas
- **StatefulSet**: Manages stateful applications with persistent storage
- **Service**: Exposes the application internally and externally
- **PersistentVolume (PV)**: Provides persistent storage
- **PersistentVolumeClaim (PVC)**: Requests storage from PVs

## Files Description

### my-pod.yml
A simple Pod running Nginx with resource limits (128Mi memory, 500m CPU).

### my-deployment.yml
A Deployment that creates 3 replicas of Nginx pods, using a PersistentVolumeClaim for data persistence.

### my-replicaset.yml
A ReplicaSet maintaining 3 Nginx pod replicas.

### my-stateful-set.yml
A StatefulSet with 3 replicas, each with its own persistent volume created via volumeClaimTemplates. Includes a headless service for stable network identities.

### my-service.yml
A NodePort Service exposing the application on port 4000, mapped to node port 30500.

### my-pv.yml
A PersistentVolume providing 1Gi of hostPath storage.

### my-pvc.yml
A PersistentVolumeClaim requesting 1Gi storage from the PV.

## Prerequisites

- A running Kubernetes cluster (e.g., Minikube, Docker Desktop, or cloud provider)
- `kubectl` configured to access your cluster

## Deployment Instructions

1. **Apply the PersistentVolume and PersistentVolumeClaim:**
   ```bash
   kubectl apply -f my-pv.yml
   kubectl apply -f my-pvc.yml
   ```

2. **Deploy the basic pod (optional):**
   ```bash
   kubectl apply -f my-pod.yml
   ```

3. **Deploy the ReplicaSet:**
   ```bash
   kubectl apply -f my-replicaset.yml
   ```

4. **Deploy the Deployment:**
   ```bash
   kubectl apply -f my-deployment.yml
   ```

5. **Deploy the Service:**
   ```bash
   kubectl apply -f my-service.yml
   ```

6. **Deploy the StatefulSet:**
   ```bash
   kubectl apply -f my-stateful-set.yml
   ```

## Accessing the Application

- **Via Service:** Access the application using the NodePort service on `http://<node-ip>:30500`
- **Check pod status:** `kubectl get pods`
- **Check services:** `kubectl get services`
- **Check persistent volumes:** `kubectl get pv,pvc`

## Cleanup

To remove all resources:
```bash
kubectl delete -f my-stateful-set.yml
kubectl delete -f my-service.yml
kubectl delete -f my-deployment.yml
kubectl delete -f my-replicaset.yml
kubectl delete -f my-pod.yml
kubectl delete -f my-pvc.yml
kubectl delete -f my-pv.yml
```

## Notes

- The StatefulSet uses volumeClaimTemplates to automatically create PVCs for each replica
- The headless service in the StatefulSet provides stable DNS names for pods
- Resource limits are set to prevent overconsumption
- Storage uses hostPath for demonstration; in production, use cloud storage or NFS

## Troubleshooting

- If pods fail to start, check resource availability and storage class compatibility
- Ensure your cluster has sufficient resources for the requested CPU and memory limits
- For hostPath PV, ensure the host directory `/tmp/demo-pv` exists and is writable
