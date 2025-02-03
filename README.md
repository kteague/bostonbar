# BostonBar

BostonBar is a cloud orchestration platform for Kubernetes that is designed to synchronize the creation of cloud resources across multiple environments and tools.

Applications and services deployed to Kubernetes often rely on supporting resources in other clouds. For example, an AWS EKS Kubernetes cluster may require resources such as IAM Roles and Policies to enable Kubernetes services to interact with AWS. A typical way to solve this problem is use Terraform to provision the cloud resources and feed the output of the identifiers of the supporting AWS resources into Helm charts which are deploy into Kubernetes with ArgoCD. Every time a terraform workspace is run, ARN outputs need to be feed into Helm charts.

BostonBar is a way to declare your application's cloud dependencies with a formal schema and then provide automation to inspect, report and synchronize the running of your cloud pipelines between each other.

Or at least that's the vision ...

## BostonBar PoC

Goal: Read the state and outputs of terraform pipelines with AWS IAM resources

Initial Demo

  * Terraform pipeline for a Route 53 HostedZone

  * Terraform pipeline for IAM Resources to support ExternalDNS to participate in that Route53

  * ArgoCD deploys a Helm chart with ExternalDNS
  
YAML file to declare:

 - Cloud Orchestrator
    - Terraform pipeline (state file, outputs)
        + Terraform/AWS/Route53
        + Terraform/AWS/IAM
    - CloudFormation
    - CDK
    - Pulumi
 - KubernetesApplication
   - Dependencies --> Route53, IAM
   - GitOpsTarget

Parse YAML, read outputs from Terraform pipeline, determine dependencies and feed those values into a git repo.
