# Self-Healing, Cost-Optimized EKS Platform 🚀

![License](https://img.shields.io/badge/License-MIT-blue.svg) ![Kubernetes](https://img.shields.io/badge/kubernetes-%23326ce5.svg?style=for-the-badge&logo=kubernetes&logoColor=white) ![AWS](https://img.shields.io/badge/AWS-%23FF9900.svg?style=for-the-badge&logo=amazon-aws&logoColor=white)

## 📖 Overview
This project demonstrates a **production-grade migration** from traditional Kubernetes autoscaling to **Karpenter**, combining **FinOps** (cost optimization) with resilience and observability in a fully automated EKS environment.

### Key Challenges Solved
1. **Cost Inefficiency:** Standard scaling was using 100% On-Demand nodes.
2. **Slow Node Recovery:** Node provisioning took 3–5 minutes during traffic spikes.

---

## 🏗 Architecture
The platform uses a **Hybrid Node Strategy**:

| Component          | Strategy                     | Technology                        |
|-------------------|------------------------------|----------------------------------|
| **Stateless Apps** | Spot Instances (80% Savings) | Karpenter NodePool (Default)      |
| **Databases**      | On-Demand Instances (Critical) | Karpenter NodePool (Critical)    |
| **Observability**  | Full Traceability            | OpenTelemetry + Jaeger           |
| **Resilience**     | Auto-Drain on Spot Interruption | AWS Spot Termination Handler    |

---

## 📸 Proof of Concepts

### 1. Hybrid Architecture (Spot vs. On-Demand)
Critical databases run on On-Demand nodes, while stateless frontend apps scale on Spot nodes.  
![Hybrid Nodes](https://github.com/sikander098/karpenter-finops-platform/raw/main/screenshots/1-hybrid-nodes.png.PNG)

### 2. Distributed Tracing (OpenTelemetry + Jaeger)
Trace requests from frontend to Redis/Postgres.  
![Jaeger Trace](https://github.com/sikander098/karpenter-finops-platform/raw/main/screenshots/2-jaeger-trace.png.PNG)

### 3. Automated Self-Healing
Karpenter detects AWS Spot Interruption Warnings and automatically cordons/drains affected nodes.  
![Self-Healing Logs](https://github.com/sikander098/karpenter-finops-platform/raw/main/screenshots/3-self-healing-log.png.PNG)

### 4. Karpenter Metrics Dashboard
Grafana visualizing provisioning time, spot usage %, and other metrics.  
![Grafana Dashboard](https://github.com/sikander098/karpenter-finops-platform/raw/main/screenshots/4-grafana-dashboard.png.PNG)

---

## 🛠 Tech Stack
* **Kubernetes:** Amazon EKS v1.29  
* **Autoscaling Engine:** Karpenter v1.0  
* **Monitoring & Metrics:** Kube-Prometheus-Stack (Prometheus / Grafana / Alertmanager)  
* **Tracing:** Jaeger & OpenTelemetry Operator  
* **Security:** IAM Roles for Service Accounts (IRSA)  

---

## 🚀 How to Deploy

### 1. Apply Karpenter NodePools
```bash
kubectl apply -f infra/karpenter-pool.yaml
kubectl apply -f infra/karpenter-ondemand.yaml
2. Deploy OpenTelemetry Demo
bash
Copy code
helm upgrade --install otel-demo opentelemetry-demo/ -f apps/sre-values.yaml
3. Verify Automated Node Healing
bash
Copy code
kubectl get nodes
Nodes on Spot instances should automatically be cordoned/drained on AWS Spot Interruption Warnings.

🔑 Key Learnings
Hybrid Node Pools drastically reduce costs while keeping critical workloads safe.

In-cluster self-healing ensures zero downtime on node termination.

Full observability provides actionable insights on provisioning times and resource usage.

Automated metrics & dashboards turn engineering work into measurable business value.
