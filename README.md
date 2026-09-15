# Kubernetes
All about Kubernetes

**Introduction to K8s**
------------------------------------------------
What is Kubernetes?

K8s Architecture

Main K8s components 

Minikube and kubectl-local setup

Main kubectl commands-K8s CLI

K8s YAML configuration file

Hands-on Demo

------------------------------------------------
**Advanced Concepts**
------------------------------------------------
K8s Namespaces-organize your components

K8s Ingress

Helm package manager

Volumes-Persisting data in K8s 

K8s StatefulSet-Deploying Stateful Apps

K8s Services

------------------------------------------------

**1. What is Kubernetes?**

  **Official Definition:** An open-source container orchestration tool developed by Google that helps you manage containerized applications in different deployment environments(Physical, Virtual, Cloud, Hybrid).
  
**2. What problems does Kubernetes solve?**
  
  The need for a container orchestration tool:
  
    i.   Trend from Monolith to Microservices
    ii.  Increased usage of containers
    iii. Demand for a proper way of managing those hundreds of containers. 
    
**3. What features do orchestration tools offer?**

    i.   High Availability or no downtime
    ii.  Scalability or high performance
    iii. Disaster recovery - backup & restore
    
**4. Kubernetes Components**

    i.    Pod, Node
    
    ii.   Service
    
    iii.  Ingress
    
    iv.   ConfigMap
    
    v.    Secrets
    
    vi.   StatefulSet
    
    vii.  Deployment
    
    viii. Volumes
   
   **i. Pod, Node**

<img width="876" height="525" alt="image" src="https://github.com/user-attachments/assets/b9331fee-7e92-4078-8b27-00ae314a9c96" />

     Pod: Smallest unit of K8s
   
       An abstraction over a container
       Pod creates a running container environment to make the containers run on top of it.
       We only interact with the Kubernetes layer.
       Usually 1 application per pod; we can run many containers in a pod, but mainly it needs to be 1 pod.
       Each pod gets its own IP address (internal IP address)
   
     Node: A simple server, physical or virtual machine(an EC2 instance can be considered a node)

       * Pods are ephemeral(They can destroy easily)
       * A new IP address will be assigned whenever a pod is re-created

   -> Every time a pod is killed or crashes and is re-created, it gets a new IP, so for internal communication we need to update the IP address in the code as well. To solve this problem, we have **Service**

   **ii. Service**
   
 <img width="858" height="522" alt="image" src="https://github.com/user-attachments/assets/24469c35-5049-4d2a-a8c1-a1ef9d1474be" />


    Service: Helps to initiate communication on top of the IP address. 

     * Permanent IP address
     * my-app will have its own service, and the DB will have its own service.
   
    

