# Cosmocloud Deploy Helm Chart

This Helm chart deploys a sample application consisting of:
- **Backend** service: `shreybatra/sample-backend`
- **Frontend** service: `shreybatra/sample-frontend`
- **Redis** database: `redis`

## Configuration

Environment variables are managed as follows:
- `BACKEND_URL` is stored in a **ConfigMap**.
- `REDIS_URI` is stored in a **Secret**.


- **Mix of ConfigMap and Secrets**: Non-sensitive `BACKEND_URL` is in a ConfigMap, while sensitive `REDIS_URI` is in a Secret.
- **Service Types**: Backend and Redis use `ClusterIP`, while the frontend uses `NodePort`.
- **Scalable and Modular**: You can update the ConfigMap or Secret without altering Deployments.