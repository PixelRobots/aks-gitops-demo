# Cluster configuration sample for GitOps on AKS

This repository contains sample Kubernetes manifest files that can be deployed using GitOps to an Azure Kubernetes Service (AKS) cluster. This repo is based on the [arc-k8s-demo](https://github.com/Azure/arc-k8s-demo) from Microsoft.

## Contents

| File/folder       | Description                                |
|-------------------|--------------------------------------------|
| `README.md`       | This README file. |
| `cluster-apps`    | Contains an example application that should be deployed to every cluster. |
| `namespaces`    | Contains three namespace resources to provision on an attached cluster. |
| `pixel`    | Contains a ConfigMap resource written into `pixel` namespace, communicates configuration data for the application team. |

## Prerequisites

One or more AKS clusters with the GitOps AKS add-on enabled. \
The `k8s-configuration` Azure CLI extension installed. \
The below resource providers registered:

```console
az provider register --namespace Microsoft.Kubernetes
az provider register --namespace Microsoft.KubernetesConfiguration
az provider register --namespace Microsoft.ExtendedLocation
```


## Running the sample

Create a GitOps configuration referencing this sample repo by using the Azure CLI extension `k8s-configuration`. Complete documentation on the available options can be found [here](https://pixelrobots.co.uk).

After a configuration is created, AKS will instantiate the resources described in this repository. This includes creating namespaces, deploying an example workload, and a config map.

```console
az k8s-configuration create \
    --name cluster-config \
    --cluster-name ${CLUSTER_NAME} --resource-group ${RESOURCE_GROUP} \
    --operator-instance-name cluster-config --operator-namespace cluster-config \
    --repository-url https://github.com/PixelRobots/aks-gitops-demo \
    --scope cluster --cluster-type managedClusters \
    --operator-params='--git-branch=main'
```

