# CKA Mock Exam Questions Repository

This repository aims to be a collaborative resource for individuals preparing for the Certified Kubernetes Administrator (CKA) exam. It contains mock exam questions and answers, organized by the five key domains outlined in the official CKA curriculum.

## Exam Domains and File Structure

The repository is structured to mirror the CKA exam's domain breakdown. Each domain has its own dedicated file containing practice questions and detailed answers.

1.  **Storage (10%)**: `storage.md`
    * Implement storage classes and dynamic volume provisioning
    * Configure volume types, access modes, and reclaim policies
    * Manage persistent volumes and persistent volume claims
2.  **Troubleshooting (30%)**: `troubleshooting.md`
    * Troubleshoot clusters and nodes
    * Troubleshoot cluster components
    * Monitor cluster and application resource usage
    * Manage and evaluate container output streams
    * Troubleshoot services and networking
3.  **Workloads and Scheduling (15%)**: `workloads-scheduling.md`
    * Understand application deployments and how to perform rolling updates and rollbacks
    * Use ConfigMaps and Secrets to configure applications
    * Configure workload autoscaling
    * Understand the primitives used to create robust, self-healing application deployments
    * Configure Pod admission and scheduling (limits, node affinity, etc.)
4.  **Cluster Architecture, Installation, and Configuration (25%)**: `cluster-architecture.md`
    * Manage role-based access control (RBAC)
    * Prepare underlying infrastructure for installing a Kubernetes cluster
    * Create and manage Kubernetes clusters using `kubeadm`
    * Manage the lifecycle of Kubernetes clusters
    * Implement and configure a highly available control plane
    * Use Helm and Kustomize to install cluster components
    * Understand extension interfaces (CNI, CSI, CRI, etc.)
    * Understand CRDs, install, and configure operators
5.  **Servicing and Networking (20%)**: `servicing-networking.md`
    * Understand connectivity between Pods
    * Define and enforce Network Policies
    * Use ClusterIP, NodePort, LoadBalancer service types, and endpoints
    * Use the Gateway API to manage Ingress traffic
    * Know how to use Ingress controllers and Ingress resources
    * Understand and use CoreDNS

## Contributing

This is a collaborative repository, and contributions are highly encouraged! If you'd like to contribute, please follow these guidelines:

1.  **Fork the repository.**
2.  **Create a new branch for your contributions.** (`git checkout -b feature/your-contribution`)
3.  **Add your mock exam questions and answers to the appropriate file.**
    * Ensure your questions are clear, concise, and representative of the CKA exam.
    * Provide detailed and accurate answers.
    * If possible include example commands, and yaml files.
4.  **Format your contributions using Markdown.**
5.  **Commit your changes.** (`git commit -am 'Add new mock exam questions'`)
6.  **Push to the branch.** (`git push origin feature/your-contribution`)
7.  **Create a pull request (PR) to the `main` branch.**
    * Provide a clear and descriptive title and description for your PR.
    * Explain the changes you've made.

### Contribution Guidelines

* **Accuracy:** Ensure all questions and answers are accurate and up-to-date with the latest Kubernetes version and CKA curriculum.
* **Clarity:** Write clear and concise questions and answers.
* **Completeness:** Provide thorough explanations and examples.
* **Formatting:** Follow the existing Markdown formatting in the files.
* **No Duplicates:** Before adding new questions, ensure they don't already exist in the repository.
* **Respectful Contributions:** Keep contributions relevant to the CKA exam.

## How to Use This Repository

1.  **Clone the repository to your local machine.**
2.  **Navigate to the desired domain file.**
3.  **Attempt the mock exam questions.**
4.  **Compare your answers with the provided solutions.**
5.  **Use this repository as a study aid to identify areas where you need to improve.**

## License

This repository is licensed under the [MIT License](LICENSE). Feel free to use and distribute the content as needed.

## Disclaimer

These mock exam questions are intended for educational purposes only and are not a substitute for the official CKA exam. While they are designed to be representative of the exam, they do not guarantee success on the actual CKA exam.
