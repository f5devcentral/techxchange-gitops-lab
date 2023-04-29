# Lab Introduction

The objective of this lab is to demonstrate the deployment of an application into a Managed Kubernetes instance running on an AppStack Customer Edge (CE). We will deploy the F5 Distributed Cloud (XC) Ingress Controller to route traffic from the internet to an application, and NGINX Ingress Controller to provide granular routing capabilities to this application.

As an ideal, application code should be versioned and maintained in s secure Source Control Management (SCM) repository, such as Git. However, what about the configuration, management and governance of infrastructure?

## GitOps

GitOps is an operational framework that takes DevOps best practices used for application development such as version control, collaboration, compliance, and CI/CD, and applies them to infrastructure automation.

We will be practicing GitOps for Continuous Deployment of our applications. ArgoCD is the tool we will be using to watch your lab repository for changes, and automatically deploy updated resources to Kubernetes for you.

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

**You will need a GitHub Account for this lab. If you do not have one, go set one up at [GitHub](https://github.com/), then resume this lab. Make note of your username, as you will need it later in this lab.**

## Deploy the TechXchange GitOps UDF Blueprint

1. In your browser, navigate to the [Terraform Modular Demo Framework](https://udf.f5.com/b/99ed0091-30c5-4a2d-b8e0-e29574980c46#documentation) blueprint.

1. Click the **Deploy** button, and deploy it in the region geographically closest to you.

    <img src="assets/udf-01.png" alt="UDF Deploy" width="600"/>

    <img src="assets/udf-02.png" alt="UDF Deploy 2" width="600"/>

1. Start the UDF deployment with the default suggested resource settings.

    <img src="assets/udf-03.png" alt="UDF Resources" width="600"/>

    <img src="assets/udf-04.png" alt="UDF Resources 2" width="600"/>

## Log into the **devbox** VM in the UDF Deployment

1. If the **devbox** component is not running, start it now.

1. Select the **XRDP** access method in this component.

    <img src="assets/udf-05.png" alt="UDF Component" width="600"/>

    <img src="assets/udf-06.png" alt="UDF XRDP Access Method" width="600"/>

1. Once the RDP file downloads, open it with your Remote Desktop client of choice, usually by double-clicking on the downloaded file.

1. When prompted to login, use the credentials that are shown in the **Documentation** tab of the **devbox** UDF component.

## Log into F5 Distributed Cloud Console

1. Once you started the UDF deployment, a workflow was triggered to create a user account for you in the `f5-sales-demo` tenant. You should have received an email requesting you to set your password for this account. Follow those instructions in the email.

    > **Note:** If you already have an account in the `f5-sales-demo` XC tenant, you can simply log in with your existing credentials.

1. Once you are logged into the tenant, navigate to **Multi-Cloud App Connect**.

1. In the URL, you will find the namespace that has been randomly generated for you:

    <img src="assets/xc-namespace.png" alt="XC Namespace" width="800"/>

    > **Note:** If you already have an account in the `f5-sales-demo` XC tenant, you may have a personal application namespace. If not, create one now, and note its name.

1. Make a note of the above namespace, as you will need it in an upcoming step.

## Trigger build of lab environment

1. Open a terminal and run the following command to switch to the infrastructure branch we will use for this lab:

    ```bash
    cd ~/terraform-modular-demo-framework
    git checkout gitops-lab-merge
    ```

1. Run the following script to initiate the infrastructure build for this lab:

    ```bash
    export TF_VAR_namespace=<your xc namespace here>
    ./gitops-lab/setup-lab-environment.sh 
    ```

    > **Note:** When prompted to apply, type `y` then enter.

## Fork the lab repository

1. On your desktop (not the lab VM), you will need to fork the lab repository to your GitHub account. If this is your first time, then take a few minutes to review the [GitHub Docs on how to Fork a repo](https://docs.github.com/en/get-started/quickstart/fork-a-repo).

    You can complete this task through the [repository GitHub UI](https://github.com/f5devcentral/techxchange-gitops-lab):

    <img src="assets/gh-fork-1.png" alt="GitHub Fork" width="700"/>

    > **Note:** If you are a member of any GitHub organizations, be sure to select **yourself** as the owner, and not an organization (such as `f5devcentral` or `nginxinc`):

    <img src="assets/gh-fork-2.png" alt="GitHub Fork" width="800"/>

1. In the **devbox** VM, Open Visual Studio Code, then click **File -> New Window**

    > **Note:** If you are prompted with a *Authentication required prompt* to *Unlock Login Keyring*, then enter the same credentials used to RPD into the jumphost.

1. Click the **Terminal -> New Terminal** menu item to open a bash shell session if one is not already open at the bottom of the window.

## Configure GitHub authentication

Later in this lab, you will require GitHub authentication to push commits back to your forked repo.

1. Log into GitHub using the `gh` CLI:

    ```bash
    gh auth login -h github.com -p https -w
    ```

1. Type "Y" and enter when prompted to "Authenticate Git with your GitHub credentials?"

    Note: an authorization code will be shown. Copy this for use later.

1. Press **Enter** to open the browser.

1. Enter your GitHub username, then password.

1. Enter the authorization code when prompted

1. Click the **Authorize github** button on the next screen

## Clone your workshop repository

We will now clone your forked copy of the workshop repository to your lab workstation.

1. In Visual Studio Code, configure your Git name and email by entering the following into the terminal window. These will be used for Git commit logs:

    ```bash
    git config --global user.email "<your work email address>"
    git config --global user.name "<your full name>"
    ```

1. Create an environment variable with your GitHub username:

    > **Note:** Make sure to replace `<your github user name>` with actual your GitHub username.

    ```bash
    export GITHUB_USER=<your github user name>
    ```

1. Clone your repository via the git command:

    ```bash
    cd ~
    git clone https://github.com/$GITHUB_USER/techxchange-gitops-lab.git
    ```

1. Click **File -> Open Folder** and select the `techxchange-gitops-lab` folder that was just created from the command above.

## Set up your lab environment

1. Click the **Terminal -> New Terminal** menu item to open a bash shell session if one is not already open at the bottom of the window.

1. Run the following command in the terminal window to generate personalized Kubernetes manifests you will be using in this lab:

    > **Note:** Since this is a new bash session, you will once again need to specify your GitHub username. Make sure to replace `<your github user name>` with actual your GitHub username.

    ```shell
    export GITHUB_USER=<your github user name>
    gomplate -t gomplate-templates.tmpl
    ```

1. Using the Source Control pane, stage and commit the entire contents of the `charts` and `manifests` folders that were just created for you.

    <img src="assets/initial-commit.png" alt="Visual Studio Source Control commit" width="380"/>

1. Push the changes to your origin repository.

    <img src="assets/sync-repo.png" alt="Visual Studio Source Control sync" width="300"/>

1. You may see several prompts in a row:

    - If prompted "This action will push and pull...", click the **Ok** button.

    - If prompted by "The extension 'GitHub' wants to sign in using GitHub", click the **Open xdg-button** button and follow the prompts

    - If prompted with "Allow an extension to open this URI?", click the **Open** button.

    - If prompted to "Unlock Login Keyring", enter the password you used to connect to this Ubuntu workstation, and click the **Unlock** button.

1. Use the terminal to open the Chrome browser to your repository and verify that your changes were successfully pushed to your GitHub repo:

    ```shell
    google-chrome https://github.com/$GITHUB_USER/techxchange-gitops-lab.git
    ```

## Test AppStack Managed Kubernetes with Kubeconfig

To test interactions with the AppStack Kubernetes cluster, you will use the `kubeconfig`. A kubeconfig configuration file has already been provided for you on the **devbox** vm.

1. In the Visual Studio Code terminal window, use `kubectl` to test your new configuration:

    ```bash
    kubectl get nodes
    ```

    You should see the Kubernetes node in an output similar to this:

    ```shell
    NAME                                         STATUS   ROLES        AGE   VERSION
    ip-100-64-1-95.us-east-2.compute.internal    Ready    ves-master   12m   v1.23.14-ves
    ip-100-64-4-113.us-east-2.compute.internal   Ready    ves-master   12m   v1.23.14-ves
    ip-100-64-7-200.us-east-2.compute.internal   Ready    ves-master   12m   v1.23.14-ves
    ```

    Your AppStack Managed Kubernetes cluster is now ready to accept configuration.

[Continue to next section...](argocd.md)
