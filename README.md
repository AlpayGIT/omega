# Multi-Cluster Setup with GKE, Anthos, and ArgoCD

This Bash script automates the configuration of Google Cloud services and sets up a multi-cluster environment using GKE (Google Kubernetes Engine), Anthos, and ArgoCD.

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

    The script also outputs the URLs for your convenience.
