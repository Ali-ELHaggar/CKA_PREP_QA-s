# Workloads and Scheduling Mock Exam Questions

This file contains mock exam questions and answers related to the Workloads and Scheduling domain of the Certified Kubernetes Administrator (CKA) exam.

**Key Topics Covered:**

* **Understand application deployments and how to perform rolling updates and rollbacks:** Proficiency in using Deployments for managing application lifecycles, including rolling updates and rollbacks.
* **Use ConfigMaps and Secrets to configure applications:** Understanding how to use ConfigMaps and Secrets to decouple configuration from application code.
* **Configure workload autoscaling:** Implementing Horizontal Pod Autoscaling (HPA) to automatically scale applications based on resource utilization.
* **Understand the primitives used to create robust, self-healing application deployments:** Familiarity with Pods, ReplicaSets, and other Kubernetes primitives.
* **Configure Pod admission and scheduling (limits, node affinity, etc.):** Understanding how to use resource limits, node affinity, taints, and tolerations to control Pod scheduling.


![image](https://github.com/user-attachments/assets/6be20848-6509-4960-bd7b-9a35d1c8fa13)

Solution
Run the command to change the context: -

kubectl config use-context cluster1



Check the status of the pod: -

kubectl get pods -n dev-wl07



One of the pods is in an error state. As a quick fix, we need to rollback to the previous revision as follows: -

kubectl rollout undo -n dev-wl07 deploy webapp-wl07



After successful rolling back, inspect the updated image: -

kubectl describe deploy -n dev-wl07 webapp-wl07 | grep -i image



On the Controlplane node, save the image name to the given path /root/rolling-back-record.txt: -

ssh cluster1-controlplane

echo "kodekloud/webapp-color" > /root/rolling-back-record.txt



And increase the replica count to the 5 with help of kubectl scale command: -

kubectl scale deploy -n dev-wl07 webapp-wl07 --replicas=5



Verify it by running the command: kubectl get deploy -n dev-wl07

----

![image](https://github.com/user-attachments/assets/4a17cbad-edf3-4174-96b3-3cd418ba3e8d)

Solution
Set the correct context: -

kubectl config use-context cluster1



Use the following template to create a deployment called ocean-tv-wl09: -

---
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: ocean-tv-wl09
  name: ocean-tv-wl09
spec:
  replicas: 3
  selector:
    matchLabels:
      app: ocean-tv-wl09
  strategy: 
   type: RollingUpdate
   rollingUpdate:
     maxUnavailable: 40%
     maxSurge: 55%
  template:
    metadata:
      labels:
        app: ocean-tv-wl09
    spec:
      containers:
      - image: kodekloud/webapp-color:v1
        name: webapp-color



Now, create the deployment by using the kubectl create -f command in the default namespace: -

kubectl create -f <FILE-NAME>.yaml



After sometime, upgrade the deployment image to kodekloud/webapp-color:v2: -

kubectl set image deploy ocean-tv-wl09 webapp-color=kodekloud/webapp-color:v2



And check out the rollout history of the deployment ocean-tv-wl09: -

kubectl rollout history deploy ocean-tv-wl09
deployment.apps/ocean-tv-wl09 
REVISION  CHANGE-CAUSE
1         <none>
2         <none>



NOTE: - Revision count is 2. In your lab, it could be different.



On the student-node, store the revision count to the given file: -

echo "2" > /opt/revision-count.txt



In final task, rollback the deployment image to an old version: -

kubectl rollout undo deployment ocean-tv-wl09



Verify the image name by using the following command: -

kubectl describe deploy ocean-tv-wl09



It should be kodekloud/webapp-color:v1 image.
