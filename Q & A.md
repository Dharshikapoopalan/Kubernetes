Kubernetes fundamentals: Questions and answers

**Level 1 — Core concepts and architecture**

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

********************************************************************************************************************

**Level 2 — Understand the architecture**

1. Why do components such as the scheduler communicate with the API server?

Answer: The API server provides the central interface for reading and changing Kubernetes objects. Components use it to observe cluster state and record their decisions.
For example, the scheduler watches for unscheduled Pods and records the selected node assignment through the API.

2. Does the scheduler directly create containers?

Answer: No. The scheduler selects a node for a Pod. The kubelet on that node works with the container runtime to start the containers specified in the Pod.

3. Where is the desired state of the cluster stored?

Answer: Kubernetes object state is persisted in etcd through the API server. The desired state might specify that a Deployment should maintain three replicas.
Controllers compare this desired state with the observed state and attempt to reconcile differences.

4. How does a worker node learn that a new Pod needs to run?

Answer: After the scheduler assigns the Pod to a node, the kubelet on that node learns about the assigned Pod through the Kubernetes API, normally by watching for changes. It then works with the container runtime to start the required containers.

5. Does the API server run containers itself?

Answer: No. The API server accepts and processes API requests. The scheduler selects nodes, and the kubelet and container runtime on the selected nodes manage container execution.

6. What happens if the scheduler is temporarily unavailable?

Answer: Already-running Pods generally continue running because the scheduler isn't responsible for continuously running them. However, new Pods that require scheduling may remain Pending until scheduling resumes.
Other control-plane functions can continue if their required components remain healthy.

7. What happens if etcd becomes unavailable?

Answer: Kubernetes can experience serious control-plane problems because it relies on etcd for persisted cluster state. API operations that require reading or writing that state may fail, and controllers may be unable to reconcile changes.
Already-running containers do not necessarily stop immediately, but management and recovery capabilities can be impaired.

8. Why is kubelet important to a worker node?

Answer: The kubelet manages the Pods assigned to its node. It communicates with the container runtime, checks container and Pod health, and carries out configured lifecycle and probe actions.
If kubelet fails, existing containers may continue running for a while, but Kubernetes loses normal management and health-reporting capabilities for that node.

9. What is the difference between a controller and the controller manager?

Answer: A controller is a reconciliation process responsible for a particular task. The controller manager runs several controllers.
For example, the Deployment and ReplicaSet controllers manage different aspects of maintaining application replicas and updating workloads.

10. Which components maintain the desired state?

Answer: Kubernetes controllers are primarily responsible for reconciliation. The API server and etcd provide the API and persisted state that those controllers use. The scheduler handles node selection for unscheduled Pods, and kubelets work to ensure assigned Pods run on their nodes.

**********************************************************************************************************************

**Level 3 — Kubernetes objects and workloads**

1. What is a Pod?

Answer: A Pod is the smallest deployable unit in Kubernetes. It contains one or more containers that share certain resources, including the Pod's network namespace and, when configured, shared volumes.
Most application Pods contain one main application container, although multi-container Pods are useful for tightly coupled components.

2. Why is a Pod the smallest deployable unit rather than a container?

Answer: Kubernetes manages containers through Pods because a Pod provides a shared execution environment for one or more closely related containers. It allows them to share networking, storage, and a common lifecycle.
A container is the execution unit, but the Pod is the Kubernetes workload unit.

3. Can a Pod contain multiple containers? Why?

Answer: Yes. Multiple containers can run inside one Pod when they need to work closely together and share resources.
Examples include:
- An application container and a log-processing sidecar.
- An application container and a proxy sidecar.
- An application container and a helper container that shares a volume.
Unrelated applications should generally run in separate Pods.

4. What is the difference between a Pod and a Deployment?

Answer: A Pod runs containers. A Deployment manages a desired number of interchangeable Pod replicas through ReplicaSets and supports declarative updates and rollbacks.
For a stateless web application, you would generally create a Deployment rather than manually manage individual Pods.

5. Why do we use a ReplicaSet?

Answer: A ReplicaSet ensures that the desired number of matching Pods exists.
For example, if the specification requires three replicas and one Pod disappears, the ReplicaSet controller attempts to create a replacement. Deployments normally manage ReplicaSets for you.

6. What happens if a Pod is deleted manually?

Answer: The result depends on how the Pod was created.
- If managed by a Deployment, its ReplicaSet will generally create a replacement.
- If managed by a StatefulSet, the controller will work to restore the expected Pod identity.
- If created directly without a managing controller, it will not normally be recreated automatically.

7. What is the difference between a Deployment and a StatefulSet?

Answer: A Deployment is generally used for stateless workloads where replicas are interchangeable. A StatefulSet is designed for applications that require stable Pod identities and may require stable storage associations and ordered operations.
Examples include web front ends for Deployments and certain databases or distributed systems for StatefulSets.

8. When would you use a DaemonSet?

Answer: A DaemonSet ensures that a Pod runs on each eligible node, or on each node selected by its scheduling rules.
Common uses include:
- Node-level monitoring agents.
- Log collection agents.
- Certain networking or storage components.

9. What is a Kubernetes Job, and how is it different from a Deployment?

Answer: A Job manages Pods that perform a task until it completes successfully according to the Job's configuration.
A Deployment manages continuously running application replicas.
Examples: a database migration or batch-processing task can use a Job, while a web server typically uses a Deployment.

10. What is the difference between declarative and imperative approaches?

Answer: An imperative approach specifies an action to perform. A declarative approach specifies the desired outcome.
Example of imperative configuration:
kubectl scale deployment web --replicas=3


Example of declarative configuration:
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web
spec:
  replicas: 3


The YAML is saved to a file and applied using kubectl apply -f web.yaml. Kubernetes controllers then work toward the specified state.

11. What is a YAML manifest?

Answer: A YAML manifest is a configuration document describing a Kubernetes object and its desired configuration.
It commonly specifies the object's API version, kind, metadata, and specification. Kubernetes validates the request, and controllers act on accepted objects as needed.

12. What are apiVersion, kind, metadata, and spec?

Answer:
- apiVersion: Identifies the API group and version used for the object.
- kind: Identifies the resource type, such as Pod or Deployment.
- metadata: Contains identifying information such as name, namespace, labels, and annotations.
- spec: Describes the desired configuration of the resource.
Some Kubernetes objects also have a status section describing their observed state.

13. What is desired state, and how does Kubernetes maintain it?

Answer: Desired state is what you want Kubernetes to achieve, such as running three replicas of an application.
Controllers continuously compare the desired state with the observed state and attempt to reconcile any differences.
This reconciliation process is one of the central principles of Kubernetes.

14. What is the difference between a label and an annotation?

Answer: Both are metadata attached to Kubernetes objects, but they serve different purposes.
- Labels identify and group objects. Selectors can use labels to find matching objects.
- Annotations store additional information, such as descriptions, build details, or tool-specific configuration.
Annotations are not normally used as label selectors.

15. What is a namespace, and why do we use it?

Answer: A namespace logically separates resources within a Kubernetes cluster.
For example, a company might use development, testing, and production namespaces to organize workloads.
Namespaces also help scope names, access controls, quotas, and certain policies. They are not, by themselves, a strong security boundary.
