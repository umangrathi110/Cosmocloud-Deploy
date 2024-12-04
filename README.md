# Cosmocloud Deploy Helm Chart

This Helm chart is designed to deploy a sample application consisting of three components:

- **Backend** service 
- **Frontend** service
- **Redis** database

The chart deploys the backend, frontend, and Redis as Kubernetes deployments with associated services. The configuration and environment variables are managed through Kubernetes `ConfigMap` and `Secret` resources for better security and maintainability.

## Key Files

- **Chart.yaml**: Contains metadata about the chart, including its name, version, and description.
- **values.yaml**: Default configuration for the chart, such as Docker images, service types, ports, and environment variables.
- **templates/**: Contains Kubernetes manifest templates for the deployment and services.

## Configuration

The chart uses the `values.yaml` file to set configuration values. These values can be customized when deploying the Helm chart to modify the behavior of the services.

### Docker Images and Replicas

The chart uses specific Docker images for the backend, frontend, and Redis services. The backend and frontend are built from custom images (`shreybatra/sample-backend` and `shreybatra/sample-frontend`), while Redis uses the official Redis image. The chart deploys each service with a single replica (`replicaCount: 1`). This can be adjusted in the `values.yaml`. 

### Services

Each component (backend, frontend, Redis) is exposed via a Kubernetes `Service`:

- **Backend Service**: Exposed as a `ClusterIP` service on port `8000`, allowing communication within the cluster.
- **Frontend Service**: Exposed as a `NodePort` service on port `5173` with a node port of `31000`, making it accessible externally.
- **Redis Service**: Exposed as a `ClusterIP` service on port `6379` for internal communication.


## Kubernetes Deployments

Each service is deployed as a Kubernetes `Deployment`:

1. **Backend Deployment**: Deployed using the `shreybatra/sample-backend` image, listening on port `8000` and connecting to Redis via the `REDIS_URI` from the `redis-secret`.
2. **Frontend Deployment**: Deployed using the `shreybatra/sample-frontend` image, listening on port `5173` and connecting to the backend via the `BACKEND_URL` from the `app-config`.
3. **Redis Deployment**: Deployed using the official `redis` image, listening on port `6379`.

## Secret and ConfigMap Management

Environment variables are managed via `ConfigMap` and `Secret` resources:

- **Secrets**: The `redis-secret` contains the `REDIS_URI` environment variable for the backend to connect to Redis.
- **ConfigMaps**: The `app-config` contains the `BACKEND_URL` environment variable for the frontend to connect to the backend.

This ensures that sensitive data like URIs and connection strings are securely handled.