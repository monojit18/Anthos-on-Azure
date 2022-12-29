# Born and Nourished on GCP, Living on Azure

## Introduction

Today majority of the customers are looking towards multiple CSPs/Vendors to run their Cloud workloads at sccale and also run, manage, operate them seamlessly - whenver and wherever!

Single-cloud architecture is highly dependent on specific CSPs and their tools, services. Anthos provies a single pane of glass to manage workloads from multiple clouds with a unified management and configuration plane. Customers having containerized workloads that need to run on multiple clouds viz. AWS, Azure - can now be deployed, managed, configured from GCP Console or CLI. Moreover with the support for [Bare Metal](https://cloud.google.com/anthos/clusters/docs/bare-metal/latest), customers can now create, manage and operate K8s clusters on their own hardware.

This document would focus on an end to end implementation of creating and managing K8s cluster on Azure from GCP through Anthos. The following diagram depicts how Anthos simplifies the multi-cloud workload management.

![why-anthos-multi-cloud](./Assets/why-anthos-multi-cloud.png)



## Anthos Technical Architecture

![anthos-18-components](./Assets/anthos-18-components.png)

A [detailed technical overview](https://cloud.google.com/anthos/docs/concepts/overview) on Anthos multi-cloud depicts all its components and features and how it facilitates infrastructure management, confguration and application deloyment acorss multiple environments.

## Anthos on Azure

![azure-architecture](./Assets/azure-architecture.png)



Following is a list of Core components that are relevant to this discussion i.e. **[Anthos for Azure](https://cloud.google.com/anthos/clusters/docs/multi-cloud/azure)**

- **Anthos on Azure**
  - All components are hosted on the customer's Azure environment
  - Anthos only manages its own services that are running on the K8s cluster on Azure
  - The **Control Plane** Instances and **Worker Node Pools** are all managed by Azure only
- **Multi-cluster management**
  - Anthos provides a Fleet manager to host multiple K8s clusters
  - [Anthos Multi-cloud API](https://cloud.google.com/anthos/clusters/docs/multi-cloud/reference/rest) allows GCP to create resources onto Azure platform of Customer on their behalf.
  - A [Connect Agent](https://cloud.google.com/anthos/fleet-management/docs/connect-agent) deployment is used to establish a connection between the cluster and GCP project. GCP console can get information about the K8s workload running on the cluster on Azure through Connect Agent deployment
  - Multiple clusters can be viewed and managed together in the **Anthos Dashboard**
- **Load balancers**
  - Provides integrations with Azure standard [Load balancers](https://cloud.google.com/anthos/clusters/docs/multi-cloud/azure/how-to/network-load-balancing)

## Steps to build this

Following are the steps we would follow as we move on:

- Login and Connect to the Azure portal
- Create a Resource Group to host all resources to be created for the K8s cluster
- Create Azue Virtual Network with a /16 prefix and Subnet with a /22 prefix to host K8s cluster resources
- Create an Azure  Public IP and associate it with NAT gateway. And then associate it with the K8s Subnet create above
- 