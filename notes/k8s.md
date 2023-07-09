Study:
- https://kubernetes.io/docs/tutorials/kubernetes-basics/
- https://www.udemy.com/course/learn-kubernetes/
- https://learning.edx.org/course/course-v1:LinuxFoundationX+LFS158x+1T2022/home
- https://kubernetes.io/docs/tutorials
- https://www.educative.io/courses/practical-guide-to-kubernetes


Course Research
- https://hackr.io/blog/best-kubernetes-course
- https://dev.to/javinpaul/top-10-courses-to-learn-docker-and-kubernetes-for-programmers-4lg0
- https://stackoverflow.com/questions/59042659/where-i-can-learn-the-kubernetes-with-hands-on


# Monolith Refactoring

"Big-bang" approach or an incremental refactoring.

A so-called "Big-bang" approach focuses all efforts with the refactoring of the monolith, postponing the development and implementation of any new features - essentially delaying progress and possibly, in the process, even breaking the core of the business, the monolith.

An incremental refactoring approach guarantees that new features are developed and implemented as modern microservices which are able to communicate with the monolith through APIs, without appending to the monolith's code. In the meantime, features are refactored out of the monolith which slowly fades away while all, or most its functionality is modernized into microservices.


Applications tightly coupled with data stores are also poor candidates for refactoring.

# Container and orchestration
Container images allow us to confine the application code, its runtime, and all of its dependencies in a pre-defined format.

Benefits of container orchestration:
Fault-tolerance
On-demand scalability
Optimal resource usage
Auto-discovery to automatically discover and communicate with each other
Accessibility from the outside world
Seamless updates/rollbacks without any downtime.

# Kubernetes

# Minikube

- `minikube start --nodes 2 -p multinode-demo --driver=docker --alsologtostderr`
    - https://medium.com/@seohee.sophie.kwon/how-to-run-a-minikube-on-apple-silicon-m1-8373c248d669
    - https://minikube.sigs.k8s.io/docs/tutorials/multi_node/
    - https://minikube.sigs.k8s.io/docs/start/
    - More options
        - `minikube start --kubernetes-version=v1.23.3 --driver=podman --profile minipod`
        - `minikube start --driver=virtualbox --nodes=3 --disk-size=10g --cpus=2 --memory=4g --kubernetes-version=v1.25.1 --cni=calico --container-runtime=cri-o -p multivbox`
- minikube profile
    - The minikube start command generates a default minikube cluster with the specifications described above and it will store these specs so that we can restart the default cluster whenever desired. The object that stores the specifications of our cluster is called a profile.
    - minikube start command allows us to create such custom profiles with the `--profile or -p` flags
    - `minikube profile list` to list all the custom profiles
    - The active marker indicates the target cluster profile of the minikube command line tool. To change it use `minikube profile <profile name>`
    - Most minikube commands, such as start, stop, node, etc. are profile aware, meaning that the user is required to specify the target cluster of the command, through its profile name.
- `minikube stop -p minibox`
- `minikube start -p minibox`
- `minikube version`
- `minikube node list -p minibox`
- `minikube ip` / `minikube -p minibox ip` / `minikube -p minibox ip -n minibox-m02`
- `minikube delete -p minibox`
- minikube auto-completion - ??
- `minikube status`
- kubectl
    - A Minikube installation has its own kubectl CLI installed and ready to use. However, it is somewhat inconvenient to use as the kubectl command becomes a subcommand of the minikube command. Users would be required to type longer commands, such as `minikube kubectl -- <subcommand>`
    - While a simple solution would be to set up an alias, the recommendation is to run the kubectl CLI tool as a standalone installation.
    - Once separately installed, kubectl receives its configuration automatically for Minikube Kubernetes cluster access.
    - Installation
        - command??
        - bash-completion??
        - configuration??
    - Config
        - To access the Kubernetes cluster, the kubectl client needs the control plane node endpoint and appropriate credentials to be able to securely interact with the API Server running on the control plane node.
        - Minikube, the startup process creates, by default, a configuration file, config, inside the `.kube` directory (often referred to as the `kubeconfig`), which resides in the user's home directory. The configuration file has all the connection details required by kubectl.
        - Multiple `kubeconfig` files can be configured with a single `kubectl` client
        - `kubectl config view`
        - Although for the Kubernetes cluster installed by Minikube the ~/.kube/config file gets created automatically, this is not the case for Kubernetes clusters installed by other tools. In other cases, the config file has to be created manually and sometimes re-configured to suit various networking and client/server setups.
    - Cluster info
        - Once kubectl is installed, we can display information about the Minikube Kubernetes cluster with
        - `kubectl cluster-info`
- Addons
    - minikube addons list
    - minikube addons enable metrics-server
    - minikube addons enable dashboard
    - minikube addons list
    - minikube dashboard
    - minikube dashboard --url
- Proxy
    - `kubectl proxy` authenticates with the API server on the control plane node and makes services available on the default proxy port 8001.
- APIs with Authentication
    - When not using the `kubectl proxy`, we need to authenticate to the API Server when sending API requests. We can authenticate by providing a `Bearer Token` when issuing a curl command, or by providing a set of `keys` and `certificates`.
    - https://learning.edx.org/course/course-v1:LinuxFoundationX+LFS158x+1T2022/block-v1:LinuxFoundationX+LFS158x+1T2022+type@sequential+block@7b044c7dafae45a7a3c4773195b8f11b/block-v1:LinuxFoundationX+LFS158x+1T2022+type@vertical+block@49866078f2a54a208b57ce783b59ec2e

# Kubernetes Building Blocks

- Kubernetes became popular due to its advanced application lifecycle management capabilities, implemented through a rich object model, representing different persistent entities in the Kubernetes cluster.
    - With each object, we declare our intent, or the desired state of the object, in the spec section.
    - The Kubernetes system manages the status section for objects, where it records the actual state of the object.
    - At any given point in time, the Kubernetes Control Plane tries to match the object's actual state to the object's desired state.
    - An object definition manifest must include other fields that specify the version of the API we are referencing as the apiVersion, the object type as kind, and additional data helpful to the cluster or users for accounting purposes - the metadata.
