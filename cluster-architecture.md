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
```
----

![image](https://github.com/user-attachments/assets/f20dbc49-fb41-4f38-ac54-fd536bc4e879)

Solution
Run the following command to change the context: -

kubectl config use-context cluster2

In this task, we will use the helm commands. Here are the steps:

First, check the given namespace; if it doesn't exist, we must create it first; otherwise, it will give an error "namespaces not found" while installing the helm chart.
To check all the namespaces in the cluster2, we would have to run the following command: -

kubectl get ns

It will list all the namespaces. If the given namespace doesn't exist, then run the following command: -

kubectl create ns frontend-apd

Now, on the student-node node and go to the /opt/ directory. We have given the helm chart directory webapp-color-apd that contains templates, values files, and the chart file etc.

Update the values according to the given specifications as follows: -

a.) Update the value of the appVersion to 1.20.0 in the Chart.yaml file.

b.) Update the value of the replicaCount to 3 in the values.yaml file.

c.) Update the value of the type to NodePort in the values.yaml file.

These are the values we have to update.

Now, we will use the helm lint command to check the Helm chart because it can identify errors such as missing or misconfigured values, invalid YAML syntax, and deprecated APIs etc.

cd /opt/

helm lint ./webapp-color-apd/



If there is no misconfiguration, we will see the similar output: -

helm lint ./webapp-color-apd/
==> Linting ./webapp-color-apd/
[INFO] Chart.yaml: icon is recommended

1 chart(s) linted, 0 chart(s) failed



But in our case, there are some issues with the given templates.


Deployment apiVersion needs to be correctly written. It should be apiVersion: apps/v1.

In the service YAML, there is a typo in the template variable {{ .Values.service.name }} because of that, it's not able to reference the value of the name field defined in the values.yaml file for the Kubernetes service that is being created or updated.


Now run the following command to install the helm chart in the frontend-apd namespace: -

helm install webapp-color-apd -n frontend-apd ./webapp-color-apd



Use the helm ls command to list the release deployed using helm.

helm ls -n frontend-apd


---

![image](https://github.com/user-attachments/assets/7e553383-69a5-4949-a356-bf372b5489f7)

olution
Create the service account, cluster role and role binding:

student-node ~ ➜  kubectl --context cluster1 create serviceaccount deploy-cka20-arch
student-node ~ ➜  kubectl --context cluster1 create clusterrole deploy-role-cka20-arch --resource=deployments --verb=get
student-node ~ ➜  kubectl --context cluster1 create clusterrolebinding deploy-role-binding-cka20-arch --clusterrole=deploy-role-cka20-arch --serviceaccount=default:deploy-cka20-arch




You can verify it as below:

student-node ~ ➜  kubectl --context cluster1 auth can-i get deployments --as=system:serviceaccount:default:deploy-cka20-arch
yes

---

![image](https://github.com/user-attachments/assets/aef5e3e8-7864-489b-bd0e-fa0711b0ea60)

Solution
Update pod-cka26-arch.sh script:

student-node ~ ➜ vi pod-cka26-arch.sh




Add below command in it:

kubectl --context cluster1 get pod -n kube-system kube-apiserver-cluster1-controlplane  -o jsonpath='{.metadata.labels.component}'



