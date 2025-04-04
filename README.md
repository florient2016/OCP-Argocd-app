# OCP-Argocd-app

This repository contains the OpenShift configuration files for deploying the `activities` application using ArgoCD. The application includes a Deployment, Service, Route, and ArgoCD Application configuration to automate the deployment process.

## Table of Contents

- [Overview](#overview)
- [Files](#files)
- [Prerequisites](#prerequisites)
- [Deployment Steps](#deployment-steps)
- [Application Details](#application-details)
- [License](#license)

## Overview

The `activities` application is a containerized application deployed on OpenShift. This repository includes the following resources:

- **Deployment**: Defines the application pods and their configurations.
- **Service**: Exposes the application internally within the cluster.
- **Route**: Exposes the application externally for public access.
- **ArgoCD Application**: Automates the deployment of the application using ArgoCD.

## Files

- `deploy.yaml`: Defines the Deployment resource for the `activities` application.
- `service.yaml`: Defines the Service resource to expose the application internally.
- `route.yaml`: Defines the Route resource to expose the application externally.
- `app-argocd.yaml`: Defines the ArgoCD Application resource to automate the deployment.

## Prerequisites

Before deploying the application, ensure the following:

1. You have access to an OpenShift cluster.
2. The `oc` CLI is installed and configured to interact with your OpenShift cluster.
3. ArgoCD is installed and configured on your OpenShift cluster.
4. The `openshift-gitops` namespace exists in your cluster.

## Deployment Steps

### Option 1: Deploy Manually

1. **Clone the repository**:

    ```bash
    git clone https://github.com/florient2016/OCP-Argocd-app.git
    cd OCP-Argocd-app
    ```

2. **Create the `dev` namespace**:

    ```bash
    oc create namespace dev
    ```

3. **Apply the Deployment**:

    ```bash
    oc apply -f deploy.yaml
    ```

4. **Apply the Service**:

    ```bash
    oc apply -f service.yaml
    ```

5. **Apply the Route**:

    ```bash
    oc apply -f route.yaml
    ```

6. **Verify the Deployment**:

    ```bash
    oc get pods -n dev
    ```

7. **Access the Application**:

    Retrieve the route URL:

    ```bash
    oc get route activities-route -n dev -o jsonpath='{.spec.host}'
    ```

    Open the URL in your browser to access the application.

### Option 2: Deploy with ArgoCD

1. **Clone the repository**:

    ```bash
    git clone https://github.com/florient2016/OCP-Argocd-app.git
    cd OCP-Argocd-app
    ```

2. **Apply the ArgoCD Application**:

    ```bash
    oc apply -f app-argocd.yaml -n openshift-gitops
    ```

3. **Monitor the Deployment**:

    Open the ArgoCD UI and verify that the `activities-app-dev` application is synced and healthy.

4. **Access the Application**:

    Retrieve the route URL:

    ```bash
    oc get route activities-route -n dev -o jsonpath='{.spec.host}'
    ```

    Open the URL in your browser to access the application.

## Application Details

- **Deployment**:
  - Image: `docker.io/komlan2019/activities:latest`
  - Replicas: 2
  - Resource Requests:
    - Memory: `128Mi`
    - CPU: `128m`
  - Resource Limits:
    - Memory: `512Mi`
    - CPU: `500m`

- **Service**:
  - Port: `80`
  - Target Port: `80`

- **Route**:
  - Exposes the application externally via the `activities-route`.

- **ArgoCD Application**:
  - Repository: `https://github.com/florient2016/OCP-Argocd-app.git`
  - Target Namespace: `dev`
  - Sync Policy: Automated with pruning and self-healing enabled.

## License

This project is licensed under the MIT License. See the LICENSE file for more details.