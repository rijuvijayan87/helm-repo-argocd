# ArgoCD Helm Chart

A simple Helm chart to deploy ArgoCD.

## Prerequisites

*   A running Kubernetes cluster (e.g., Minikube, Kind).
*   Helm 3 installed.

## Getting Started

1.  **Install the chart:**

    ```sh
    helm install argocd . --namespace argocd --create-namespace
    ```

2.  **Forward the ArgoCD server port:**

    ```sh
    kubectl port-forward svc/argocd-server -n argocd 8080:80
    ```

3.  **Get the initial admin password:**

    ```sh
    kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
    ```

4.  **Login:**

    Open `http://localhost:8080` in your browser.
    Login with username `admin` and the password from the previous step.
