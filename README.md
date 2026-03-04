# 🛍️ ArgoCD Online Shop Demo Repository

A comprehensive demonstration repository showcasing **multiple ArgoCD deployment patterns** for the Online Shop application. This repository serves as both a learning resource and a practical guide for implementing Application Lifecycle Management (ALM) with ArgoCD across various deployment strategies.

## 🎯 Overview

This repository demonstrates how to deploy and manage the **Online Shop microservice** application using ArgoCD with different architectural patterns. It includes real-world examples of:
- Declarative GitOps workflows
- App-of-Apps pattern
- CLI-based deployments
- Git Generator pattern
- Multi-cluster deployments
- Monitoring integration
- UI-based management approaches

## ✨ Key Features

### 📦 Multiple Deployment Approaches
- **Declarative Approach**: Direct YAML manifests
- **App-of-Apps Pattern**: Hierarchical application management
- **CLI Approach**: Command-line driven deployments
- **Git Generator**: Dynamic application generation
- **UI Approach**: Web-based GitOps interface
- **Multi-Cluster**: Cross-cluster deployment management

### 🚀 ArgoCD GitOps Features
- **Declarative Configuration**: All infrastructure defined in Git
- **Automated Sync**: Automatic deployment on Git changes
- **Auto-Healing**: Self-healing capabilities
- **Pruning**: Automatic resource cleanup
- **Monitoring Integration**: Observability stack deployment
- **Resource Management**: CPU/Memory requests and limits

### 🔄 Application Features
- **Online Shop**: E-commerce microservice platform
- **Load Balancing**: Nginx and Apache configurations
- **Scalability**: Multi-replica deployments
- **Resource Optimization**: Container resource limits and requests
- **Service Discovery**: Kubernetes service networking

## 📁 Repository Structure

```
Online-shop/
├── declarative_approach/          # Direct GitOps manifests
│   ├── online_shop/
│   │   ├── online_shop_app.yml       # Application CRD
│   │   ├── online_shop_deployment.yml # Kubernetes deployment
│   │   └── online_shop_svc.yml       # Service configuration
│   └── apache/                       # Apache deployment
│
├── app_of_apps/                   # Hierarchical app pattern
│   ├── apps/
│   │   ├── online_shop_app.yml
│   │   ├── apache_app.yml
│   │   └── nginx_app.yml
│   └── [parent orchestration]
│
├── cli_approach/                  # CLI-based deployments
│   └── apache/
│       └── [CLI configuration files]
│
├── git_generator/                 # Git generator pattern
│   ├── apache/
│   ├── chai-app/
│   └── online-shop/
│       ├── deployment.yml
│       └── service.yml
│
├── monitoring/                    # Observability stack
│   ├── chai-app/
│   ├── online_shop/
│   │   ├── online_shop_deployment.yml
│   │   └── online_shop_svc.yml
│   └── [monitoring configurations]
│
├── multicluster/                  # Multi-cluster deployments
│   ├── online-shop/
│   │   ├── deployment.yml         # Multi-cluster deployment
│   │   └── service.yml
│   └── [cluster-specific configs]
│
├── ui_approach/                   # UI-based approach
│   └── nginx/
│
├── applicationsets/               # ApplicationSet resources
│   └── [ApplicationSet definitions]
│
└── README.md                      # This file
```

## 🛠️ Core Components

### Application Specifications

#### Online Shop Application
- Name: online-shop
- Image: amitabhdevops/online_shop:latest
- Port: 3000
- Replicas: 3-6 (varies by approach)
- Resources:
    - CPU Request: 100m
    - CPU Limit: 500m
    - Memory Request: 128Mi
    - Memory Limit: 256Mi

#### Service Configuration
- Type: ClusterIP (default)
- Port: 3000
- Target Port: 3000

### Deployment Patterns

#### 1. Declarative Approach
**Location**: `declarative_approach/online_shop/`

Direct Git-based resource management:
- Replicas: 6
- Sync Policy: Automated with pruning and self-healing
- Best For: Simple, straightforward deployments

#### 2. App-of-Apps Pattern
**Location**: `app_of_apps/apps/`

Hierarchical management of multiple applications:
- Parent application orchestrates child apps
- Supports Apache, Nginx, and Online Shop
- Best For: Complex multi-application deployments

#### 3. CLI Approach
**Location**: `cli_approach/`

Command-line driven deployment management:
- Apache deployment configuration
- Manual sync control
- Best For: CI/CD pipeline integration

#### 4. Git Generator Pattern
**Location**: `git_generator/`

Dynamic application generation from Git:
- Apache deployment generator
- Chai application templates
- Online Shop multi-variant deployments
- Replicas: 3

#### 5. Multi-Cluster Deployment
**Location**: `multicluster/`

