DevOps Challenge – End-to-End CI/CD with .NET and AKS

Overview

This project demonstrates a complete DevOps workflow that automates the deployment of a .NET 8 application to Azure Kubernetes Service (AKS) using GitHub Actions and Terraform. The application is containerized using Docker and deployed to AKS via Helm charts.

The CI/CD pipeline is designed to be modular, automated, and scalable, incorporating best practices such as Infrastructure as Code (IaC), declarative deployments, and secure image publishing.

CI/CD Workflow

GitHub Actions is used to orchestrate the CI/CD flow, which includes:

Build & Test

The .NET 8 application is built using dotnet build.

Optional unit tests can be executed in the same workflow.

Dockerization

A multi-stage Dockerfile is used to publish and package the app.

The resulting image is tagged and pushed to Azure Container Registry (ACR).

Helm Deployment

A Helm chart deploys the application to AKS.

Configurable values (e.g., image tag, port) are passed during deployment.

Infrastructure as Code (Terraform)

Terraform is used to provision all necessary Azure resources. The IaC setup is modular and reusable.

Modules:

Resource Group: Hosts all infrastructure resources.

Azure Kubernetes Service (AKS): Manages the Kubernetes cluster.

Azure Container Registry (ACR): Stores Docker images.

PostgreSQL (Stub): Simulates a production-grade database using flexible configuration.

Example Terraform Structure:

terraform/
│
├── main.tf
├── variables.tf
├── outputs.tf
├── modules/
│ ├── aks/
│ ├── acr/
│ └── postgres/

Terraform Architecture Rationale.

Modular design supports code reuse and easy maintenance.

Service principals and RBAC are used to separate access concerns.

AKS is configured with a system-assigned identity to pull from ACR securely.

Helm charts are invoked from GitHub Actions for consistency.

Helm

The Helm chart is simple and customizable. It includes:

Deployment

Service (LoadBalancer)

Configurable ports, image repository, tag, and resource limits

Assumptions and Simplifications

PostgreSQL is provisioned as a stub; no sensitive or persistent data is used.

The AKS cluster uses a basic VM size for cost efficiency.

DNS and SSL termination are not configured, but the app is exposed via a public IP.

No ingress controller is configured; direct LoadBalancer exposure is used.

Monitoring & Logging Suggestions (Optional)

Azure Monitor and Container Insights can be enabled on AKS for built-in metrics.

Application logs can be collected using fluentd or Azure Log Analytics.

Prometheus and Grafana may be added for custom dashboards and alerts.

How AI Tools Supported the Work

Some code generation and syntax structuring were assisted by automation tools to improve consistency and speed. Final design, testing, and integration decisions were made manually.

Getting Started

Clone the repository.

Configure the required secrets in GitHub:

ACR_NAME

CLIENT_ID

CLIENT_SECRET

SUBSCRIPTION_ID

TENANT_ID

Deploy infrastructure:
cd terraform
terraform init
terraform apply

Push to main branch to trigger CI/CD pipeline.

Access the app:

The LoadBalancer IP will be printed by Terraform or visible via:
kubectl get svc

Conclusion

This solution provides a foundation for modern cloud-native .NET application deployment using Azure and Kubernetes, following infrastructure-as-code and CI/CD best practices.

Let me know if you'd like this in a downloadable file format.

