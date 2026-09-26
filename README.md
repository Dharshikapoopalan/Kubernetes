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


     Service: Helps to initiate communication on top of the IP address. This means the service gets a fixed, permanent IP address; even if the pod is recreated, the service IP won't change.
  
     * Permanent IP address
     * my-app will have its own service, and the DB will have its own service.
     * Lifecycle of Pod and Service are NOT connected. 

   **iii. Ingress**
    
    -> With an internal service, we can communicate with the internal resources, but for external communication we need an external service. To solve this problem, we have **Ingress**

    To access internal services, we have "**Service**," but for external services, or when internal services need to communicate with external services like public access (the application needs to be accessible through a browser, so we need an external service)

  <img width="829" height="469" alt="image" src="https://github.com/user-attachments/assets/7f70b4e6-460d-4337-8b28-eef5cb976bef" />

   **iv. ConfigMap**

    Why Do We Use a ConfigMap?In Kubernetes, a ConfigMap stores application configuration separately from the application code. For example, suppose our application connects to a MongoDB      database through a Kubernetes Service named mongo-db-service. The application uses this service name to communicate with the MongoDB Pod. Now imagine we need to change the database        service name from mongo-db-service → mongo-service without a ConfigMap. If the database service name is hard-coded inside the application, we may need to: Modify the service name in       the application code.

    Rebuild the application.
    
    Build a new container image.
    
    Push the new image to the container registry.
    
    Deploy the new image to Kubernetes.
    
    Restart or recreate the application Pods.

    This creates unnecessary work for a simple configuration change.

    With a ConfigMap, instead of hard-coding the database service name, we can store it in a ConfigMap: DATABASE_HOST=mongo-db-service. The application can read this value through an          environment variable or a mounted configuration file.
    If the database service name changes, we update the ConfigMap: DATABASE_HOST=mongo-service

    This allows us to change the application's configuration without modifying or rebuilding the application code or container image.
    
    Important: Updating a ConfigMap does not always mean the running application automatically receives the new value. If the ConfigMap is consumed as an environment variable, the Pod         normally needs to be restarted/recreated. If it is mounted as a volume, Kubernetes can update the mounted files, but the application must be capable of reloading the configuration.
    
    Simple Definition
    
    ConfigMap separates configuration from application code, allowing us to change configuration values without rebuilding the application image.

  <img width="436" height="193" alt="image" src="https://github.com/user-attachments/assets/678f8e4d-a2e8-4c93-9d7d-e1a2f70be9da" />


   
    

