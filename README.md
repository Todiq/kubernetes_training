# Architecture

```mermaid
graph TB
    User["Utilisateur / Navigateur"]

    subgraph Minikube["Cluster Minikube"]
        subgraph HelmChart["Helm Chart — myapp-chart"]
            SVC["Service\n(NodePort :5000)"]
            CM["ConfigMap\nAPP_MESSAGE"]
            SA["ServiceAccount"]
            HPA["HorizontalPodAutoscaler\n(optionnel)"]
            ING["Ingress\n(optionnel)"]

            subgraph DEP["Deployment (2 réplicas)"]
                POD1["Pod 1\nFlask app :5000"]
                POD2["Pod 2\nFlask app :5000"]
            end
        end
    end

    subgraph Build["Build local"]
        SRC["app.py"]
        DF["Dockerfile\n(python:3.11-slim)"]
        IMG["Image Docker\nmyapp:1.0.0"]
        SRC --> DF --> IMG
    end

    IMG -->|minikube image build| DEP
    CM -->|envFrom| POD1
    CM -->|envFrom| POD2
    SA --> DEP
    HPA -->|scale| DEP
    SVC --> POD1
    SVC --> POD2
    ING -->|optionnel| SVC
    User -->|minikube service| SVC
```

# Installation

1. Install [minikube](https://minikube.sigs.k8s.io/docs/start/)

2. Install [helm](https://helm.sh/docs/intro/install/)

3. Clone the repo:
```bash
git clone --depth 1 --branch main https://github.com/Todiq/kubernetes_training.git
```

4. Build the app:
```bash
minikube start && cd kubernetes_training && minikube image build -t myapp:1.0.0 .
```

5. Deploy the app:
```bash
helm install myapp ./myapp-chart
```

6. Access the app from the browser
```bash
minikube service myapp-myapp-chart
```

7. Dynamically update the displayed text:
```bash
helm upgrade myapp ./myapp-chart --set appConfig.message='Hello there!'
```

8. Rollback:
```bash
helm rollback myapp
```

9. Uninstall the app:
Kill the `minikube service myapp-myapp-chart` process, then run
```bash
helm uninstall myapp && minikube stop
```