- Node
    - Nodes are virtual identities assigned by Kubernetes to the systems part of the cluster - whether Virtual Machines, bare-metal, Containers, etc.
    - Each node is managed with the help of two Kubernetes node agents - kubelet and kube-proxy, while it also hosts a container runtime.
        - The container runtime is required to run all containerized workload on the node - control plane agents and user workloads.
        - The kubelet and kube-proxy node agents are responsible for executing all local workload management related tasks - interact with the runtime to run containers, monitor containers and node health, report any issues and node state to the API Server, and managing network traffic to containers.
    - Based on their pre-determined functions, there are two distinct types of nodes - control plane and worker.
        - A typical Kubernetes cluster includes at least one control plane node, but it may include multiple control plane nodes for Highly Available (HA) control plane.
        - There are cases when a single all-in-one cluster is bootstrapped as a single node on a single VM, bare-metal, or Container, when high availability and resource redundancy are not of importance.
        - Features specific to multi-node clusters, such as DaemonSets, multi node networking, etc.
    - Node identities are created and assigned during the cluster bootstrapping process by the tool responsible to initialize the cluster agents. Minikube is using the default kubeadm bootstrapping tool, to initialize the control plane node during the init phase and grow the cluster by adding worker or control plane nodes with the join phase.
    - The control plane nodes run the control plane agents, such as the API Server, Scheduler, Controller Managers, and etcd in addition to the kubelet and kube-proxy node agents, the container runtime, and addons for container networking, monitoring, logging, DNS, etc.
    - Worker nodes run the kubelet and kube-proxy node agents, the container runtime, and addons for container networking, monitoring, logging, DNS, etc.
- Namespaces
    - If multiple users and teams use the same Kubernetes cluster we can partition the cluster into virtual sub-clusters using Namespaces. The names of the resources/objects created inside a Namespace are unique, but not across Namespaces in the cluster.
    - `kubectl get namespaces`
    - Kubernetes creates four Namespaces out of the box:
        - kube-system: contains the objects created by the Kubernetes system, mostly the control plane agents
        - kube-public: its a special Namespace, which is unsecured and readable by anyone, used for special purposes such as exposing public (non-sensitive) information about the cluster.
        - kube-node-lease: holds node lease objects used for node heartbeat data
        - default: contains the objects and resources created by administrators and developers, and objects are assigned to it by default unless another Namespace name is provided by the user
    - Good practice, however, is to create additional Namespaces, as desired, to virtualize the cluster and isolate users, developer teams, applications, or tiers:
    - `kubectl create namespace new-namespace-name`
    - `Resource quotas` help users limit the overall resources consumed within Namespaces, while `LimitRanges` help limit the resources consumed by Containers and their enclosing objects in a Namespace
- Pods
    - A Pod is the smallest Kubernetes workload object.
    - It is the unit of deployment in Kubernetes, which represents a single instance of the application.
    - A Pod is a logical collection of one or more containers, enclosing and isolating them to ensure that they:
        - Are scheduled together on the same host with the Pod.
        - Share the same network namespace, meaning that they share a single IP address originally assigned to the Pod.
        - Have access to mount the same external storage (volumes) and other common dependencies.
    - Pods are ephemeral in nature, and they do not have the capability to self-heal themselves. That is the reason they are used with controllers, or operators (controllers/operators are used interchangeably), which handle Pods' replication, fault tolerance, self-healing, etc.
    - Examples of controllers are Deployments, ReplicaSets, DaemonSets, Jobs, etc. When an operator is used to manage an application, the Pod's specification is nested in the controller's definition using the Pod Template.
    - The contents of spec are evaluated for scheduling purposes, then the kubelet of the selected node becomes responsible for running the container image with the help of the container runtime of the node. The Pod's name and labels are used for workload accounting purposes.
    - commands
        - `kubectl apply -f pod.yaml`
        - `kubectl get pods -o wide`
        - `kubectl run <podname> --image=nginx`
        - `kubectl run <podname> --image=nginx --port=80 --dry-run=client -o yaml > pod.yaml`
        - `kubectl describe pods <podname>`
        - `kubectl replace --force -f pod.yaml`
        - `kubectl delete -f pod.yaml`
        - `kubectl delete pods <podname>...`
