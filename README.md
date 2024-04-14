# GitHub Action for Deploying to Azure Kubernetes Service

This GitHub Action deploys a Dockerized application to an Azure Kubernetes Service (AKS) cluster. The Docker image is built and pushed to an Azure Container Registry, and then deployed to the AKS cluster.

## Secrets

This workflow requires several secrets to run:

- `AZURE_CREDENTIALS`: The JSON output of the Azure CLI command `az ad sp create-for-rbac --name "myApp" --role contributor --scopes /subscriptions/{subscription-id}/resourceGroups/{resource-group} --sdk-auth`. This command creates a service principal and outputs the credentials in JSON format. Replace `{subscription-id}` and `{resource-group}` with your Azure subscription ID and resource group name, respectively.

- `registry`: The name of your Azure Container Registry.

- `AZURE_USERNAME`: The username for your Azure Container Registry. This is usually the same as the `registry` secret.

- `AZURE_PASSWORD`: The password for your Azure Container Registry. You can find this in the Access keys section of your registry in the Azure portal.

- `repository`: The name of the Docker image repository in your Azure Container Registry.

- `resource_group`: The name of the resource group where your AKS cluster is located.

- `cluster_name`: The name of your AKS cluster.

## Usage

To use this workflow, add it to your repository as `.github/workflows/deploy.yml`. Then, set the required secrets in your repository's settings.

This workflow will run whenever a change is pushed to the `main` branch. It can be modified to run on other branches or events as needed.

## Workflow Steps

The workflow consists of two jobs: `build-and-push` and `deploy`.

The `build-and-push` job builds a Docker image from the Dockerfile in the current directory, tags it with the commit SHA, and pushes it to the Azure Container Registry.

The `deploy` job deploys the Docker image to the AKS cluster. It first sets the Kubernetes context to the AKS cluster, and then deploys the application using the deployment manifest located at `k8s/deployment.yaml`.
