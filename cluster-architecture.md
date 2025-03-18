# Cluster Architecture, Installation, and Configuration Mock Exam Questions

This file contains mock exam questions and answers related to the Cluster Architecture, Installation, and Configuration domain of the Certified Kubernetes Administrator (CKA) exam.

**Key Topics Covered:**

* **Manage role-based access control (RBAC):** Implementing and managing RBAC to control access to Kubernetes resources.
* **Prepare underlying infrastructure for installing a Kubernetes cluster:** Understanding the prerequisites and steps involved in setting up the underlying infrastructure.
* **Create and manage Kubernetes clusters using `kubeadm`:** Proficiency in using `kubeadm` to install and manage Kubernetes clusters.
* **Manage the lifecycle of Kubernetes clusters:** Understanding how to upgrade, scale, and maintain Kubernetes clusters.
* **Implement and configure a highly available control plane:** Setting up a highly available control plane for fault tolerance.
* **Use Helm and Kustomize to install cluster components:** Using Helm and Kustomize for package management and configuration management.
* **Understand extension interfaces (CNI, CSI, CRI, etc.):** Familiarity with Container Network Interface (CNI), Container Storage Interface (CSI), and Container Runtime Interface (CRI).
* **Understand CRDs, install, and configure operators:** Understanding Custom Resource Definitions (CRDs) and operators for extending Kubernetes functionality.

---

## Question 1

![image](https://github.com/user-attachments/assets/2a9168b7-8145-4d32-a7ef-a8de20db8ca1)

**Weight:** 2

**SECTION:** ARCHITECTURE, INSTALLATION AND MAINTENANCE

For this question, please set the context to `cluster3` by running:

```bash
kubectl config use-context cluster3

Create the pod:

kubectl run alpine-sleeper-cka15-arch --image=alpine --command -- sleep 7200

Verify the pod:

kubectl get pod alpine-sleeper-cka15-arch


---

## Question 2

![image](https://github.com/user-attachments/assets/caded04e-e297-4605-840b-c5d3b562904d)

**Weight:** 8

**SECTION:** ARCHITECTURE, INSTALLATION AND MAINTENANCE

For this question, please set the context to `cluster1` by running:

```bash
kubectl config use-context cluster1

Solution:

Edit the ClusterRole:
kubectl edit clusterrole green-role-cka22-arch

Modify the rules section to allow only get on namespaces:

rules:
- apiGroups: [""]
  resources: ["namespaces"]
  verbs: ["get"]

Verify the ClusterRole:

kubectl get clusterrole green-role-cka22-arch -o yaml

Ensure the rules match the updated configuration.

