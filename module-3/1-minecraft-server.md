---
layout: page
title: "Let's host a Minecraft Server!"
parent: "Module 3: Hosting Applications"
nav_order: 1
---
# Let's host a Minecraft Server!
In this section, we will set up a Minecraft server on our Kubernetes cluster. This will give us a chance to see how to deploy an application and manage its resources.

Deploying a minecraft server is one of the simpler tasks that we can do with Kubernetes. The docker image has already been made for us. All we have to do is to deploy it.

In Docker, we deploy containers, but in Kubernetes, we deploy **pods**. A pod is a group of one or more containers. Usually, pods are only used with a single container. The only time when you would use two containers in a single pod is when they are tightly coupled and need to share resources, like a web server and a monitoring agent.

The tool we need to deploy our minecraft server is `kubectl`. This is the command line tool that we use to interact with our Kubernetes cluster. We will use it to create a pod that runs the minecraft server. You should have installed it when you set up your cluster. To test wether you instlaled it correctly, run this command:
```bash
kubectl get nodes
```

The point of this command is to list all the nodes in our cluster. If you see a list of nodes, then your cluster is up and running. If not, then you need to go back and check your installation.

Now that we know `kubectl` is working, let's create a pod for our minecraft server. Kubernetes often uses YAML files called manifests to create and manage resources. This is because it makes it easy to create, edit, and delete resources from the cluster. Create a file called `minecraft-pod.yaml` and add the following content:
```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: minecraft-server
---
apiVersion: v1
kind: Pod
metadata:
  name: minecraft-server
  namespace: minecraft-server
  labels:
    app: minecraft-server
spec:
    containers:
    - name: minecraft-server
      image: itzg/minecraft-server
      ports:
      - containerPort: 25565
---
apiVersion: v1
kind: Service
metadata:
  name: minecraft-server
  namespace: minecraft-server
spec:
    selector:
        app: minecraft-server
    ports:
    - protocol: TCP
      port: 25565
      targetPort: 25565
      NodePort: 30007
    type: NodePort    
```
This manifest creates 3 different resources, each seperated by a `---`. These are:
1. A **namespace** called `minecraft-server`. Namespaces are used to organize resources in a cluster. We will use this namespace to keep all the resources for our minecraft server together.
2. A **pod** called `minecraft-server`. This pod runs the minecraft server using the `itzg/minecraft-server` docker image. It also exposes port 25565, which is the default port for minecraft. Take note that the `containers` section is a list of contianers. This is so we can add more containers if we wish.
3. A **service** called `minecraft-server`. This service exposes the pod to the outside world. There are many different types of services, but type `NodePort` means it exposes a specific port on each of the worker nodes. In this case, it exposes port 30007 on each node, which forwards traffic to port 25565 on the pod.

Now that we have our manifest, we can use `kubectl` to create the resources in our cluster. Run the following command:
```bash
kubectl apply -f minecraft-pod.yaml
```

You can see wether the resources have been provisioned. All you have to do is use the kubectl get command with different resource types. Here are some commands to try out:
```bash
kubectl get pods -n minecraft-server
kubectl get services -n minecraft-server
kubectl get namespaces
```

You can now take a second to test out the server. Is it working? Open minecraft and try it out. Enter the IP address of any of your nodes. It doesn't matter which. `kubeproxy` will make sure that the traffic will go where it needs to.

Now that we are done with our minecraft server, we can delete it. Just use this command for that.
```bash
kubectl delete -f minecraft-pod.yaml
```

In the next lesson, we will see what happened behind the scenes when we provisioned this. Also, we did something very wrong that we need to fix. I will point that out in the next lesson.