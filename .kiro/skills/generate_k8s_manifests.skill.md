---
name: generate_k8s_manifests
description: Generate Kubernetes Deployment, Service, Ingress, and ConfigMap manifests. Use after analyze_project.
---

# Skill: generate_k8s_manifests

## Goal
Generate production-ready Kubernetes manifests in a `k8s/` directory.

## Instructions
- Infer `app name` from package.json `name`, go.mod module name, Cargo.toml `[package].name`, etc.
- Use the detected port from analyze_project

### Files to generate

**`k8s/deployment.yaml`**
- `replicas: 2`
- Image: `<app-name>:latest` (placeholder)
- Resource requests: `cpu: 100m, memory: 128Mi`
- Resource limits: `cpu: 500m, memory: 512Mi`
- Liveness probe: HTTP GET `/health` on detected port, initialDelaySeconds: 15
- Readiness probe: HTTP GET `/health` on detected port, initialDelaySeconds: 5
- Load env vars from ConfigMap ref

**`k8s/service.yaml`**
- Type: `ClusterIP`
- Port 80 → targetPort = detected port

**`k8s/ingress.yaml`**
- `ingressClassName: nginx`
- Host: `<app-name>.example.com` (placeholder)
- Path: `/` → service port 80

**`k8s/configmap.yaml`**
- Include env vars found in `.env.example` or `.env.sample` (keys only, no secret values)
- Add `NODE_ENV=production` / `PYTHON_ENV=production` / etc. based on language

## Output
k8s/ directory with deployment.yaml, service.yaml, ingress.yaml, configmap.yaml
