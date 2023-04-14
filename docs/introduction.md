# Lab Introduction

The objective of this lab is to demonstrate the deployment of an application into a Managed Kubernetes instance running on an AppStack Customer Edge (CE). We will deploy the XC Ingress Controller to route traffic from the internet to an application, and NGINX Ingress Controller to provide granular routing capabilities to this application.

As an ideal, application code should be versioned and maintained in s secure Source Control Management (SCM) repository, such as Git. However, what about the configuration, management and governance of infrastructure?

## GitOps

GitOps is an operational framework that takes DevOps best practices used for application development such as version control, collaboration, compliance, and CI/CD, and applies them to infrastructure automation.

We will be practicing GitOps for Continuous Deployment of our applications. Argo CD is the tool we will be using to watch your lab repository for changes, and automatically deploy updated resources to Kubernetes for you.

## Getting Started

## Deploy the TechXchange GitOps UDF Blueprint

TODO: Need link for BP

1. Open the TechXchange GitOps Lab UDF Blueprint and deploy it in the region geographically closest to you. Start the deployment with the default suggested resource settings.

## Fork the lab repository

1. You will need to fork the lab repository to your GitHub account.  If this is your first time, then take a few minutes to review the [GitHub Docs on how to Fork a repo](https://docs.github.com/en/get-started/quickstart/fork-a-repo).

    You can complete this task through the [repository GitHub UI](https://github.com/f5devcentral/techxchange-gitops-lab):
    ![GitHub Fork](../assets/gh_fork.jpg)

## Log into the Ubuntu Workstation in the UDF Deployment

1. TODO: (STEPS HERE...)

## Clone your workshop repository to your UDF deployment

Now that you have forked the workshop repository, you'll want to clone the repo to your local laptop.  

1. Perform this via the git or GitHub CLI commands.

    > **Note:** Make sure to replace your_username with your GitHub username.

    > **Note:** If you have not [configured GitHub authentication](https://docs.github.com/en/authentication) with your local laptop, please stop and do that now.

    ```bash
    git clone https://github.com/your_username/techxchange-gitops-lab.git techxchange-gitops-lab
    ```

## Get Kubeconfig

To interact with the AppStack Kubernetes cluster, you will need to download and configure a kubeconfig file on the Ubuntu workstation.

1. In the UDF blueprint, use the RDP access method

1. TODO: NEED STEPS HERE

1. Use kubectl to test your new configuration:

    ```bash
    kubectl get nodes
    ```

    You should see the Kubernetes node:

    ```shell
    TODO: fill this in
    ```

[Continue to next step...](argocd.md)
