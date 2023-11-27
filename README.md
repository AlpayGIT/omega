# Multi-Cluster Setup with GKE, Anthos, and ArgoCD

The fleet_prep.sh script automates the configuration of Google Cloud services and sets up a multi-cluster environment using GKE (Google Kubernetes Engine), Anthos, and ArgoCD.

## Prerequisites

Before running the script, ensure you have the following:

- A Google Cloud project.
- Appropriate permissions to enable services and create clusters.
- `gcloud` CLI installed and configured.

## Usage

1. **Clone the Repository**

    ```bash
    git clone <repository-url>
    cd <repository-directory>
    ```

2. **Set Environment Variables**

    Replace the placeholder values in the script with your specific configuration:

    ```bash
    export PROJECT_ID=<your-project-id>
    export SYNC_REPO=<your-sync-repo-url>
    export PAT_TOKEN=<your-personal-access-token>
    ```

3. **Run the Script**

    Execute the script to set up the multi-cluster environment:

    ```bash
    ./fleet_prep.sh -p $PROJECT_ID -r $SYNC_REPO -t $PAT_TOKEN
    ```

    This script performs the following steps:

    - Enables required Google Cloud services.
    - Creates an autopilot control cluster (if not already existing).
    - Connects to the control cluster and configures the context.
    - Adds IAM policy binding for the MCS service account.
    - Adds the control cluster to the fleet.
    - Configures multi-cluster ingress for the fleet.
    - Enables and configures Anthos Service Mesh.
    - Creates a global public IP for the Anthos Service Mesh Gateway.
    - Sets up ArgoCD on the control cluster, including configuring it for GKE Ingress.
    - Creates a global public IP for ArgoCD.
    - Configures Cloud Endpoint DNS.
    - Deploys ArgoCD Endpoint.
    - Sets up ArgoCD for GKE Ingress.
    - Configures a sync repository for ArgoCD.
    - Sets up application sets.

4. **Check Configuration**

    After the script completes, check the configuration by visiting:

    - Anthos Service Mesh Dashboard: [https://asm-gw-ip](https://asm-gw-ip)
    - ArgoCD Dashboard: [https://argocd.endpoints.PROJECT_ID.cloud.goog](https://argocd.endpoints.PROJECT_ID.cloud.goog)

# Creating a new app from the app template
One application cluster is ready to serve apps. Now all we need to do is create configs for a new app and push them up to the Argocd sync repo and all the prep we have done will simply allow this app to start serving traffic through the ASM gateway.

1. **Set Environment Variables**

    Replace the placeholder values in the script with your specific configuration:

    ```bash
    export APP_NAME=<your-app-name>
    export APP_IMAGE=<your-app-image>
    export PROJECT_ID=<your-project-id>
    export TEAM_NAME=<your-team-name>
    ```

2. **Run the Script**

    Execute the script to deploy the application and configure GitOps:

    ```bash
    ./team_app_add.sh -a $APP_NAME -i $APP_IMAGE -p $PROJECT_ID -t $TEAM_NAME
    ```

    This script performs the following steps:

    - Configures Cloud Endpoint DNS for the application.
    - Deploys the application-specific endpoint and SSL certificate.
    - Copies application templates to the appropriate directories.
    - Replaces placeholders in configuration files with provided values.
    - Commits and pushes changes to the Git repository for ArgoCD.
    - Applies configuration changes to ArgoCD for different waves.
  
# Add another application cluster to the Fleet

1. **Set Environment Variables**

    Replace the placeholder values in the script with your specific configuration:

    ```bash
    export PROJECT_ID=<your-project-id>
    export CLUSTER_NAME=<your-cluster-name>
    export CLUSTER_REGION=<your-cluster-region>
    export APP_DEPLOYMENT_WAVE=<deployment-wave>
    ```

2. **Run the Script**

    Execute the script to deploy the GKE cluster and integrate it with Anthos Fleet:

    ```bash
    ./fleet_cluster_add.sh -p $PROJECT_ID -n $CLUSTER_NAME -r $CLUSTER_REGION -w $APP_DEPLOYMENT_WAVE
    ```

    This script performs the following steps:

    - Checks if the cluster already exists; if not, creates a new GKE cluster.
    - Configures firewall rules for multi-cluster pod communication.
    - Registers the GKE cluster with Anthos Fleet.
    - Configures network tags and updates the cluster labels.
    - Configures namespaces and service accounts in the GKE cluster.
    - Creates a TLS secret for Anthos Service Mesh (ASM) gateways.
    - Updates Anthos Fleet management settings for the cluster.
