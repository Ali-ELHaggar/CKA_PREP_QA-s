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

---

![image](https://github.com/user-attachments/assets/4215a22d-f190-4d92-94c5-c4281b251b66)

Solution
Let's check the POD status
kubectl get pod --context=cluster2

So you will see that yello-cka20-trb pod is in Pending state. Let's check out the relevant events.

kubectl get event --field-selector involvedObject.name=yello-cka20-trb --context=cluster2

You will see some errors like:

Warning   FailedScheduling   pod/yello-cka20-trb   0/2 nodes are available: 1 node(s) had untolerated taint {node-role.kubernetes.io/master: }, 1 node(s) had untolerated taint {node: node01}. preemption: 0/2 nodes are available: 2 Preemption is not helpful for scheduling.

Notice this error 1 node(s) had untolerated taint {node: node01} so we can see that one of nodes have taints applied. We don't want to remove the node taints and we are not going to re-create the POD so let's look into the POD config if its using any other toleration settings.

kubectl get pod yello-cka20-trb --context=cluster2 -o yaml

You will notice this in the output

tolerations:
  - effect: NoSchedule
    key: node
    operator: Equal
    value: cluster2-node01

Here notice that the value for key node is cluster2-node01 but the node has different value applied i.e node01 so let's update the taints values for the node as needed.

kubectl --context=cluster2 taint nodes cluster2-node01 node=cluster2-node01:NoSchedule --overwrite=true

Let's check the POD status again

kubectl get pod --context=cluster2

It should be in Running state now.

---
![image](https://github.com/user-attachments/assets/ea826222-3e58-4639-88b0-da74442d3103)

Solution
Find out the name of the DB POD:
kubectl get pod

Check the DB POD logs:
kubectl logs <pod-name>

You might see something like as below which is not that helpful:

Error from server (BadRequest): container "db" in pod "db-deployment-cka05-trb-7457c469b7-zbvx6" is waiting to start: CreateContainerConfigError

So let's look into the kubernetes events for this pod:

kubectl get event --field-selector involvedObject.name=<pod-name>

You will see some errors as below:

Error: couldn't find key db in Secret default/db-cka05-trb

Now let's look into all secrets:

kubectl get secrets db-root-pass-cka05-trb -o yaml
kubectl get secrets db-user-pass-cka05-trb -o yaml
kubectl get secrets db-cka05-trb -o yaml

Now let's look into the deployment.

Edit the deployment
kubectl edit deployment db-deployment-cka05-trb -o yaml

You will notice that some of the keys are different what are reffered in the deployment.

Change some env keys: db to database , db-user to username and db-password to password
Change a secret reference: db-user-cka05-trb to db-user-pass-cka05-trb
Finally save the changes.

---

![image](https://github.com/user-attachments/assets/94d6b78e-92ec-4efa-827d-9753fbc8b288)

Solution
Check current status of the deployment

kubectl get deploy 

Let's check deployment status

kubectl get deploy black-cka25-trb -o yaml

Under status: you will see message: Deployment is paused so seems like deployment was paused, let check the rollout status

kubectl rollout status deployment black-cka25-trb

You will see this message

Waiting for deployment "black-cka25-trb" rollout to finish: 0 out of 1 new replicas have been updated...

So, let's resume

kubectl rollout resume deployment black-cka25-trb

Check again the status of the deployment

kubectl get deploy 

It should be good now.

---

![image](https://github.com/user-attachments/assets/3f10401f-81dd-42a0-937a-61ced7fefab4)

Solution
Check the purple-curl-cka27-trb pod logs

kubectl logs purple-curl-cka27-trb

You will see some logs as below

Not able to connect to the nginx app on http://purple-svc-cka27-trb

Now to debug let's try to access this app from within the purple-app-cka27-trb pod

kubectl exec -it purple-app-cka27-trb -- bash
curl http://purple-svc-cka27-trb
exit

You will notice its stuck, so app is not reachable. Let's look into the service to see its configured correctly.

kubectl edit svc purple-svc-cka27-trb

Under ports: -> port: and targetPort: is set to 8080 but nginx default port is 80 so change 8080 to 80 and save the changes
Let's check the logs now

kubectl logs purple-curl-cka27-trb

You will see Thank you for using nginx. in the output now.

---
