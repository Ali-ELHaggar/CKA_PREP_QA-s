# Storage Mock Exam Questions

This file contains mock exam questions and answers related to the Storage domain of the Certified Kubernetes Administrator (CKA) exam.

**Key Topics Covered:**

* **Implement storage classes and dynamic volume provisioning:** Understanding how to create and utilize StorageClasses for dynamic provisioning of PersistentVolumes.
* **Configure volume types, access modes, and reclaim policies:** Familiarity with different volume types (e.g., hostPath, NFS, cloud provider volumes), access modes (ReadWriteOnce, ReadOnlyMany, ReadWriteMany), and reclaim policies (Retain, Delete).
* **Manage persistent volumes and persistent volume claims:** Proficiency in creating, managing, and troubleshooting PersistentVolumes (PVs) and PersistentVolumeClaims (PVCs).

---

## Question 1

![image](https://github.com/user-attachments/assets/f995033f-4aa3-477f-baa3-c4c291190328)

**Weight:** 8

**SECTION:** STORAGE

For this question, please set the context to `cluster1` by running:

```bash
kubectl config use-context cluster1

A persistent volume called papaya-pv-cka09-str is already created with a storage capacity of 150Mi. It's using the papaya-stc-cka09-str storage class with the path /opt/papaya-stc-cka09-str.

Also, a persistent volume claim named papaya-pvc-cka09-str has also been created on this cluster. This PVC has requested 50Mi of storage from papaya-pv-cka09-str volume.

Resize the PVC to 80Mi and make sure the PVC is in Bound state.

Solution:

Edit the PVC:

Bash

kubectl edit pvc papaya-pvc-cka09-str
Change the resources.requests.storage field to 80Mi.

YAML

spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 80Mi # Modified to 80Mi
  storageClassName: papaya-stc-cka09-str
  volumeMode: Filesystem
  volumeName: papaya-pv-cka09-str
Verify the PVC:

Bash

kubectl describe pvc papaya-pvc-cka09-str
Ensure that the Capacity and Status are updated to 80Mi and Bound respectively.
