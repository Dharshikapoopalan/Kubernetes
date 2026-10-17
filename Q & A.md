Kubernetes fundamentals: Questions and answers
Level 1 — Core concepts and architecture
1. What is Kubernetes, and what problem does it solve?

Answer: Kubernetes is an open-source container orchestration platform. 
It automates the deployment, scaling, networking, and management of containerized applications.
Without Kubernetes, an administrator might have to manually restart failed containers, manage application replicas, and distribute workloads across servers. Kubernetes automates many of these tasks.

2. Why do we need Kubernetes when Docker can already run containers?

Answer: Docker can build and run containers, while Kubernetes manages containerized applications across a cluster of machines.
Kubernetes provides capabilities such as:
- Scheduling workloads across nodes.
- Maintaining the desired number of replicas.
- Restarting failed containers through node-level management.
- Service discovery and load distribution.
- Rolling updates and rollbacks.
- Scaling workloads.
Kubernetes does not replace the need for a container runtime or container images.

3. What is a container, and how is it different from a virtual machine?

Answer: A container packages an application and its dependencies into an isolated environment. Containers share the host operating system's kernel.
A virtual machine runs its own guest operating system on virtualized hardware.
Container	Virtual machine
Shares the host kernel	Runs a guest OS kernel
Usually starts quickly	Usually takes longer to boot
Generally uses fewer resources	Generally uses more resources
Isolated using OS-level mechanisms	Isolated using hardware virtualization

4. What is a Kubernetes cluster?

Answer: A Kubernetes cluster is a collection of machines, called nodes, that work together to run containerized applications.
A cluster generally contains:
- A control plane, which manages the cluster.
- Worker nodes, which run application workloads.
The control plane may consist of multiple machines for high availability.

5. What is the difference between a control plane and a worker node?

Answer: The control plane makes cluster-wide management decisions and maintains the desired state. Worker nodes run application Pods.
For example, when you request three replicas of an application, the control plane coordinates scheduling and reconciliation, while worker nodes run the resulting Pods.

6. What are the main components of Kubernetes architecture?

Answer: The main control-plane components are:
- kube-apiserver — exposes the Kubernetes API.
- etcd — stores cluster state.
- kube-scheduler — selects nodes for unscheduled Pods.
- kube-controller-manager — runs controllers that reconcile cluster state.
- cloud-controller-manager — integrates with supported cloud-provider functionality, when used.
Important worker-node components include:
- kubelet — manages Pods assigned to its node.
- Container runtime — runs containers.
- kube-proxy — implements Service networking rules in many clusters, though some network implementations replace it.

7. What is the role of the API server?

Answer: The API server is the main entry point for Kubernetes API operations. Tools such as kubectl, controllers, and other clients communicate with it.
It handles API requests, authentication, authorization, admission control, and persistence of accepted state changes.
It does not directly run application containers.

8. What is etcd, and why is it important?

Answer: etcd is a distributed, consistent key-value store used by Kubernetes to persist cluster data, including configuration and the state of Kubernetes objects.
It is critical because the control plane relies on this data to understand the cluster's recorded state. Production clusters must protect etcd through appropriate availability, access control, and backups.

9. What does the Kubernetes scheduler do?

Answer: The scheduler selects an appropriate node for a Pod that has not yet been assigned to one.
It evaluates factors such as available resources, scheduling constraints, affinity rules, taints, and tolerations. It then records the node assignment through the API.
The scheduler does not normally create or start the containers itself.

10. What is the role of the controller manager?

Answer: The kube-controller-manager runs multiple controllers that continuously compare the desired state with the observed state and attempt to reconcile differences.
For example, a ReplicaSet controller works to maintain the requested number of Pod replicas.

11. What is kubelet, and where does it run?

Answer: The kubelet is an agent that runs on each worker node. It receives Pod specifications through the Kubernetes control plane and works with the container runtime to ensure the assigned containers are running and healthy according to their configuration.
It is not the same as the scheduler: the scheduler chooses a node, while the kubelet manages Pods assigned to that node.

12. What is kube-proxy, and what networking functionality does it provide?

Answer: kube-proxy traditionally configures networking rules on nodes to implement Kubernetes Service behavior, directing traffic toward appropriate backends.
It commonly uses mechanisms such as iptables or IPVS, depending on the implementation and configuration. Some cluster networking solutions implement Service forwarding without kube-proxy.

13. What is a container runtime?

Answer: A container runtime is the software responsible for running containers. Kubernetes communicates with the runtime through the Container Runtime Interface (CRI).
Examples include:
- containerd
- CRI-O
Docker Engine can also be used with Kubernetes through an appropriate CRI integration, such as cri-dockerd. Kubernetes does not require Docker Engine specifically.

14. Can a Kubernetes cluster have multiple control-plane and worker nodes?

Answer: Yes. A cluster can have multiple control-plane nodes to improve availability and multiple worker nodes to distribute workloads.
However, multiple control-plane nodes alone do not guarantee high availability. The API server, etcd, load balancing, and other dependencies must also be configured appropriately.

15. What happens if a worker node fails?

Answer: The control plane detects that the node is no longer reporting health. The node may eventually be marked NotReady, and its Pods may be considered for replacement on other nodes.
The timing depends on health-detection settings and workload configuration. Replacement Pods need suitable capacity and must satisfy scheduling constraints. Stateful workloads may also depend on storage and recovery behavior.
