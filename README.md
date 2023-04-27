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
- ArgoCD Load Balancer is created via automation (TODO: need this to be HTTPs with a Let's Encrypt certificate)
- Student and `volt-ic` namespaces are created in the cluster
- F5 Distributed Cloud (XC) Ingress Secret is created in `volt-ic` namespace in the cluster
- NGINX container registry pull Secret is created in student namespace in the cluster

## Utilities Needed in UDF Blueprint (already installed for you)

- jq
- yq
- hey
- openssl
- gh (github cli)
- kubectl (v1.25.9)
- curl
- gomplate

## UDF Blueprint

TODO: Add this

## Lab guide

[Begin Lab](docs/README.md)
