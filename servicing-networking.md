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
```

![image](https://github.com/user-attachments/assets/199e67d6-8866-4a9c-a7e1-731645f09d8b)

Solution
Change to the cluster context before attempting the task:

kubectl config use-context cluster2

To expose the web-service via the web-gateway, deploy an HTTPRoute resource with the following configuration:

apiVersion: gateway.networking.k8s.io/v1
kind: HTTPRoute
metadata:
  name: web-route
  namespace: nginx-gateway
spec:
  hostnames:
  - cluster2-controlplane
  parentRefs:
  - name: web-gateway
  rules:
  - backendRefs:
    - name: web-service
      port: 80

---

![image](https://github.com/user-attachments/assets/6ec54201-b64e-4c62-ad43-c050c67e8925)

Solution
Switching to cluster1:



kubectl config use-context cluster1




To create a pod nginx-resolver-cka06-svcn and expose it internally:



student-node ~ ➜ kubectl run nginx-resolver-cka06-svcn --image=nginx 
student-node ~ ➜ kubectl expose pod/nginx-resolver-cka06-svcn --name=nginx-resolver-service-cka06-svcn --port=80 --target-port=80 --type=ClusterIP 




To create a pod test-nslookup. Test that you are able to look up the service and pod names from within the cluster:



student-node ~ ➜  kubectl run test-nslookup --image=busybox:1.28 --rm -it --restart=Never -- nslookup nginx-resolver-service-cka06-svcn
student-node ~ ➜  kubectl run test-nslookup --image=busybox:1.28 --rm -it --restart=Never -- nslookup nginx-resolver-service-cka06-svcn > /root/CKA/nginx.svc.cka06.svcn




Get the IP of the nginx-resolver-cka06-svcn pod and replace the dots(.) with hyphon(-) which will be used below.



student-node ~ ➜  kubectl get pod nginx-resolver-cka06-svcn -o wide
student-node ~ ➜  IP=`kubectl get pod nginx-resolver-cka06-svcn -o wide --no-headers | awk '{print $6}' | tr '.' '-'`
student-node ~ ➜  kubectl run test-nslookup --image=busybox:1.28 --rm -it --restart=Never -- nslookup $IP.default.pod > /root/CKA/nginx.pod.cka06.svcn

---

![image](https://github.com/user-attachments/assets/9ac46d4a-f8bc-44c5-b01c-6cbe3151aadd)

Solution
First change the context to "cluster3":



student-node ~ ➜  kubectl config use-context cluster3
Switched to context "cluster3".




Now apply the ingress resource with the given requirements:



kubectl apply -f - << EOF
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: nginx-ingress-cka04-svcn
  annotations:
    nginx.ingress.kubernetes.io/ssl-redirect: "false"
spec:
  rules:
  - http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: nginx-service-cka04-svcn
            port:
              number: 80
EOF




Check if the ingress resource was successfully created:



student-node ~ ➜  kubectl get ingress
NAME                       CLASS    HOSTS   ADDRESS       PORTS   AGE
nginx-ingress-cka04-svcn   <none>   *       172.25.0.10   80      13s




As the ingress controller is exposed on cluster3-controlplane using traefik service, we need to ssh to cluster3-controlplane first to check if the ingress resource works properly:



student-node ~ ➜  ssh cluster3-controlplane

cluster3-controlplane:~# curl -I 172.25.0.11
HTTP/1.1 200 OK
...