- Labels
    - Labels are key-value pairs attached to Kubernetes objects
    - Labels are used to organize and select a subset of objects, based on the requirements in place. Many objects can have the same Label(s
    - Controllers use Labels to logically group together decoupled objects, rather than using objects' names or IDs.
- Label Selectors
    - Controllers, or operators, and Services, use label selectors to select a subset of objects
    - Kubernetes supports two types of Selectors:
        - Equality-Based Selectors:
            - filtering of objects based on Label keys and values.
            - Matching is achieved using the =, == (equals, used interchangeably), or != (not equals) operators.
            - For example, with env==dev or env=dev
        - Set-Based Selectors
            - allow filtering of objects based on a set of values
            - We can use in, notin operators for Label values, and exist/does not exist operators for Label keys.
            - For example, `env in (dev,qa)` and with `!app` we select objects with no Label key app.
- ReplicationControllers
    - Although no longer a recommended controller, a ReplicationController is a complex operator that ensures a specified number of replicas of a Pod is running at any given time, by constantly comparing the actual state with the desired state of the managed application.
    - If there are more Pods than the desired count, the replication controller randomly terminates the number of Pods exceeding the desired count, and, if there are fewer Pods than the desired count, then the replication controller requests additional Pods to be created until the actual count matches the desired count.
    - In addition to replication, the ReplicationController operator also supports application updates.
    - However, the default recommended controller is the Deployment which configures a ReplicaSet controller to manage application Pods' lifecycle.
- ReplicaSets
    - A ReplicaSet is, in part, the next-generation ReplicationController, as it implements the replication and self-healing aspects of the ReplicationController. ReplicaSets support both equality- and set-based Selectors, whereas ReplicationControllers only support equality-based Selectors.
    - we can run in parallel multiple instances of the application, hence achieving high availability.
    - The lifecycle of the application defined by a Pod will be overseen by a controller - the ReplicaSet.
    - With the help of the ReplicaSet, we can scale the number of Pods running a specific application container image.
    - Scaling can be accomplished manually or through the use of an autoscaler.
    - ReplicaSets can be used independently as Pod controllers but they only offer a limited set of features. A set of complementary features are provided by Deployments, the recommended controllers for the orchestration of Pods.
    - It creates new pods when needed, don't reuse pods
- Deployment
    - Deployment objects provide declarative updates to Pods and ReplicaSets.
    - The DeploymentController is part of the control plane node's controller manager, and as a controller it also ensures that the current state always matches the desired state of our running containerized application.
    - It allows for seamless application updates and rollbacks, known as the default RollingUpdate strategy, through rollouts and rollbacks
    - It also supports a disruptive, less popular update strategy, known as Recreate.
    - it directly manages its ReplicaSets for application scaling.
    - In case of update, Deployment controller creates a new replica set
    - A rolling update is triggered when we update specific properties of the Pod Template for a deployment. While planned changes such as updating the container image, container port, volumes, and mounts would trigger a new Revision, other operations that are dynamic in nature, like scaling or labeling the deployment, do not trigger a rolling update, thus do not change the Revision number.
    - Once the rolling update has completed, the Deployment will show both ReplicaSets A and B, where A is scaled to 0 (zero) Pods, and B is scaled to 3 Pods. This is how the Deployment records its prior state configuration settings, as **Revisions**.
    - Once ReplicaSet B and its 3 Pods are ready, the Deployment starts actively managing them. However, the Deployment keeps its prior configuration states saved as Revisions which play a key factor in the rollback capability of the Deployment - returning to a prior known configuration state.
    - Why previous replica set is not deleted?? [SO-Q](https://stackoverflow.com/questions/37255731/clean-up-replica-sets-when-updating-deployments)
    - commands
        - `kubectl create deployment mynginx --image=nginx:1.15-alpine`
        - `kubectl get deploy,rs,po -l app=mynginx `
        - `kubectl scale deploy mynginx --replica=3`
        - `kubectl describe deploy mynginx`
        - `kubectl rollout history deploy mynginx`
        - `kubectl rollout history deploy mynginx --revision=1`
        - `kubectl set image deployment mynginx nginx=nginx:1.16-alpine`
        - `kubectl rollout undo deployment mynginx --to-revision=1`
- DaemonSets
    - DaemonSets are operators designed to manage node agents. They resemble ReplicaSet and Deployment operators when managing multiple Pod replicas and application updates, but the DaemonSets present a distinct feature that enforces a single Pod replica to be placed per Node, on all the Nodes.
    - In contrast, the ReplicaSet and Deployment operators by default have no control over the scheduling and placement of multiple Pod replicas on the same Node.
    - DaemonSet operators are commonly used in cases when we need to collect monitoring data from all Nodes, or to run a storage, networking, or proxy daemons on all Nodes, to ensure that we have a specific type of Pod running on all Nodes at all times.
    - They are critical API resources in multi-node Kubernetes clusters.
    - Eg:
        - kube-proxy agent running as a Pod on every single node in the cluster
        - the Calico networking node agent implementing the Pod networking across all nodes of the cluster
    - Whenever a Node is added to the cluster, a Pod from a given DaemonSet is automatically placed on it. Although it ensures an automated process, the DaemonSet's Pods are placed on all cluster's Nodes by the controller itself, and not with the help of the default Scheduler.
    - When any one Node crashes or it is removed from the cluster, the respective DaemonSet operated Pods are garbage collected. If a DaemonSet is deleted, all Pod replicas it created are deleted as well.
    - The placement of DaemonSet Pods is still governed by scheduling properties which may limit its Pods to be placed only on a subset of the cluster's Nodes.  This can be achieved with the help of Pod scheduling properties such as nodeSelectors, node affinity rules, taints and tolerations. This ensures that Pods of a DaemonSet are placed only on specific Nodes, such as workers if desired. However, the default Scheduler can take over the scheduling process if a corresponding feature is enabled, accepting again node affinity rules.
    - Commands
        - `kubetl apply -f fluentd-agent.yaml`
        - `kubectl get daemonsets.app`
        - `kubectl get pods -o wide`
        - `kubectl get ds -A`
        - `kubectl delete daemonsets.apps fluentd-agent`
- Services
    - A containerized application deployed to a Kubernetes cluster may need to reach other such applications, or it may need to be accessible to other applications and possibly clients. This is problematic because the container does not expose its ports to the cluster's network, and it is not discoverable either.
    - The solution would be a simple port mapping, as offered by a typical container host. However, due to the complexity of the Kubernetes framework, such a simple port mapping is not that "simple". The solution is much more sophisticated, with the involvement of the kube-proxy node agent, IP tables, routing rules, cluster DNS server, all collectively implementing a micro-load balancing mechanism that exposes a container's port to the cluster's network, even to the outside world if desired. This mechanism is called a Service, and it is the recommended method to expose any containerized application to the Kubernetes network.
    - The benefits of the Kubernetes Service becomes more obvious when exposing a multi-replica application, when multiple containers running the same image need to expose the same port. This is where the simple port mapping of a container host would no longer work, but the Service would have no issue implementing such a complex requirement.

# Authentication, Authorization, Admission Control

- To access and manage Kubernetes resources or objects in the cluster, we need to access a specific API endpoint on the API server. Each access request goes through the following access control stages:
    - Authentication: Authenticate a user based on credentials provided as part of API requests.
    - Authorization: Authorizes the API requests submitted by the authenticated user.
    - Admission Control: Software modules that validate and/or modify user requests.
- Authentication
    - https://learning.edx.org/course/course-v1:LinuxFoundationX+LFS158x+1T2022/block-v1:LinuxFoundationX+LFS158x+1T2022+type@sequential+block@f6f8a73dc38647eeb576dce791500901/block-v1:LinuxFoundationX+LFS158x+1T2022+type@vertical+block@5e9b7d23126a4ff1b13b10ee71e04cc0
- Admission Control
    - Admission controllers are used to specify granular access control policies, which include allowing privileged containers, checking on resource quota, etc. We force these policies using different admission controllers, like ResourceQuota, DefaultStorageClass, AlwaysPullImages, etc. They come into effect only after API requests are authenticated and authorized.
    -

# Services

- Service
    - To access the application, a user or another application need to connect to the Pods.
    - As Pods are ephemeral in nature, resources like IP addresses allocated to them cannot be static
    - To overcome this situation, Kubernetes provides a higher-level abstraction called **Service**, which logically groups Pods and defines a policy to access them. This grouping is achieved via **Labels** and **Selectors**.
    - The Service name is also registered with the cluster's internal DNS service.
    - Services can expose single Pods, ReplicaSets, Deployments, DaemonSets, and StatefulSets. When exposing the Pods managed by an operator, the Service's Selector may use the same label(s) as the operator.
    - A Service provides load balancing by default while selecting the Pods for traffic forwarding.
    - If the targetPort is not defined explicitly, then traffic will be forwarded to Pods on the port on which the Service receives traffic.
    - It is very important to ensure that the value of the targetPort matches the value of the containerPort property of the Pod spec section.
    - A logical set of a Pod's IP address, along with the targetPort is referred to as a Service endpoint. In our example, the frontend-svc Service has 3 endpoints: 10.0.1.3:5000, 10.0.1.4:5000, and 10.0.1.5:5000. Endpoints are created and managed automatically by the Service, not by the Kubernetes cluster administrator.
- kube-proxy
    - Each cluster node runs a daemon called kube-proxy, a node agent that watches the API server on the master node for the addition, updates, and removal of Services and endpoints.
    - kube-proxy is responsible for implementing the Service configuration on behalf of an administrator or developer
    - for each new Service, on each node, kube-proxy configures iptables rules to capture the traffic for its ClusterIP and forwards it to one of the Service's endpoints. Therefore any node can receive the external traffic and then route it internally in the cluster based on the iptables rules.
- Traffic Policies
    - The kube-proxy node agent together with the iptables implement the load-balancing mechanism of the Service when traffic is being routed to the application Endpoints.
    - Due to restricting characteristics of the iptables this load-balancing is random by default. This mechanism does not guarantee that the selected receiving Pod is the closest or even on the same node as the requester
    - Since this is the iptables supported load-balancing mechanism, if we desire better outcomes, we would need to take advantage of traffic policies.
    - Traffic policies allow users to instruct the kube-proxy on the context of the traffic routing. The two options:
        - The **Cluster** option allows kube-proxy to target all ready Endpoints of the Service in the load-balancing process.
        - The **Local** option, however, isolates the load-balancing process to only include the Endpoints of the Service located on the same node as the requester Pod. While this sounds like an ideal option, it does have a shortcoming - if the Service does not have a ready Endpoint on the node where the requester Pod is running, the Service will not route the request to Endpoints on other nodes to satisfy the request.
- Service Discovery
    - Kubernetes supports two methods for discovering Services:
        - Environment Variables
            - As soon as the Pod starts on any worker node, the kubelet daemon running on that node adds a set of environment variables in the Pod for all active Services
            - With this solution, we need to be careful while ordering our Services, as the Pods will not have the environment variables set for Services which are created after the Pods are created.
        - DNS
            - Kubernetes has an add-on for DNS, which creates a DNS record for each Service and its format is **my-svc.my-namespace.svc.cluster.local**.
            - Services within the same Namespace find other Services just by their names.
            - ods from other Namespaces, such as test-ns, lookup the same Service by adding the respective Namespace as a suffix, such as redis-master.my-ns or providing the FQDN of the service as redis-master.my-ns.svc.cluster.local.
- ServiceType
    - While defining a Service, we can also choose its access scope. We can decide whether the Service:
        - Is only accessible within the cluster.
        - Is accessible from within the cluster and the external world.
        - Maps to an entity which resides either inside or outside the cluster.
    - Access scope is decided by **ServiceType** property, defined when creating the Service.
        - **ClusterIP** is the default ServiceType. A Service receives a Virtual IP address, known as its ClusterIP. This Virtual IP address is used for communicating with the Service and is accessible only from within the cluster.
        - With the **NodePort** ServiceType, in addition to a ClusterIP, a high-port, dynamically picked from the default range 30000-32767, is mapped to the respective Service, from all the worker nodes.
            - For example, if the mapped NodePort is 32233 for the service frontend-svc, then, if we connect to any worker node on port 32233, the node would redirect all the traffic to the assigned ClusterIP - 172.17.0.4.
            - If we prefer a specific high-port number instead, then we can assign that high-port number to the NodePort from the default range when creating the Service.
            - The NodePort ServiceType is useful when we want to make our Services accessible from the external world. The end-user connects to any worker node on the specified high-port, which proxies the request internally to the ClusterIP of the Service, then the request is forwarded to the applications running inside the container.
            - To manage access to multiple application Services from the external world, administrators can configure a reverse proxy - an **ingress**, and define rules that target specific Services within the cluster.
        - **LoadBalancer**: With the LoadBalancer ServiceType:
            - NodePort and ClusterIP are automatically created, and the external load balancer will route to them.
            - The Service is exposed at a static port on each worker node.
            - The Service is exposed externally using the underlying cloud provider's load balancer feature.
            - The LoadBalancer ServiceType will only work if the underlying infrastructure supports the automatic creation of Load Balancers and have the respective support in Kubernetes, as is the case with the Google Cloud Platform and AWS. If no such feature is configured, the LoadBalancer IP address field is not populated, it remains in Pending state, but the Service will still work as a typical NodePort type Service.
        - **ExternalIP** A Service can be mapped to an ExternalIP address if it can route to one or more of the worker nodes. Traffic that is ingressed into the cluster with the ExternalIP (as destination IP) on the Service port, gets routed to one of the Service endpoints.
            - This type of service requires an external cloud provider such as Google Cloud Platform or AWS and a Load Balancer configured on the cloud provider's infrastructure.
            - ExternalIPs are not managed by Kubernetes. The cluster administrator has to configure the routing which maps the ExternalIP address to one of the nodes.
        - **ExternalName**: is a special ServiceType that has no Selectors and does not define any endpoints. When accessed within the cluster, it returns a CNAME record of an externally configured Service.
            - The primary use case of this ServiceType is to make externally configured Services like my-database.example.com available to applications inside the cluster.
- Multi-Port Services
    - A Service resource can expose multiple ports at the same time if required.
    - This is a helpful feature when exposing Pods with one container listening on more than one port, or when exposing Pods with multiple containers listening on one or more ports.
- commands
    - `kubectl run pod-hello --image=pbitty/hello-from:latest --port=80 --expose=true`
    - `kubectl get po,svc,ep --show-labels`
    - `minikube service --all`
    - `kubectl edit svc pod-hello` - to change to node port
    - `kubectl create deployment deploy-hello --image=pbitty/hello-from:latest --port=80 --replicas=3`
      `kubectl expose deployment deploy-hello --type=NodePort`
- Whats the difference between node port and load balancer in terms of networking
- Why to use external load balancer?? Does we shouold use Local traffic policy with LoadBalancer type?? So that round robin can work? Image- https://learning.edx.org/course/course-v1:LinuxFoundationX+LFS158x+1T2022/block-v1:LinuxFoundationX+LFS158x+1T2022+type@sequential+block@57c4cdbe6ead47baa06e18967578b163/block-v1:LinuxFoundationX+LFS158x+1T2022+type@vertical+block@2bc963f158a34cde872d06e182e45ce5

# Deploying a Stand-Alone Application

- Deploy via dashboard
    - You can create anything from the dashboard
    - `minikube dashbaord`
- Exploring Labels and Selectors
    - `kubectl get pods -L k8s-app,label2` to show pods with labels
    - `kubectl get pods -l k8s-app=web-dash` to filter pods with labels
- Deploying application using CLI
    - `kubectl delete deployments web-dash`
    - Deleting a Deployment also deletes the ReplicaSet and the Pods it created:
    - `kubectl create -f webserver.yaml`
    - `kubectl create deployment webserver --image=nginx:alpine --replicas=3 --port=80`
    - `kubectl create -f webserver-svc.yaml`
    - `kubectl expose deployment webserver --name=web-service --type=NodePort`
    - It is not necessary to create the Deployment first, and the Service after. They can be created in any order. A Service will find and connect Pods based on the Selector.
    - `minikube ip`
    - `minikube service web-service`
    - `minikube service web-service --url`
- Liveness
    - If a container in the Pod has been running successfully for a while, but the application running inside this container suddenly stopped responding to our requests, then that container is no longer useful to us. This kind of situation can occur, for example, due to application deadlock or memory pressure. In such a case, it is recommended to restart the container to make the application available.
    - Rather than restarting it manually, we can use a Liveness Probe
    - Liveness Probe checks on an application's health, and if the health check fails, kubelet restarts the affected container automatically.
    - Liveness Probes can be set by defining:
        - Liveness command
        - Liveness HTTP request
        - TCP Liveness probe
    - ```yaml
      livenessProbe:
      exec:
        command:
        - cat
        - /tmp/healthy
      initialDelaySeconds: 15
      failureThreshold: 1
      periodSeconds: 5
      ```
    - `kubectl get pod <name> -w` to watch
    - Or can use events in pod describe details to know more
    - Liveness HTTP Request
        - ```yaml
          livenessProbe:
      httpGet:
      path: /healthz
      port: 8080
      httpHeaders:
        - name: X-Custom-Header
          value: Awesome
          initialDelaySeconds: 15
          periodSeconds: 5
       ```
    - TCP probe
        - ```yaml
          livenessProbe:
      tcpSocket:
      port: 8080
      initialDelaySeconds: 15
      periodSeconds: 5
      ```
- Readiness Probes
    - Sometimes, while initializing, applications have to meet certain conditions before they become ready to serve traffic. These conditions include ensuring that the dependent service is ready, or acknowledging that a large dataset needs to be loaded, etc
    - In such cases, we use Readiness Probes and wait for a certain condition to occur. Only then, the application can serve traffic.
    - ```yaml
      readinessProbe:
          exec:
            command:
            - cat
            - /tmp/healthy
          initialDelaySeconds: 5 
          periodSeconds: 5
      ```

# Kubernetes Volume Management

- Volumes
    - As we know, containers running in Pods are ephemeral in nature. All data stored inside a container is deleted if the container crashes. However, the kubelet will restart it with a clean slate, which means that it will not have any of the old data.
    - To overcome this problem, Kubernetes uses Volumes, storage abstractions that allow various storage technologies to be used by Kubernetes and offered to containers in Pods as storage media.
    - A Volume is essentially a mount point on the container's file system backed by a storage medium. The storage medium, content and access mode are determined by the Volume Type.
    - In Kubernetes, a Volume is linked to a Pod and can be shared among the containers of that Pod.
        - Although the Volume has the same life span as the Pod, meaning that it is deleted together with the Pod
        - the Volume outlives the containers of the Pod - this allows data to be preserved across container restarts.
- Volume Types
    - A directory which is mounted inside a Pod is backed by the underlying Volume Type. A Volume Type decides the properties of the directory, like size, content, default access modes, etc. Some examples of Volume Types are:
        - **emptyDir**: An empty Volume is created for the Pod as soon as it is scheduled on the worker node. The Volume's life is tightly coupled with the Pod. If the Pod is terminated, the content of emptyDir is deleted forever.
        - **hostPath**: With this we can share a directory between the host and the Pod. If the Pod is terminated, the content of the Volume is still available on the host.
        - **gcePersistentDisk**: With this we can mount a [Google Compute Engine (GCE) persistent disk](https://cloud.google.com/compute/docs/disks/) into a Pod.
        - awsElasticBlockStore, azureDisk, azureFile
        - **cephfs**: With cephfs, an existing CephFS volume can be mounted into a Pod. When a Pod terminates, the volume is unmounted and the contents of the volume are preserved.
        - **nfs**: With nfs, we can mount an NFS share into a Pod.
        - **iscsi**: With iscsi, we can mount an iSCSI share into a Pod.
        - **secret**: With this we can pass sensitive information, such as passwords, to Pods.
        - **configMap**: with this we can provide configuration data, or shell commands and arguments into a Pod.
        - **persistentVolumeClaim**: We can attach a [PersistentVolume](https://kubernetes.io/docs/concepts/storage/persistent-volumes/) to a Pod using a [persistentVolumeClaim](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#persistentvolumeclaims)
- PersistentVolumes
    - It becomes challenging, given the many Volume Types we have seen earlier.
    - Kubernetes resolves this problem with the PersistentVolume (PV) subsystem, which provides APIs for users and administrators to manage and consume persistent storage.
    - To manage the Volume, it uses the PersistentVolume API resource type, and to consume it, it uses the PersistentVolumeClaim API resource type.
    - A Persistent Volume is a storage abstraction backed by several storage technologies, which could be local to the host where the Pod is deployed with its application container(s), network attached storage, cloud storage, or a distributed storage solution. A Persistent Volume is statically provisioned by the cluster administrator.
    - PersistentVolumes can be [dynamically](https://kubernetes.io/docs/concepts/storage/dynamic-provisioning/) provisioned based on the StorageClass resource. A [StorageClass](https://kubernetes.io/docs/concepts/storage/storage-classes/) contains predefined provisioners and parameters to create a PersistentVolume. Using PersistentVolumeClaims, a user sends the request for dynamic PV creation, which gets wired to the StorageClass resource.
    - Some of the Volume Types that support managing storage using PersistentVolumes are: GCEPersistentDisk, AWSElasticBlockStore, AzureFile, AzureDisk, CephFS, NFS, iSCSI
- PersistentVolumeClaims
    - A PersistentVolumeClaim (PVC) is a request for storage by a user. Users request for PersistentVolume resources based on storage class, access mode, size, and optionally volume mode.
    - There are four access modes:
        - ReadWriteOnce (read-write by a single node)
        - ReadOnlyMany (read-only by many nodes)
        - ReadWriteMany (read-write by many nodes)
        - ReadWriteOncePod (read-write by a single pod).
    - The optional volume modes, filesystem or block device, allow volumes to be mounted into a pod's directory or as a raw block device respectively. Once a suitable PersistentVolume is found, it is bound to a PersistentVolumeClaim.
    - After a successful bound, the PersistentVolumeClaim resource can be used by the containers of the Pod.
    - Once a user finishes its work, the attached PersistentVolumes can be released. The underlying PersistentVolumes can then be reclaimed (for an admin to verify and/or aggregate data), deleted (both data and volume are deleted), or recycled for future usage (only data is deleted), based on the configured persistentVolumeReclaimPolicy property.
- Container Storage Interface (CSI)
    - Container orchestrators like Kubernetes, Mesos, Docker or Cloud Foundry used to have their own methods of managing external storage using Volumes. For storage vendors, it was challenging to manage different Volume plugins for different orchestrators.
    - Storage vendors and community members from different orchestrators started working together to standardize the Volume interface - a volume plugin built using a standardized [Container Storage Interface (CSI)](https://kubernetes.io/docs/concepts/storage/volumes/#csi) designed to work on different container orchestrators with a variety of storage providers.
    - Between Kubernetes releases v1.9 and v1.13 CSI matured from alpha to stable support, which makes installing new CSI-compliant Volume drivers very easy.
    - With CSI, third-party storage providers can develop solutions without the need to add them into the core Kubernetes codebase.
- Using a Shared hostPath Volume Type
    - ```yaml
      apiVersion: apps/v1
      kind: Deployment
      metadata:
        creationTimestamp: null
        labels:
          app: blue-app
        name: blue-app
      spec:
        replicas: 1
        selector:
          matchLabels:
            app: blue-app
        template:
          metadata:
            creationTimestamp: null
            labels:
              app: blue-app
              type: canary
          spec:
            volumes:
            - name: host-volume
              hostPath:
                path: /home/docker/blue-shared-volume
            containers:
            - image: nginx
              name: nginx
              ports:
              - containerPort: 80
              volumeMounts:
              - mountPath: /usr/share/nginx/html
                name: host-volume
            - image: debian
              name: debian
              volumeMounts:
              - mountPath: /host-vol
                name: host-volume
              command: ["/bin/sh", "-c", "echo Welcome to BLUE App! > /host-vol/index.html ; sleep infinity"]
      ```

# ConfigMaps and Secrets

- ConfigMaps
    - ConfigMaps allow us to decouple the configuration details from the container image
    - Using ConfigMaps, we pass configuration data as key-value pairs, which are consumed by Pods or any other system components and controllers, in the form of environment variables, sets of commands and arguments, or volumes.
    - We can create ConfigMaps from literal values, from configuration files, from one or more files or directories.
    - Create a ConfigMap from Literal Values
        - `kubectl create configmap my-config --from-literal=key1=value1 --from-literal=key2=value2`
        - `kubectl get configmaps my-config -o yaml`
        - `kubectl create -f customer1-configmap.yaml`
    - Create a ConfigMap from a File
        - Create a file permission-reset.properties with the following configuration data:
            ```
            permission=read-only
            allowed="true"
            resetCount=3
            ```
        - `kubectl create configmap permission-config --from-file=<path/to/>permission-reset.properties`
    - Use ConfigMaps Inside Pods: As Environment Variables
        - Inside a Container, we can retrieve the key-value data of an entire ConfigMap or the values of specific ConfigMap keys as environment variables.
        - All key-val pairs:
        ```yaml
         containers:
         - name: myapp-full-container
           image: myapp
           envFrom:
           - configMapRef:
           name: full-config-map
        ```
        - from specific key-value pairs
        ```yaml
        containers:
        - name: myapp-specific-container
          image: myapp
          env:
          - name: SPECIFIC_ENV_VAR1
            valueFrom:
              configMapKeyRef:
                name: config-map-1
                key: SPECIFIC_DATA
          - name: SPECIFIC_ENV_VAR2
            valueFrom:
              configMapKeyRef:
                name: config-map-2
                key: SPECIFIC_INFO
        ```
    - Use ConfigMaps Inside Pods: As Volumes
        - We can mount a vol-config-map ConfigMap as a Volume inside a Pod. For each key in the ConfigMap, a file gets created in the mount path (where the file is named with the key name) and the respective key's value becomes the content of the file:
        - ```yaml
          containers:
          - name: myapp-vol-container
            image: myapp
            volumeMounts:
            - name: config-volume
              mountPath: /etc/config
          volumes:
          - name: config-volume
            configMap:
              name: vol-config-map
          ```
    - Any file (not only key-calue pair like properties) like .html can be used to create config map from file. Like in the demo video of edx course, they get the index.html file in an nginx server via configmap
- Secrets
    - While creating any Deployment, we can include any password in the Deployment's YAML file, but the password would not be protected.
    - In this scenario, the Secret object can help by allowing us to encode the sensitive information before sharing it. With Secrets, we can share sensitive information like passwords, tokens, or keys in the form of key-value pairs, similar to ConfigMaps. In Deployments or other resources, the Secret object is referenced, without exposing its content.
    - It is important to keep in mind that by default, the Secret data is stored as plain text inside **etcd**, therefore administrators must limit access to the API server and etcd. However, Secret data can be encrypted at rest while it is stored in etcd, but this feature needs to be enabled at the API server level
    - Create a Secret from Literal Values
        - `kubectl create secret generic my-password --from-literal=password=mysqlpassword`
        - `kubectl get secret my-password`
        - `kubectl describe secret my-password`
    - Create a Secret from a Definition Manifest
        - ```yaml
          apiVersion: v1
          kind: Secret
          metadata:
            name: my-password
          type: Opaque
          data:
            password: bXlzcWxwYXNzd29yZAo=
          ```
        - ```yaml
          apiVersion: v1
          kind: Secret
          metadata:
            name: my-password
          type: Opaque
          stringData:
            password: mysqlpassword
          ```
        - With `stringData` maps, there is no need to encode the value of each sensitive information field with base64. The value of the sensitive field will be encoded when the my-password Secret is created.
        - `kubectl create -f mypass.yaml`
    - Create a Secret from a File
        - `echo mysqlpassword | base64 > password.txt`
        - `kubectl create secret generic my-file-password --from-file=password.txt`
    - Use Secrets Inside Pods: As Environment Variables
        - ```yaml
          spec:
          containers:
          - image: wordpress:4.7.3-apache
            name: wordpress
            env:
            - name: WORDPRESS_DB_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: my-password
                  key: password
          ```
    - Use Secrets Inside Pods: As Volumes
        - ```yaml
          spec:
          containers:
          - image: wordpress:4.7.3-apache
            name: wordpress
            volumeMounts:
            - name: secret-volume
              mountPath: "/etc/secret-data"
              readOnly: true
          volumes:
          - name: secret-volume
            secret:
              secretName: my-password
          ```

# Chapter 14. Ingress

- With Services, routing rules are associated with a given Service. They exist for as long as the Service exists, and there are many rules because there are many Services in the cluster. If we can somehow decouple the routing rules from the application and centralize the rules management, we can then update our application without worrying about its external access. This can be done using the Ingress resource.
- To allow the inbound connection to reach the cluster Services, Ingress configures a Layer 7 HTTP/HTTPS load balancer for Services and provides the following:
    - TLS (Transport Layer Security)
    - Name-based virtual hosting
    - Fanout routing
    - Loadbalancing
    - Custom rules
- With Ingress, users do not connect directly to a Service. Users reach the Ingress endpoint, and, from there, the request is forwarded to the desired Service.
- Name-Based Virtual Hosting
    - ```yaml
      apiVersion: networking.k8s.io/v1 
      kind: Ingress
      metadata:
        annotations:
          kubernetes.io/ingress.class: "nginx"
        name: virtual-host-ingress
        namespace: default
      spec:
        rules:
        - host: blue.example.com
          http:
            paths:
            - backend:
                service:
                  name: webserver-blue-svc
                  port:
                    number: 80
              path: /
              pathType: ImplementationSpecific
        - host: green.example.com
          http:
            paths:
            - backend:
                service:
                  name: webserver-green-svc
                  port:
                    number: 80
              path: /
              pathType: ImplementationSpecific
      ```
- Fanout Ingress rules
    - ```yaml
      apiVersion: networking.k8s.io/v1
      kind: Ingress
      metadata:
        annotations:
          kubernetes.io/ingress.class: "nginx"
        name: fan-out-ingress
        namespace: default
      spec:
        rules:
        - host: example.com
          http:
            paths:
            - path: /blue
              backend:
                service:
                  name: webserver-blue-svc
                  port:
                    number: 80
              pathType: ImplementationSpecific
            - path: /green
              backend:
                service:
                  name: webserver-green-svc
                  port:
                    number: 80
              pathType: ImplementationSpecific
      ```
- Ingress Controller
    - An Ingress Controller is an application watching the Control Plane Node's API server for changes in the Ingress resources and updates the Layer 7 Load Balancer accordingly.
    - Ingress Controllers are also know as Controllers, Ingress Proxy, Service Proxy, Revers Proxy, etc
    - Kubernetes supports an array of Ingress Controllers, and, if needed, we can also build our own
        -  GCE L7 Load Balancer Controller, Nginx, Contour, HAProxy Ingress, Istio Ingress, Kong, Traefik, etc.
    -  Starting the Ingress Controller in Minikube is extremely simple. Minikube ships with the Nginx Ingress Controller set up as an addon, disabled by default.
        - `minikube addons enable ingress`
- Deploy an Ingress Resource
    - `kubectl create -f virtual-host-ingress.yaml`

# Chapter 15. Advanced Topics

- Annotations
    - With Annotations, we can attach arbitrary non-identifying metadata to any objects, in a key-value format. Annotations are displayed while describing an object.
      ```yaml
        apiVersion: apps/v1
        kind: Deployment
        metadata:
          name: webserver
          annotations:
            description: Deployment based PoC dates 2nd Mar'2022
      ```
    - Unlike Labels, annotations are not used to identify and select objects. Annotations can be used to:
        - Store build/release IDs, PR numbers, git branch, etc.
        - Phone/pager numbers of people responsible, or directory entries specifying where such information can be found.
        - Pointers to logging, monitoring, analytics, audit repositories, debugging tools, etc.
        - Ingress controller information.
        - Deployment state and revision information.
- Quota and Limits Management
    - When there are many users sharing a given Kubernetes cluster, there is always a concern for fair usage. A user should not take undue advantage. To address this concern, administrators can use the ResourceQuota API resource, which provides constraints that limit aggregate resource consumption per Namespace
    - We can set the following types of quotas per Namespace:
        - Compute Resource Quota: total sum of CPU, memory, etc in a namespace
        - Storage Resource Quota: total sum of storage resources (PersistentVolumeClaims, requests.storage, etc.)
        - Object Count Quota: the number of objects of a given type (pods, ConfigMaps, PersistentVolumeClaims, ReplicationControllers, Services, Secrets, etc.)
    - An additional resource that helps limit resources allocation to pods and containers in a namespace, is the **LimitRange**, used in conjunction with the ResourceQuota API resource. A LimitRange can:
        - Set compute resources usage limits per Pod or Container in a namespace.
        - Set storage request limits per PersistentVolumeClaim in a namespace.
        - Set a request to limit ratio for a resource in a namespace.
        - Set default requests and limits and automatically inject them into Containers' environments at runtime.
- Autoscaling
    - While it is fairly easy to manually scale a few Kubernetes objects, this may not be a practical solution for a production-ready cluster where hundreds or thousands of objects are deployed. We need a dynamic scaling solution which adds or removes objects from the cluster based on resource utilization, availability, and requirements.
    - Autoscaling can be implemented in a Kubernetes cluster via controllers which periodically adjust the number of running objects based on single, multiple, or custom metrics. There are various types of autoscalers available in Kubernetes which can be implemented individually or combined for a more robust autoscaling solution:
        - Horizontal Pod Autoscaler (HPA): HPA is an algorithm-based controller API resource which automatically adjusts the number of replicas in a ReplicaSet, Deployment or Replication Controller based on CPU utilization.
        - Vertical Pod Autoscaler (VPA): VPA automatically sets Container resource requirements (CPU and memory) in a Pod and dynamically adjusts them in runtime, based on historical utilization data, current resource availability and real-time events.
        - Cluster Autoscaler: automatically re-sizes the Kubernetes cluster when there are insufficient resources available for new Pods expecting to be scheduled or when there are underutilized nodes in the cluster.
- Jobs and CronJobs
    - A Job creates one or more Pods to perform a given task. The Job object takes the responsibility of Pod failures. It makes sure that the given task is completed successfully. Once the task is complete, all the Pods have terminated automatically. Job configuration options include:
        - parallelism - to set the number of pods allowed to run in parallel;
        - completions - to set the number of expected completions;
        - activeDeadlineSeconds - to set the duration of the Job;
        - backoffLimit - to set the number of retries before Job is marked as failed;
        - ttlSecondsAfterFinished - to delay the cleanup of the finished Jobs.
    - Starting with the Kubernetes 1.4 release, we can also perform Jobs at scheduled times/dates with **CronJobs**, where a new Job object is created about once per each execution cycle. The CronJob configuration options include:
        - startingDeadlineSeconds - to set the deadline to start a Job if scheduled time was missed;
        - concurrencyPolicy - to allow or forbid concurrent Jobs or to replace old Jobs with new ones.
- StatefulSets
    - The StatefulSet controller is used for stateful applications which require a unique identity, such as name, network identifications, or strict ordering. For example, MySQL cluster, etcd cluster.
    - The StatefulSet controller provides identity and guaranteed ordering of deployment and scaling to Pods. However, the StatefulSet controller has very strict Service and Storage Volume dependencies that make it challenging to configure and deploy. It also supports scaling, rolling updates, and rollbacks.
- Custom Resources
    - in most cases existing Kubernetes resources are sufficient to fulfill our requirements, we can also create new resources using custom resources
    - To make a resource declarative, we must create and install a custom controller, which can interpret the resource structure and perform the required actions.
- Kubernetes Federation
    - With Kubernetes Cluster Federation we can manage multiple Kubernetes clusters from a single control plane. We can sync resources across the federated clusters and have cross-cluster discovery. This allows us to perform Deployments across regions, access them using a global DNS record, and achieve High Availability.
    - Although still a Beta feature, the Federation is very useful when we want to build a hybrid solution, with one cluster running inside our private datacenter and another one in the public cloud, allowing us to avoid provider lock-in. We can also assign weights for each cluster in the Federation, to distribute the load based on custom rules.
- Security Contexts and Pod Security Admission
    - At times we need to define specific privileges and access control settings for Pods and Containers. Security Contexts allow us to set Discretionary Access Control for object access permissions, privileged running, capabilities, security labels, etc. However, their effect is limited to the individual Pods and Containers where such context configuration settings are incorporated in the spec section.
    - In order to apply security settings to multiple Pods and Containers cluster-wide, we can use the Pod Security Admission, a built-in admission controller for Pod Security that is enabled by default in the API Server. It can enforce the three Pod Security Standards at namespace level, by automating the security context restriction to pods when they are deployed.
- Network Policies
    - Kubernetes was designed to allow all Pods to communicate freely, without restrictions, with all other Pods in cluster Namespaces. In time it became clear that it was not an ideal design, and mechanisms needed to be put in place in order to restrict communication between certain Pods and applications in the cluster Namespace. Network Policies are sets of rules which define how Pods are allowed to talk to other Pods and resources inside and outside the cluster. Pods not covered by any Network Policy will continue to receive unrestricted traffic from any endpoint.
- Monitoring, Logging, and Troubleshooting
    - Metrics: In Kubernetes, we have to collect resource usage data by Pods, Services, nodes, etc., to understand the overall resource consumption and to make decisions for scaling a given application. Two popular Kubernetes monitoring solutions are the
        - Kubernetes Metrics Server: its a cluster-wide aggregator of resource usage data - a relatively new feature in Kubernetes.
        - Prometheus:
    - Logging: In Kubernetes, we can collect logs from different cluster components, objects, nodes, etc. Unfortunately, however, Kubernetes does not provide cluster-wide logging by default, therefore third party tools are required to centralize and aggregate cluster logs. A popular method to collect logs is using Elasticsearch together with Fluentd with custom configuration as an agent on the nodes. Fluentd is an open source data collector, which is also part of CNCF.
    - in the case of several consecutive container restarts due to failures - the logs of the very last failed container
        - `kubectl logs pod-name -c container-name -p`
        - `kubectl get events`
- Helm
    - To deploy a complex application, we use a large number of Kubernetes manifests to define API resources such as Deployments, Services, PersistentVolumes, PersistentVolumeClaims, Ingress, or ServiceAccounts. It can become counter productive to deploy them one by one. We can bundle all those manifests after templatizing them into a well-defined format, along with other metadata. Such a bundle is referred to as Chart. These Charts can then be served via repositories, such as those that we have for rpm and deb packages.
    - Helm is a package manager (analogous to yum and apt for Linux) for Kubernetes, which can install/update/delete those Charts in the Kubernetes cluster.
    - Helm is a CLI client that may run side-by-side with kubectl on our workstation, that also uses kubeconfig to securely communicate with the Kubernetes API server.
    - The helm client queries the Chart repositories for Charts based on search parameters, downloads a desired Chart, and then it requests the API server to deploy in the cluster the resources defined in the Chart.
- Service Mesh
    - Service Mesh is a third party solution alternative to the Kubernetes native application connectivity and exposure achieved with Services paired with Ingress Controllers. Service Mesh tools are gaining popularity especially with larger organizations managing larger, more dynamic Kubernetes clusters. These third party solutions introduce features such as service discovery, multi-cloud routing, and traffic telemetry.
    - Several implementations of Service Mesh are:
        - Consul by HashiCorp
        - Istio is one of the most popular service mesh solutions, backed by Google, IBM and Lyft
- Application Deployment Strategies
    - Canary / Blue/Green



# More
- Stateful vs Deployment: https://stackoverflow.com/questions/41583672/kubernetes-deployments-vs-statefulsets
- Course
    - https://www.udemy.com/course/learn-kubernetes/
    - https://learning.edx.org/course/course-v1:LinuxFoundationX+LFS158x+1T2022/home
- Mac Node Port issue - https://github.com/kubernetes/minikube/issues/11193

# Real Examples

- [A Detailed Example of Deployment of a Stateful Application](https://betterprogramming.pub/kubernetes-a-detailed-example-of-deployment-of-a-stateful-application-de3de33c8632)
    - Step 1. Containerize the application and upload image to container image registry
    - Step 2. Environment config setup i.e. configmaps and secrets
    - Step 3. Configure PVC, service, and deployment for database
    - Step 4. Configure service, deployment for the backend app
    - Step 5. Front-end env config (server-uri) and service + deployment

# FAQ

## Deployment vs StatefulSet
    - Deployment volume is shared while StatefulSet volume is unique to each pod/replica/slave
    - https://stackoverflow.com/a/56865064/742173

## Ingres vs Service