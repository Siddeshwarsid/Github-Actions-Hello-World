# GitHub Actions Workflow: Deploy Hello-World Python App to Kubernetes

This repository contains a GitHub Actions workflow to automate the process of building, containerizing, and deploying a Python-based Hello-World application to a Kubernetes cluster (k8s).

## Workflow Overview

The workflow is defined in `.github/workflows/first-actiosn.yaml` and performs the following steps:
1. **Checkout the Repository**: Clones the repository to the GitHub Actions runner.
2. **Set up Python**: Installs Python and required dependencies for the application.
3. **Install Dependencies**: Installs the Python packages (`flask` and `gunicorn`) required for the Hello-World app.
4. **Set up kubectl**: Installs `kubectl` to interact with the Kubernetes cluster.
5. **Configure Kubeconfig**: Sets up the Kubernetes configuration using a secret stored in the repository.
6. **Build and Push Docker Image**: Builds a Docker image for the application and pushes it to Docker Hub.
7. **Debug Kubernetes Connection**: Verifies the connection to the Kubernetes cluster.
8. **Verify Kubeconfig**: Ensures the Kubernetes configuration is correctly set up.
9. **Deploy to Kubernetes**: Deploys the application to the Kubernetes cluster using a deployment manifest.

## Prerequisites

Before using this workflow, ensure the following prerequisites are met:

1. **Secrets Configuration**:
   - `KUBECONFIG`: Base64-encoded Kubernetes configuration file for accessing the cluster.
   - `DOCKER_USERNAME`: Docker Hub username.
   - `DOCKER_PASSWORD`: Docker Hub password.

2. **Kubernetes Cluster**:
   - A running Kubernetes cluster (e.g., k8s) accessible via the kubeconfig file.

3. **Docker Hub Repository**:
   - A Docker Hub repository to push the containerized application.

## Workflow Trigger

The workflow is triggered automatically on a `push` event to the `main` branch.

## Steps in the Workflow

### 1. Checkout Repository
The workflow uses the `actions/checkout` action to clone the repository to the runner.

### 2. Set up Python
The `actions/setup-python` action is used to install Python 3.x on the runner.

### 3. Install Dependencies
The workflow installs the required Python dependencies (`flask` and `gunicorn`) using `pip`.

### 4. Set up kubectl
The `azure/setup-kubectl` action is used to install the latest version of `kubectl`.

### 5. Configure Kubeconfig
The workflow decodes the `KUBECONFIG` secret and sets up the Kubernetes configuration file.

### 6. Build and Push Docker Image
The workflow builds a Docker image for the Hello-World Python app and pushes it to Docker Hub. The image is tagged as `siddeshwarsid/helloworld-py:latest`.

### 7. Debug Kubernetes Connection
The workflow verifies the connection to the Kubernetes cluster by checking the API server's version and testing the network connection.

### 8. Verify Kubeconfig
The workflow ensures the Kubernetes configuration is valid and retrieves the list of nodes in the cluster.

### 9. Deploy to Kubernetes
The workflow applies the Kubernetes deployment manifest (`k8s/deployment.yaml`) to deploy the application and waits for the deployment to complete.

### 10. Delete Kubernetes Deployment
The workflow deletes the Kubernetes deployment using the following command:
```bash
kubectl delete -f [deployment.yaml](http://_vscodecontentref_/0)
```

## Deployment Manifest

The deployment manifest (`k8s/deployment.yaml`) should define the Kubernetes resources required to run the Hello-World application, such as a Deployment and a Service.

## How to Use

1. **Fork or Clone the Repository**:
   ```bash
   git clone https://github.com/<your-username>/github-actions-deploy.git
   cd github-actions-deploy
   ```

2. **Set Up Secrets**: Add the required secrets (KUBECONFIG, DOCKER_USERNAME, DOCKER_PASSWORD) in the repository's Settings > Secrets and variables > Actions section.

3. **Push Changes**: Push changes to the main branch to trigger the workflow:
   ```bash
   git add .
   git commit -m "Trigger workflow"
   git push origin main
   ```

4. **Monitor Workflow**: Go to the Actions tab in your repository to monitor the workflow's progress.

**Cleanup**
To clean up resources created in the Kubernetes cluster, run:
```bash
kubectl delete -f [deployment.yaml](http://_vscodecontentref_/0)
```
