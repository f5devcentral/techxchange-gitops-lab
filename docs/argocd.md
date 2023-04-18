# Argo CD

[Argo CD](https://argoproj.github.io/cd/) is a declarative, GitOps continuous delivery tool for Kubernetes.

Argo CD is the tool we will be using to watch your lab repository for changes, and automatically deploy updated resources to Kubernetes for you. Argo CD runs as a Kubernetes application itself, and "pulls" in applications to the cluster it runs on, or other external clusters. ArgoCD has already been installed for you in your lab cluster.

In addition to deploying applications for the first time, Argo CD can be used to ensure the state of your application remains consistent with an application's configuration stored in Git. Argo CD continuously monitors the state of application configuration in Git, and compares it to the state of the deployed application in Kubernetes. Argo CD will update an application's state if it detects configuration drift.

This is a fundamental tenet in GitOps: Source control is the source of truth for an application (or infrastructure), and configuration changes to a deployed application outside of this process will not be tolerated, and will be corrected by Argo CD.

## Login to Argo CD

1. Obtain the Argo CD password by running the following from a terminal in the **appdev** vm:

    ```bash
    echo $ARGOCD_PASSWORD
    ```

1. In your browser, navigate to <your namespace>-argocd.sales-demo.f5demos.com

1. Login with the `admin` user and the password you obtained above.

1. In the Argo CD UI, note that there are no applications TODO: Add screenshot

Next, we will deploy additional applications to your Kubernetes cluster leveraging Argo CD.

[Continue to next step...](infra-apps.md)
