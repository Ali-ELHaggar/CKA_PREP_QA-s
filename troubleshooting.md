# Troubleshooting Mock Exam Questions

This file contains mock exam questions and answers related to the Troubleshooting domain of the Certified Kubernetes Administrator (CKA) exam.

**Key Topics Covered:**

* **Troubleshoot clusters and nodes:** Identifying and resolving issues related to cluster components (etcd, control plane, worker nodes).
* **Troubleshoot cluster components:** Ability to diagnose and fix problems with core Kubernetes components.
* **Monitor cluster and application resource usage:** Understanding how to monitor resource utilization (CPU, memory, storage) using tools like `kubectl top` and metrics servers.
* **Manage and evaluate container output streams:** Working with container logs using `kubectl logs` and understanding how to debug application issues from logs.
* **Troubleshoot services and networking:** Diagnosing and resolving networking issues, including service discovery, DNS resolution, and network policies.


## Question 1

**Weight:** 4

**SECTION:** TROUBLESHOOTING

For this question, please set the context to `cluster1` by running:

```bash
kubectl config use-context cluster1

Inspect the CronJob:

```bash

kubectl describe cronjob orange-cron-cka10-trb
Check the Schedule field and the command used in the job.

Edit the CronJob:

```bash
kubectl edit cronjob orange-cron-cka10-trb
Modify the schedule field to */2 * * * * and change the command to curl the service orange-svc-cka10-trb.

Verify the CronJob:

```bash

kubectl describe cronjob orange-cron-cka10-trb
Ensure the Schedule and container args are correct.

Verify the Pods Created by the CronJob:

```bash

kubectl get pods -l job-name=orange-cron-cka10-trb -o wide
```

---
## Question 2

![image](https://github.com/user-attachments/assets/03cb7423-b52f-471a-bbd8-5ff7c93a9ea3)

**Weight:** 6

**SECTION:** TROUBLESHOOTING

For this question, please set the context to `cluster4` by running:

```bash
kubectl config use-context cluster4
```

The kube-controller-manager pod on cluster4-controlplane is in a CrashLoopBackOff state. Investigate and resolve the issue.

Solution:

Describe the Pod:

Bash

kubectl describe pod kube-controller-manager-cluster4-controlplane -n kube-system
Examine the Events and Last State sections for error messages.

Based on the provided output, the error is:

Error: failed to create containerd task: failed to create shim task: OCI runtime create failed: runc create failed: unable to start container process: exec: "kube-controller-manage": executable file not found in $PATH: unknown
This indicates that the kube-controller-manager container is unable to find the kube-controller-manage executable.

Inspect the Pod's Configuration:

Look at the Command section in the describe pod output:

Command:
  kube-controller-manage
  --allocate-node-cidrs=true
  ...
The command should be kube-controller-manager, not kube-controller-manage. This is the root cause of the error.

Edit the Pod Manifest (or Static Pod Manifest):

Since kube-controller-manager is a static pod, you need to edit the static pod manifest on the node. The manifest is typically located in /etc/kubernetes/manifests/.

Bash

ssh cluster4-controlplane
sudo vi /etc/kubernetes/manifests/kube-controller-manager.yaml
Locate the command section and correct the command:

YAML

command:
- kube-controller-manager
- --allocate-node-cidrs=true
...
Verify the Pod:

After saving the changes, Kubernetes will automatically restart the pod.

Bash

kubectl get pod kube-controller-manager-cluster4-controlplane -n kube-system
Ensure the pod is in the Running state and the Restart Count stops increasing.

Check the Logs (Optional):

Bash

kubectl logs kube-controller-manager-cluster4-controlplane -n kube-system
Verify that the controller manager is running without errors.


