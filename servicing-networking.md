# Servicing and Networking Mock Exam Questions

This file contains mock exam questions and answers related to the Servicing and Networking domain of the Certified Kubernetes Administrator (CKA) exam.

**Key Topics Covered:**

* **Understand connectivity between Pods:** Understanding how Pods communicate with each other within a Kubernetes cluster.
* **Define and enforce Network Policies:** Implementing Network Policies to control network traffic between Pods.
* **Use ClusterIP, NodePort, LoadBalancer service types, and endpoints:** Proficiency in using different Service types to expose applications.
* **Use the Gateway API to manage Ingress traffic:** Understanding the Gateway API and how it differs from traditional Ingress resources.
* **Know how to use Ingress controllers and Ingress resources:** Configuring and managing Ingress controllers and Ingress resources for external access.
* **Understand and use CoreDNS:** Understanding how CoreDNS works and how to configure it for service discovery.

## Question 1

![image](https://github.com/user-attachments/assets/28cba20f-4ce9-455e-9567-4408d7c91a1e)

**Weight:** 6

**SECTION:** SERVICE NETWORKING

For this question, please set the context to `cluster1` by running:

```bash
kubectl config use-context cluster1

Create the HPA:
kubectl autoscale deployment frontend-deployment --cpu-percent=70 --min=2 --max=10
Verify the HPA:
kubectl get hpa frontend-deployment -o yaml
Check the output YAML to ensure:

scaleTargetRef.name is frontend-deployment.
targetCPUUtilizationPercentage is 70.
minReplicas is 2.
maxReplicas is 10.