Cross-cluster deployment management:
- Online Shop deployment
- Replicas: 4
- Service networking across clusters
- Best For: High-availability and disaster recovery

#### 6. Monitoring Integration
**Location**: `monitoring/`

Observability stack deployment:
- Chai application monitoring
- Online Shop monitoring setup
- Metrics collection

## 🚀 Getting Started

### Prerequisites

1. **Kubernetes Cluster**
   - kubectl configured and ready
   - Access to cluster admin operations

2. **ArgoCD Installation**
   ```bash
   kubectl create namespace argocd
   kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
   ```

3. **ArgoCD CLI**
   ```bash
   brew install argocd  # macOS
   # or download from GitHub releases
   ```

### Initial Setup - CRITICAL!

1. **Get Your Cluster URL**
   ```bash
   argocd cluster list
   ```

2. **Update ALL Manifests with Your Cluster URL**
   
   Replace `https://172.31.19.178:33893` in:
   - declarative_approach/online_shop/online_shop_app.yml
   - app_of_apps/apps/apache_app.yml
   - app_of_apps/apps/nginx_app.yml
   - app_of_apps/apps/online_shop_app.yml
   - And other relevant files

### Deployment Steps

#### Option 1: Declarative Approach (Simplest)

```bash
# Update cluster URL first in the yaml files
kubectl apply -f declarative_approach/online_shop/online_shop_app.yml
```

#### Option 2: App-of-Apps Pattern

```bash
kubectl apply -f app_of_apps/apps/
```

#### Option 3: CLI Approach

```bash
argocd app create online-shop \
  --repo https://github.com/rakeshkolipakaace/Online-shop.git \
  --path declarative_approach/online_shop \
  --dest-server https://kubernetes.default.svc \
  --dest-namespace default
```

#### Option 4: Multi-Cluster

```bash
kubectl apply -f multicluster/online-shop/
```

## 📊 Application Configurations

### Deployment Replicas by Approach

| Approach | Replicas | Purpose |
|----------|----------|---------|
| Declarative | 6 | High availability |
| App-of-Apps | 6 | Hierarchical management |
| CLI | Varies | Manual control |
| Git Generator | 3 | Template-based |
| Multi-Cluster | 4 | HA across clusters |

## 🎓 GitOps Concepts Explained

### Declarative Configuration
- Everything in Git = source of truth
- No manual kubectl apply on prod
- Version controlled infrastructure

### Sync Policies
```yaml
syncPolicy:
  automated:
    prune: true      # Delete removed resources
    selfHeal: true   # Auto-sync on drift
```

### Resource Management
```yaml
resources:
  requests:
    cpu: "100m"
    memory: "128Mi"
  limits:
    cpu: "500m"
    memory: "256Mi"
```

## 🔧 Common Operations

### View Status
```bash
argocd app list
argocd app get online-shop-app
```

### Manual Sync
```bash
argocd app sync online-shop-app
```

### Hard Refresh
```bash
argocd app sync online-shop-app --hard-refresh
```

### Delete
```bash
argocd app delete online-shop-app
```

## 🐛 Troubleshooting

### Cluster URL Mismatch
- Error: Application won't sync
- Solution: Update server URL in Application CRD with output from `argocd cluster list`

### Port 3000 Already in Use
- Error: Service binding fails
- Solution: Change port in service.yml or use different service type

### Resource Quota Issues
- Error: Pod pending
- Solution: Reduce replicas or increase cluster resources

## ✅ Best Practices

1. Always update cluster URL before deploying
2. Use namespaces for isolation
3. Set resource limits and requests
4. Enable auto-sync for production
5. Monitor application health
6. Use sealed secrets for sensitive data
7. Version all infrastructure in Git
8. Test in staging first

## 📚 Learning Resources

- [ArgoCD Official Documentation](https://argo-cd.readthedocs.io/)
- [Kubernetes Manifests](https://kubernetes.io/docs/concepts/configuration/overview/)
- [GitOps Best Practices](https://www.gitops.tech/)

## 🤝 Contributing

Improvements and examples are welcome. Please submit pull requests!

## 📄 License

Open source for educational and commercial use.

## 👨‍💻 Author

**Rakesh** - [GitHub](https://github.com/rakeshkolipakaace)

---

## ⚠️ IMPORTANT - Read Before Deploying

**You MUST update the cluster server URL in all Application CRDs before deploying!**

Default URL `https://172.31.19.178:33893` is only for the demo environment.

Get your cluster URL:
```bash
argocd cluster list
```

Update these files with your URL:
- declarative_approach/online_shop/online_shop_app.yml
- app_of_apps/apps/*.yml
- All other Application manifests

**Last Updated**: March 2026
**Repository**: [Online-shop](https://github.com/rakeshkolipakaace/Online-shop)
**Status**: Active Learning Resource
