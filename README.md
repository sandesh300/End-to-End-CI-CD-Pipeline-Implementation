# 🚀 Jenkins CI/CD Pipeline for Java Apps with Maven, SonarQube, Argo CD, Helm & Kubernetes
---
This repository demonstrates an **end-to-end DevOps pipeline** for a Java-based application using:

* **Jenkins** – CI orchestration
* **Maven** – Build & dependency management
* **SonarQube** – Static code analysis
* **Helm** – Kubernetes packaging
* **Argo CD** – GitOps-based CD
* **Kubernetes** – Runtime platform

---

## 🏗️ Architecture Overview
![Screenshot 2023-03-28 at 9 38 09 PM](https://user-images.githubusercontent.com/43399466/228301952-abc02ca2-9942-4a67-8293-f76647b6f9d8.png)


**Flow summary:**

1. Developer pushes code to **Git**
2. Jenkins pipeline triggers automatically
3. Maven builds & tests the application
4. SonarQube scans code quality
5. Docker image + Helm chart updated
6. Jenkins commits Helm changes to GitOps repo
7. Argo CD syncs cluster state
8. App runs on Kubernetes

---

## 📋 Prerequisites

Before starting, ensure you have:

* Java application source code in Git
* Jenkins server with pipeline support
* Kubernetes cluster (EKS / AKS / GKE / on-prem)
* Helm installed
* Argo CD deployed in the cluster
* Docker registry (ECR / Docker Hub / GCR)

---

## 🔌 Jenkins Plugins Required

Install the following inside Jenkins:

* Git Plugin
* Maven Integration Plugin
* Pipeline Plugin
* Kubernetes Continuous Deploy Plugin
* SonarQube Scanner Plugin

---

## 🧩 Pipeline Stages

The Jenkins pipeline is defined inside a **`Jenkinsfile`** and contains:

### **Stage 1 – Checkout Code**

Pulls source from Git.

### **Stage 2 – Build**

Runs:

```bash
mvn clean compile
```

### **Stage 3 – Unit Testing**

Executes tests using:

* **JUnit**
* **Mockito**

```bash
mvn test
```

---

### **Stage 4 – SonarQube Scan**

Runs static analysis:

```bash
mvn sonar:sonar
```

Quality gates decide whether the pipeline continues.

---

### **Stage 5 – Package Artifact**

Creates JAR:

```bash
mvn package
```

---

### **Stage 6 – Deploy to Test Environment (Helm)**

Helm installs or upgrades the test release:

```bash
helm upgrade --install bankapp ./helm-chart \
  --namespace test
```

---

### **Stage 7 – UAT / Integration Tests**

Automated UI or API tests using:

* **Selenium**

---

### **Stage 8 – Production Promotion via Argo CD**

Jenkins updates Helm values in GitOps repo →
Argo CD detects commit → syncs cluster → production rollout 🎯

---

## ⚙️ Argo CD Setup

1. Install Argo CD in Kubernetes:

```bash
kubectl create namespace argocd
kubectl apply -n argocd \
 -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

2. Create a **GitOps repository**:

   * Stores Helm charts
   * Environment-specific values (`values-dev.yaml`, `values-prod.yaml`)

3. Register repository in Argo CD UI or CLI

4. Create Argo CD Application:

```bash
argocd app create bankapp-prod \
  --repo https://github.com/org/bankapp-gitops \
  --path helm/bankapp \
  --dest-server https://kubernetes.default.svc \
  --dest-namespace prod
```

---

## 🔐 Jenkins ↔ Argo CD Integration

1. Generate Argo CD API token:

```bash
argocd account generate-token
```

2. Store token in Jenkins **Credentials Manager**

3. Jenkins deployment stage example:

```groovy
stage('Promote to Production') {
  steps {
    sh """
      argocd login argocd.example.com \
        --username admin \
        --password $ARGOCD_TOKEN \
        --insecure

      argocd app sync bankapp-prod
    """
  }
}
```

---

## 📂 Repository Structure

```
.
├── Jenkinsfile
├── pom.xml
├── src/
├── helm-chart/
│   ├── Chart.yaml
│   ├── values.yaml
│   └── templates/
└── docker/
    └── Dockerfile
```

---

## 🧠 What This Project Demonstrates

✅ Complete CI/CD automation
✅ Code quality gates
✅ GitOps deployment model
✅ Kubernetes production rollout
✅ Helm-based environment control
✅ Enterprise-ready DevSecOps flow

---

## ⭐ If You Like This Project…

Give it a ⭐ and feel free to fork & extend for:

* AWS EKS
* Canary deployments
* Blue-Green releases
* Prometheus + Grafana monitoring
* Trivy image scanning

---

If you want, I can also:

👉 Add a **sample Jenkinsfile**
👉 Add Helm chart templates
👉 Add Argo CD Application YAML
👉 Convert this into a DevSecOps-style README with security scanning stages 😎
