# GitOps TechXchange Lab

Objective:
Deploy an application in F5 Distributed Cloud (XC) Managed Kubernetes that uses NGINX Ingress Controller (Plus edition) with XC Load Balancer ingress.

## Prerequisites

Each student needs:

- A GitHub account
- A remote desktop client

## Environment Preconditions

- AppStack is installed in a CSP as Managed Kubernetes node
- ArgoCD is installed via automation
- ArgoCD Load Balancer and Origin Pool is created via automation
- Grafana Load Balancer and Origin Pool is created via automation
- Student and `volt-ic` namespaces are created in the cluster
- F5 Distributed Cloud (XC) Ingress Secret is created in `volt-ic` namespace in the cluster
- NGINX container registry pull secret is created in student namespace in the cluster

## Utilities Needed in UDF Blueprint (already installed for you)

- [jq](https://stedolan.github.io/jq/)
- [yq](https://github.com/mikefarah/yq)
<!-- - [hey](https://github.com/rakyll/hey) -->
- [gh (github cli)](https://cli.github.com/)
- [kubectl (v1.25.9)](https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html)
- [curl](https://curl.se/docs/manpage.html)
- [gomplate](https://docs.gomplate.ca/)

## UDF Blueprint

TODO: Need final link for BP

## Lab guide

[Begin Lab](docs/README.md)
