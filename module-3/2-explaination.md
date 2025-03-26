---
layout: page
title: "Behind the Scenes"
parent: "Module 3: Hosting Applications"
nav_order: 2
---
# Behind the Scenes
In this lesson, let's take a look at what happened behind the scenes when we deployed our minecraft server.

What we see is that we ran the `kubectl apply` command and kubernetes just deployed some resources to make magic happen. But what exaclty happened with each of the components? Let's figure that out. Here are the steps:

1. **API Server**: When we ran the `kubectl apply` command, it sent a request to the Kubernetes API server. The API server then writes to etcd regarding the new desired state of the server. This state involves a new namespace, pod, and service.
1. **Controller Manager**: There are a lot of controllers involved in the provisioning of our minecraft server. Here's a list:
   1. **Namespace Controller**: This controller watches for new namespaces and creates them in the cluster.
   1. **Pod Controller**: This controller watches for new pods and creates them in the cluster.
   1. **Service Controller**: This controller watches for new services and creates them in the cluster.
2. **Scheduler**: The scheduler is responsible for placing pods on nodes. It looks at the resources required by the pod and the resources available on each node. It then places the pod on a node that has enough resources.
3. **Kubelet**: The kubelet is responsible for managing the pods on each node. It communicates with the API server to get the desired state of the pod and then starts the pod.
4. **Kube Proxy**: The kube proxy is responsible for managing the network for the pods. It creates iptables rules to route traffic to the correct pod.

As you can see, even in a simple task like deploying a minecraft server, there are a lot of components involved. This is the power of Kubernetes. It abstracts away the complexity of managing containers and allows us to focus on deploying and managing our applications.

But, I previously mentioned that there was a problem with our deployment. The thing is, we aren't supposed to directly create pods in Kubernetes. The pod is supposed to be an ephemeral resource. This means it is meant to be created and destroyed frequently. It is supposed to be a temporary resource. Kubernetes doesn't hesitate to destroy these pods. There are a lot of minor things that will get the pod destroyed and Kubernetes won't care to recreate it.

For example, if the node that the pod is running on goes down, the pod will be destroyed. Kubernetes will not recreate it. This is a problem because we want our minecraft server to be always available. If the pod is destroyed, we want it to be recreated automatically. But Kubernetes won't do that for us.

There is a solution for this, though. We should create a different type of resource. One that will recreate our pod if it is detroyed. This resource is called a **Replicaset**. A replicaset ensures that a specified number of pod replicas are running at any given time. If a pod is deleted, the replicaset will create a new one to replace it. We will explore replicasets in the next lesson.