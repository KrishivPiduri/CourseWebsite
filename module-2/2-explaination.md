---
layout: page
title: "What Just happened?"
parent: "Module 2: Setting up Kubernetes"
nav_order: 2
---

# What Just Happened?

We just did a lot, so let's figure out what exactly happened.

I hope the first couple steps were rather simple. First, we installed a container runtime. Second, we installed Kubeadm and set everything up so we could initialize the cluster. Then, this is the complicated part. We just ran a command and a lot of things started happening. Let's break it down.

The command we ran is `kubeadm init`. The goal of this command is rather simple. Given any node, it can provision a Kubernetes cluster with that node as the master. But what exaclty does it do to achieve that?

The first thing I want you to acknowledge is that Kubernetes is a collection of many different components. This makes it very flexible, because you can choose to have some components while excluding others. You can also make some components redundant to ensure zero downtime. In addition to that, you can choose the distribution of components across your devices. Let's see what the components are and what they do:

1. **etcd**: This is a distributed key-value store that Kubernetes uses to store all of its data. It is the source of truth for the cluster. Every time you make a change to the cluster, it is stored in etcd. This key-value store basically answers the question of "What is the cluster supposed to look like?".
   1. This component is rather important, so it is usually recommended to have multiple instances of it running in a cluster to ensure high availability.
1. **kube-controller-manager**: This component does exactly what it says: it manages controllers. What is a controller, you ask? It's basically a process that maintains the desired state of the cluster. Remember that the desired state of the cluster is dictated by whatever is in etcd. The controller manager is responsible for ensuring that the desired state is maintained.
   1. For example, let's say you tell Kubernetes that you want to provision a website. One of the controllers managed by the kube-controller manager will realize that the desired state of the cluster involves a website existing, but that isn't the case. Then it will venture to fix it.
   1. There are many different types of controllers, each with their own purpose. For example, there is a controller that manages nodes, another that manages endpoints, and another that manages namespaces. If you don't understand, don't worry. We'll get to that.
1. **kube-scheduler**: This component is responsible for scheduling pods. A pod is the smallest deployable unit in Kubernetes. I know that might not mean much to you, right now, but just think of it as a container (or multiple containers) that are running together on the same host. The scheduler's job is to figure out which node a pod should be placed on.
   1. The scheduler takes into account many factors, such as resource availability, node affinity, and taints and tolerations.
1. **kubelet**: This is an agent that runs on each node in the cluster. It is responsible for ensuring that the containers in a pod are running. The kubelet communicates with the container runtime to make sure that the containers are running as expected. This means, none of the other components care about what container runtime is being used on each node. As long as kubelet is doing its job, the other components don't care.
1. **kube-proxy**: This component is responsible for networking in the cluster. It maintains network rules on each node so that pods can communicate with each other. It also provides load balancing for services. In short, it makes sure that the network is working as expected.
1. **kube-apiserver**: This is the component at the center of the cluster. This is what orchestrates everything and makes everything work together. This is also the component that, you, the user, will use to interact with the cluster. Everytime you do something on the cluster, it is this component that is responsible for making it happen.

Don't worry if you still don't feel like you fully understand this. We will be running though a lot more examples throughout the course regarding these components and their jobs.

While this component structure is part of the reason why Kubernetes is so awesome, this also makes it quite annoying to work with. If you want to set up a cluster, you have to deal with all of those components. That is why kubeadm was created. The goal is that if you want to set up a simple cluster, kubeadm has got your back. Did you see how easy that set up was? Before kubeadm, it was way harder. Again, if anybody wants to try it out, check out this github repo. It will have everything you need to get started: [Kubernetes the Hard Way](https://github.com/kelseyhightower/kubernetes-the-hard-way).

In the next module, we will start using our cluster. We will host a minecraft server and take a look at what happened behind the scenes.