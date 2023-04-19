# Lab Introduction

The objective of this lab is to demonstrate the deployment of an application into a Managed Kubernetes instance running on an AppStack Customer Edge (CE). We will deploy the F5 Distributed Cloud (XC) Ingress Controller to route traffic from the internet to an application, and NGINX Ingress Controller to provide granular routing capabilities to this application.

As an ideal, application code should be versioned and maintained in s secure Source Control Management (SCM) repository, such as Git. However, what about the configuration, management and governance of infrastructure?

## GitOps

GitOps is an operational framework that takes DevOps best practices used for application development such as version control, collaboration, compliance, and CI/CD, and applies them to infrastructure automation.

We will be practicing GitOps for Continuous Deployment of our applications. Argo CD is the tool we will be using to watch your lab repository for changes, and automatically deploy updated resources to Kubernetes for you.

## Lab Architecture

Upon first starting the lab, an XC AppStack Managed K8s cluster has been deployed for you in an AWS VPC. An namespace for you to deploy application into has been created, and the ArgoCD application has been installed for you.

Upon successful completion of this lab, the following components will have been deployed by you with the help of ArgoCD:

- Grafana and Prometheus for metrics collection and analytics dashboards
- An F5 XC Load Balancer for the Grafana UI
- XC Ingress Controller for creation of Load Balancers & Origin Pools for service connectivity
- NGINX Ingress Controller for advanced traffic routing
- A microservices application called "Brewz", complete with XC Load Balancers & Origin Pools for service connectivity

<img src="assets/lab-architecture.png" alt="Lab architecture" width="900"/>

## Getting Started

## Deploy the TechXchange GitOps UDF Blueprint

TODO: Need final link for BP

1. Open the TechXchange GitOps Lab UDF Blueprint and deploy it in the region geographically closest to you. Start the deployment with the default suggested resource settings.

## Fork the lab repository

1. You will need to fork the lab repository to your GitHub account.  If this is your first time, then take a few minutes to review the [GitHub Docs on how to Fork a repo](https://docs.github.com/en/get-started/quickstart/fork-a-repo).

    You can complete this task through the [repository GitHub UI](https://github.com/f5devcentral/techxchange-gitops-lab):

    <img src="assets/gh-fork-1.png" alt="GitHub Fork" width="700"/>

    Note: If you are a member of any GitHub organizations, be sure to select **yourself** as the owner, and not an organization (such as `f5devcentral` or `nginxinc`):

    <img src="assets/gh-fork-2.png" alt="GitHub Fork" width="800"/>

## Log into the **appdev** vm in the UDF Deployment

1. If the **appdev** component is not running, start it now.

1. Select the **xRDP** access method in this component.

1. Once the RDP file downloads, open it with your Remote Desktop Client of choice, usually by double-clicking on the downloaded file.

1. When prompted to login, use the credentials that are shown in the **Documentation** tab of the **appdev** UDF component.

## Clone your workshop repository

Now that you have forked the workshop repository, you'll want to clone the repo to your lab workstation.

1. Configure `git` authentication in the **appdev** by following [these instructions](https://cli.github.com/manual/gh_auth_login). Later in this lab, you will require GitHub authentication to push commits back to your forked repo.

1. TODO: show gh cli auth login steps

1. Perform this via the git or GitHub CLI commands.

    > **Note:** Make sure to replace your_username with your GitHub username.

    ```bash
    GITHUB_USER=<your github user name>
    git clone https://github.com/$GITHUB_USER/techxchange-gitops-lab.git techxchange-gitops-lab
    ```

## Test AppStack Managed Kubernetes with Kubeconfig

To test interactions with the AppStack Kubernetes cluster, you will use the `kubeconfig`. A kubeconfig configuration file has already been provided for you on the **appdev** vm.

1. In the **appdev** VM, open the **Terminal** application.

1. Use kubectl to test your new configuration:

    ```bash
    kubectl get nodes
    ```

    You should see the Kubernetes node in an output similar to this:

    ```shell
    NAME                                        STATUS   ROLES        AGE   VERSION
    ip-100-64-1-16.us-east-2.compute.internal   Ready    ves-master   86m   v1.23.14-ves
    ```

    Your Managed Kubernetes cluster is now ready to accept configuration.

## Set up your lab environment

1. In the **appdev** VM, open Visual Studio Code.

1. Open the `~/techxchange-gitops-lab` folder in Visual Studio Code.

1. Click the **Terminal -> New Terminal** menu item. This will open a terminal to the root of your repository.

1. Run the following command in the terminal window to generate the Kubernetes manifests you will be using in this lab:

    ```shell
    gomplate -t gomplate-templates.tmpl
    ```

1. Stage and commit the entire contents of the `charts` and `manifests` folders that were just created for you.

1. Push the changes to your origin repository.


[Continue to next step...](argocd.md)
