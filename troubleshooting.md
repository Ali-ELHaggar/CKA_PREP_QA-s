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
