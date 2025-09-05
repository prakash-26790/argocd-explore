# Argo CD Apps of Apps Pattern

This repository demonstrates the Argo CD "apps of apps" pattern with four sample applications:

1. Nginx - Web server
2. Redis - In-memory data store
3. MongoDB - NoSQL database
4. PostgreSQL - SQL database

## Directory Structure

```
.
├── apps/                  # Parent application directory
│   ├── kustomization.yaml # Kustomize configuration for parent app
│   ├── nginx.yaml         # Application CR for nginx
│   ├── redis.yaml         # Application CR for redis
│   ├── mongodb.yaml       # Application CR for mongodb
│   └── postgres.yaml      # Application CR for postgres
├── nginx/                 # nginx application manifests
│   ├── kustomization.yaml # Kustomize configuration for nginx
│   ├── deployment.yaml    # Kubernetes deployment for nginx
│   └── service.yaml       # Kubernetes service for nginx
├── redis/                 # redis application manifests
│   ├── kustomization.yaml # Kustomize configuration for redis
│   ├── deployment.yaml    # Kubernetes deployment for redis
│   └── service.yaml       # Kubernetes service for redis
├── mongodb/               # mongodb application manifests
│   ├── kustomization.yaml # Kustomize configuration for mongodb
│   ├── deployment.yaml    # Kubernetes deployment for mongodb
│   └── service.yaml       # Kubernetes service for mongodb
└── postgres/              # postgres application manifests
    ├── kustomization.yaml # Kustomize configuration for postgres
    ├── deployment.yaml    # Kubernetes deployment for postgres
    └── service.yaml       # Kubernetes service for postgres
```

## How to Use

### Prerequisites

- Kubernetes cluster
- Argo CD installed on the cluster

### Deployment Steps

1. Fork this repository and update the `repoURL` in each Application CR in the `apps/` directory to point to your forked repository.

2. Create the parent application in Argo CD:

```bash
# Create the parent application
argocd app create apps \
  --repo https://github.com/prakash-26790/argocd-explore.git \
  --path apps \
  --dest-server https://kubernetes.default.svc \
  --dest-namespace argocd \
  --sync-policy automated \
  --auto-prune \
  --self-heal
```

3. Sync the parent application:

```bash
argocd app sync apps
```

This will automatically create and sync all the child applications (nginx, redis, mongodb, postgres).

### Accessing the Applications

- Nginx: Access via the service at `http://nginx.default.svc.cluster.local`
- Redis: Connect to the service at `redis.default.svc.cluster.local:6379`
- MongoDB: Connect to the service at `mongodb.default.svc.cluster.local:27017`
- PostgreSQL: Connect to the service at `postgres.default.svc.cluster.local:5432`

## Customization

You can customize each application by modifying the Kubernetes manifests in their respective directories. Kustomize allows you to easily customize applications without modifying the original manifests.

### Using Kustomize Directly

You can also use Kustomize directly to generate the manifests:

```bash
# Generate manifests for an application
kubectl kustomize nginx/

# Apply manifests for an application
kubectl apply -k nginx/