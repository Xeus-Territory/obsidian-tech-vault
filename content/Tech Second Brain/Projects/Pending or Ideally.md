---
title: "Project with DIY purpose: Pending or Ideally"
tags:
  - devops
  - research
  - infosec
  - devsecops
  - self-hosted
  - cloud-services
  - DIY
  - docker
  - usage
  - solutions
  - architecture
  - k8s
draft: "true"
---
# Prehook framework of wouops

>[!summary]
>Create a framework on light weight and isolation components for check commit and push event in currently source code to repositories, with features like
>- Code smell and validate code for improving the code quality
>- Handling prehook for pentesting surface application on whitebox like secret leak, CVE, misconfiguration
>- Offer the announcement location, can be grafana or report with dashboard about your commit

# Setup Kubernetes Cluster locally + Customize script for manipulation scheduled Kubernetes

>[!summary]
>Use the instant technologies for setting up Kubernetes cluster, with
>- Vargrant - Template the kubernetes cluster with HyperV, VMware, VMbox
>- Ansible - To setup and run script and bring up kubernetes cluster on locally
>- Use Kubeadm to expand, manage kubernetes nodes
>- Use Cilium or Calico to setup ebpf for observe, networkng or Apply service mesh in cluster
>- Use extend CSI for volume in kubernetes
>- Setup etcd in kubernetes
>- Cusmtomize default scheduled in kubernetes cluster
>- Setup the monitoring cluster inside the cluster

>[!summary]
>Create a customize script for optimize and manipulation scheduled instead of default for Kubernetes, with feature like
>- Determine density of all workloads and nodes to scheduling
>- Be able to overwrite or set priority for nodes, workloads to scheduling
>- Work great with another cluster configuration like AKS, EKS or Kubeadm, ...

# HTTP Tunneling for secure connection

>[!summary]
>Tunneling the request to internet to HTTP and return response to currently browser like VPN but alternative

# Sidecar for remote profiling applications

>[!summary]
>Make a container can distribute and mental monitoring, profiling application with remotely by Sidecar pattern

![[Pasted image 20240502101316.png]]

