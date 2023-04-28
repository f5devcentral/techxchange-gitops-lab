# Infrastructure Application Services

When a cluster is provisioned, there is usually a desire to install services that can be used cluster-wide by one or more user applications. These "infrastructure apps" can vary from security services, traffic shapers/routers, deployment operators, security vaults, dashboards, telemetry providers, etc.

Typically a platform operations engineer will ensure these services are available in a cluster based on the needs of *their* customers, the application owners, or developers. With the power of F5 Distributed Cloud's Managed Kubernetes offering, platform engineers can automate the deployment of a fleet of clusters worldwide, all interconnected with our best-in-class network.

We will introduce and deploy a few of these "infrastructure apps".

> **Note:** For learning purposes, in this lab we are manually creating the configurations that instruct Argo CD to deploy applications one at a time. In a real-world scenario, we should automate both the creation of the AppStack Managed Kubernetes cluster, the installation of Argo CD, complete with all application configurations to be installed once the cluster is available without requiring any additional intervention.

## Metrics and Visibility

Grafana is a multi-platform open source analytics and interactive visualization web application. It provides charts, graphs, and alerts for the web when connected to supported data sources.

The NGINX Ingress Controller (used later in this lab) provides a Grafana dashboard to provide analytical data related to the performance and behavior of the controller's underlying NGINX metrics. The NGINX Ingress Controller has been installed with support for providing these metrics in the Prometheus format, and we will leverage Grafana to consume these metrics, and provide a visual representation of this data.

You will install both Grafana and Prometheus into your Kubernetes cluster.

## Grafana Installation

We will leverage Argo CD to install NGINX Ingress Controller for us, leveraging Helm Charts.

[Helm](https://helm.sh/) helps you manage Kubernetes applications — Helm Charts help you define, install, and upgrade even the most complex Kubernetes application with minimal, declarative configuration.

Use `kubectl` to create an ArgoCD Application resource. ArgoCD uses Application resources to deploy applications whose configuration is present in a git repository.

1. Examine the contents of the ArgoCD Application resource by opening the `manifests/grafana-subchart.yaml` file in Visual Studio Code. Note the `spec.source.repoUrl` is already set to your repository's helm chart folder for Grafana. This is where ArgoCD will fetch the resources for the initial deployment, as well as monitor for changes.

1. Run the following command in the Visual Studio Code terminal:

    ```shell
    kubectl apply -f manifests/grafana-subchart.yaml
    ```

1. Verify the installation was successful by logging into Argo CD. Ensure that an application called `grafana` has been installed, and is in sync and healthy.

## Grafana Login

1. In your browser, open a new tab and log into Grafana by navigating to `https://grafana-<your namespace>.labs.f5demos.com`

    > **Note:** You set your namespace to an environment variable in the last section. You can retrieve it by running the following: `echo $XC_NAMESPACE`

1. When presented for login credentials, enter `admin` as the username. To acquire the password, you must interrogate k8s for the secret containing the password from Visual Studio Code terminal:

    ```bash
    kubectl get secret --namespace $XC_NAMESPACE grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
    ```

1. For now, you are just confirming that you can log in to Grafana; we will use it later in this lab so keep this tab open.

## Prometheus Installation

1. Examine the contents of the ArgoCD Application resource by opening the `manifests/prometheus-subchart.yaml` file in Visual Studio Code. Note the `spec.source.repoUrl` is already set to your repository's helm chart folder for Prometheus. This is where ArgoCD will fetch the resources for the initial deployment, as well as monitor for changes.

1. Run the following command in the Visual Studio Code terminal:

    ```shell
    kubectl apply -f manifests/prometheus-subchart.yaml
    ```

1. Verify the installation was successful by logging into Argo CD. Ensure that an application called `prometheus` has been installed, and is in sync and healthy.

Although Prometheus has a user interface to query data, we will not be using it in this lab. Grafana will be querying Prometheus for the metrics data it fetched from NGINX Ingress Controller.

## F5 XC Ingress Controller

The Ingress Controller manages external access to HTTP services in a Kubernetes cluster using the F5 Distributed Cloud Services Platform. The ingress controller is a K8s deployment that configures the HTTP Load Balancer using the K8s ingress manifest file. The Ingress Controller automates the creation of load balancer and other required objects such as VIP, Layer 7 routes (path-based routing), advertise policy, certificate creation(k8s secrets or automatic custom certificate), etc.

Though the XC Ingress Controller has some application routing features, we will be utilizing NGINX Ingress Controller for this. The value that XC Ingress Controller provides is its capability to create the necessary Load Balancer and Origin pool objects for the applications we expose - automatically, without having to spawn any custom workflows.

1. Deploy F5 XC Ingress Controller by running the following command in the Visual Studio Code terminal:

    ```shell
    kubectl apply -f manifests/volterra-ingress-controller-subchart.yaml
    ```

1. Verify the installation was successful by logging into Argo CD. Ensure that an application called `volterra-ingress-controller` has been installed, and is in sync and healthy.

1. Click on the created `volterra-ingress-controller` application card in ArgoCD.

1. Examine all the resources that were created for the XC Ingress Controller.

1. Click on the `pod` with the name starting with **volt-ic-**.

1. Click the **Logs** tab. Here you will see the logs for the XC Ingress Controller pod. You should not see any errors here.

    > **Note:** You can also see events generated by the XC Ingress Controller in the Managed K8s' Events panel in the F5 XC console.

## NGINX Ingress Controller

The NGINX Ingress Controller an implementation of a Kubernetes Ingress Controller for NGINX and NGINX Plus.

### What is the Ingress Controller?

The Ingress Controller is an application that runs in a cluster and configures an HTTP load balancer according to Ingress resources. The load balancer can be a software load balancer running in the cluster or a hardware or cloud load balancer running externally. Different load balancers require different Ingress Controller implementations.

In the case of NGINX, the Ingress Controller is deployed in a pod along with the load balancer.

### NGINX Ingress Controller Installation

We will again leverage Argo CD to install NGINX Ingress Controller for us, leveraging Helm charts.

1. Deploy NGINX Ingress Controller by running the following command in the Visual Studio Code terminal:

    ```shell
    kubectl apply -f manifests/nginx-ingress-subchart.yaml
    ```

1. Verify the installation was successful by logging into Argo CD. Ensure that an application called `nic` has been installed, and is in sync and healthy.

Now, the installation of our "infrastructure apps" is complete!

[Continue to next section...](brewz-application.md)
