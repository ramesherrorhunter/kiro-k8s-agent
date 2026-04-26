# k8s-agent

An intelligent Kiro CLI agent that auto-generates production-ready Kubernetes manifests by analyzing your codebase — no manual configuration needed.

---

## Purpose

Eliminate the manual effort of writing Kubernetes manifests. The agent inspects your project, detects its language, port, and environment variables, and produces a complete `k8s/` directory ready to deploy.

---

## Solutions Offered

| Problem | Solution |
|---|---|
| Writing K8s manifests from scratch | Auto-generates based on project files |
| Missing resource limits | Always sets CPU and memory requests/limits |
| No health checks | Always adds liveness and readiness probes |
| Hardcoded env vars | Uses ConfigMap, references Secrets for sensitive values |
| Missing Ingress config | Auto-generates nginx Ingress with host placeholder |

---

## Benefits

- **Zero config** — infers everything from your project files
- **Production-ready** — resource limits, health probes, and replicas set by default
- **Secure** — env vars in ConfigMap, sensitive values referenced as Secrets
- **Multi-language support** — Node.js, Python, Go, Java, Rust, Ruby, and generic projects
- **Consistent output** — follows the same best-practice sequence every time

---

## How It Works

The agent runs 2 skills in sequence:

```
analyze_project → generate_k8s_manifests
```

| Skill | What it does |
|---|---|
| `analyze_project` | Detects language, framework, port, and env vars in a single pass |
| `generate_k8s_manifests` | Writes Deployment, Service, Ingress, and ConfigMap |

---

## Installation

**1. Clone the repo**
```bash
git clone https://github.com/ramesherrorhunter/kiro-k8s-agent.git
```

**2. Copy the agent into your project**
```bash
cp -r kiro-k8s-agent/.kiro /path/to/your/project/
```

Or copy into the current directory:
```bash
cp -r kiro-k8s-agent/.kiro .
```

That's it — the agent and all skills are now available in your project.

---

## SOP — Standard Operating Procedure

### Prerequisites
- Kiro CLI installed
- Project directory accessible

### Steps

**1. Navigate to your project**
```bash
cd /path/to/your/project
```

**2. Start Kiro CLI**
```bash
kiro-cli
```

**3. Select the agent**

Type `/agent` and from the dropdown select `k8s-agent`

**4. Review generated files**
```
k8s/
├── deployment.yaml
├── service.yaml
├── ingress.yaml
└── configmap.yaml
```

### Expected Outputs

| File | Description |
|---|---|
| `deployment.yaml` | 2 replicas, resource limits, liveness + readiness probes |
| `service.yaml` | ClusterIP, port 80 → detected app port |
| `ingress.yaml` | nginx ingress with host placeholder |
| `configmap.yaml` | Env vars from `.env.example` |

### Defaults

| Setting | Value |
|---|---|
| Replicas | 2 |
| Namespace | default |
| Ingress class | nginx |
| CPU request / limit | 100m / 500m |
| Memory request / limit | 128Mi / 512Mi |

### Notes
- Update the image name in `deployment.yaml` before deploying
- Replace host placeholder in `ingress.yaml` with your actual domain
- Re-run the agent after adding new env vars or changing the app port

## License

MIT
