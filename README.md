# Kubernetes-Basic
This repository come to explain kubernetes basic objects, their attributes and how to use them.

## Kubernetes Terminology
- **Kubectl** – is a command line tool that allows us to talk to the kubernetes api server, kubectl is configured with a kube config file, the address and authentication detail is defined in the kube config this tells kubectl where the api server is and which cluster we can talk to.
<br/><br/>
- **Kube config file** – in the config file we can set a context which points to the cluster, also we define users who can access the cluster or part of him by token or username and password. Kubectl connect to the cluster via the api server that takes our configuration and makes it happen. in the cluster the api server runs in the control plane alongside the scheduler and other controllers. controllers watch the state of the workload and make sure the desired state matches the actual state.
<br/><br/>
- **Pod** – pod is a workload that we want to deploy to kubernetes the workload can be any type of compute script code or application or part of the application, we can also deploy more than one process in the pod and in other words multiple containers. containers have environment variables, ports to receive network traffic, resources which define request values of how much cpu and memory the pod needs as well as limits and kubernetes will throttle pods that use more cpu as well as kill pods that go over their memory limit.
 - **Volume Mounts** – volume mounts allow us to mount files into specific paths of the container in order to mount a file a volume needs to be defined.
 - **Volumes** – volumes are a medium of storage attached to a pod volumes can be a folder on the host where the pod is running or a persistent volume. volumes can also be configurations or secrets configurations are defined as config maps.
<br/><br/>
- **Liveliness Probes** – liveliness probes ensure that containers are alive and kubernetes will restart the pod if the probe condition is not met.
<br/><br/>
- **Readiness Probes** – readiness probes are similar but tells kubernetes when the pod is ready to take traffic. this is for pods that may need to initialize data or get ready to accept network connections.
<br/><br/>
- **[Config Map](https://github.com/matanelg/Kubernetes-Basic/tree/main/ConfigMap)** – config maps allow us to store configurations for pods as files or key value pairs that can be mapped to environment variables.
<br/><br/>
- **[Secret](https://github.com/matanelg/Kubernetes-Basic/tree/main/Secret)**** – a secret is similar we can store files tls certificates or key value pairs that can be mapped to environment variables.
<br/><br/>
- **Cron Job** – a cron job is a way to schedule a pod you can run a pod once a day once a week once a month or at your own custom schedule every time the schedule is triggered a job gets created and a job can run one or more pods.
<br/><br/>
- **Deployment** – if you want to run a pod constantly like a web server a proxy or an application you can use a deployment. a deployment has a number of replicas which tells kubernetes how many pods to run concurrently. pods are distributed across nodes in the cluster. nodes are machines where the containers run as pods. nodes can be physical on-prem or virtual cloud machines. when pods are created they may move around. this is okay for web servers but not for databases or caches this is where stateful sets come in.
<br/><br/>
- **Stateful Sets** –  it's similar to a deployment but allows us to pin pods to nodes. when a pod restarts it gets recreated on the same node. to ensure its storage is reattached to the same pod stateful sets also have replicas and pod information as described earlier dateful sets provide volume claim templates that allows us to dynamically provision storage for a new pod when it scales up using a storage class.
<br/><br/>
- **Storage Class** – storage classes abstract storage that a cluster offers a cluster can offer various amount of storage like azure file share aws block storage gce disk nfs network share local file storage and more storage classes are defined at a cluster level so it can be consumed by the platform to consume a storage class in a pod we define a persistent volume.
<br/><br/>
- **Persistent Volume** – a persistent volume indicates its storage class and represents a size of the storage available to the cluster every pod can have its own persistent volume or you can share persistent volumes through persistent volume claims.
<br/><br/>
- **Persistent Volume Claims** –  persistent volume claims allow developers to claim storage from a persistent volume without having to provision or interact with the storage itself.
<br/><br/>
- **Daemon Set** – daemon sets has its own scheduling mechanisms to ensure a pod runs on every node.
<br/><br/>
- **Services** – services allow us to define how to access pods over the network. when we define a service we give it a name and we select pods using label selectors we define a service port and map it to a pod container port. the pods are then accessible over the service name as a dns plus port. the types of services available are cluster ip for private communication, load balancer for public communication mostly via cloud resources and node port where the ports are expose and can be reached by public communication.
<br/><br/>
- **Ingress** – ingress is a rule set that allows us to define how we want to accept traffic into the cluster without having to configure proxies and route to various services. ingress have rules where we define a host domain, a url path and a target service and port. this allows us to accept traffic on a single load balancer service and route request to different private services. ingress objects are managed by an ingress controller which watches the state of ingresses and updates its core proxy whenever ingresses gets added deleted or updated. there are many types of ingress controller implementations like nginx traffic envoy haproxy and more.
<br/><br/>
- **Controller** – controllers are pods that run in a controlled loop. a controlled loop is a non-terminating loop that regulates the state of a system. a controller tracks at least one kubernetes resource type. ingress controllers track ingress objects there are other types of controllers we can even build our own controller.
<br/><br/>
- **Namespaces** – namespaces provide a way to divide cluster resources between users teams and departments. it's like a grouping mechanism. You may want to deploy all your monitoring pods to a monitoring namespace and deploy your ingress controllers to its own namespace. by default all the control plane pods are in the kubesystem namespace. you could also have a namespace per team or none at all. for objects the names should be unique per namespace. to give users permissions to namespaces we use rbac.
<br/><br/>
- **Rbac** – rbac roles and role bindings provide permissions to objects in a namespace. cluster roles and cluster role bindings provide permissions across all namespaces.
<br/><br/>



