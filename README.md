# Simple ArgoCD Deployment

This repository contains a Helm chart for deploying ArgoCD to a Kubernetes cluster. This `README.md` provides basic steps to get ArgoCD up and running in a local development environment.

## Prerequisites

Before you begin, ensure you have the following:

*   **Kubernetes Cluster:** A running Kubernetes cluster (e.g., Minikube, Kind, Docker Desktop Kubernetes).
*   **Helm:** Helm 3 installed on your local machine.

## Deployment Steps

1.  **Navigate to the Chart Directory:**
    Change your current directory to the root of this repository where the `Chart.yaml` file is located.

    ```bash
    cd /path/to/your/helm-repo-argocd
    ```

2.  **Install the ArgoCD Helm Chart:**
    Deploy ArgoCD to your Kubernetes cluster using Helm. You can choose a namespace for your ArgoCD installation (e.g., `argocd`).

    ```bash
    helm install argocd . --namespace argocd --create-namespace
    ```

    *   `argocd`: This is the release name for your ArgoCD installation. You can choose any name.
    *   `.`: This indicates that the Helm chart is in the current directory.
    *   `--namespace argocd --create-namespace`: This creates a new namespace called `argocd` and deploys the chart into it.

3.  **Verify Deployment:**
    Wait for all ArgoCD pods to be running. You can check their status with:

    ```bash
    kubectl get pods -n argocd
    ```

    Ensure all pods are in a `Running` or `Completed` state.

## Accessing ArgoCD

1.  **Port-Forward the ArgoCD Server:**
    The ArgoCD UI is exposed via a `ClusterIP` service by default. To access it locally, you need to port-forward the `argocd-server` service:

    ```bash
    kubectl port-forward svc/argocd-server -n argocd 8080:80
    ```

    This will make the ArgoCD UI available at `http://localhost:8080`. Keep this command running in a separate terminal.

2.  **Retrieve the Initial Admin Password:**
    The initial password for the `admin` user is stored in a Kubernetes secret. Retrieve it using:

    ```bash
    kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
    ```

    Copy this password.

3.  **Login to ArgoCD UI:**
    Open your web browser and go to `http://localhost:8080`.
    Login with username `admin` and the password you retrieved in the previous step.

4.  **Login via ArgoCD CLI (Optional):**
    If you have the ArgoCD CLI installed, you can also log in from your terminal:

    ```bash
    argocd login localhost:8080
    ```

    When prompted, enter `admin` for the username and the retrieved password.

You are now ready to use ArgoCD!