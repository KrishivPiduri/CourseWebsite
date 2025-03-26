---
layout: page
title: "Hosting our own Website!"
parent: "Module 3: Hosting Applications"
nav_order: 3
---
# Hosting our own Website!

Again, the first step will be to make our manifest file. This will be to very similar to our manifest from the previous project. The only thing we will change is the creation of the pod. Rather than creating a standalone pod, we will be creating a replicaset. Here is the manifest:

```yaml

```

Do you understand how the manifest for a replica set works? All we do is define the pod that we want this replica set to use in the `template` field. `replicas` determines the number of replicas of the template pod to maintain.

In this case, we said we want two replicas. The template says that all those pods need to have the label `app: nginx`. The selector for the service is also `app: nginx`. This means the service selector includes any pods in the replicaset. Any traffic sent to the service will be LoadBalanced across all of those pods. This ability for us to easily be able to scale the number of pods available is extremely useful in the real world where you might need to scale your app very often.

There is another huge advantage to using replica sets. In our previous deployment, our pod was very fragile. In this configuration, however, whenever a pod is deleted, the `replication controller` will make sure pods are created to replace those replicas.

Let’s deploy it and see this for yourselves:

```bash
kubectl apply -f manifest.yaml
```

You can wait for all the resources to be created. You can check the status of the replicaset by running this command:

```bash
kubectl get replicasets -n web-app
```

You can also see all the pods that have been created by the replicaset using this command

```bash
kubectl get pods -n web-app
```

We can delete one of the pods just to see that it will get re-created. Use this command to delete a pod

```bash
kubectl delete pod <pod_name> -n web-app
```

Of course, you will need to replace `<pod_name>` with the name of one of the pods. If you run `kubectl get pods` again, you should see that there is another pod being created.

Another cool thing you can do with replicasets is to scale them. You can change the number of replicas in the manifest and re-apply it. Or you can use this command to scale it:

```bash
kubectl scale replicaset web-app --replicas=4 -n web-app
```

This will scale the replicaset to 4 replicas. You can check the status of the replicaset again to see that it has been scaled. You can also check the status of the pods to see that there are now 4 pods running.

You can also delete the replicaset and see that all the pods are deleted. You can do this with this command:

```bash
kubectl delete replicaset web-app -n web-app
```

Now that you have witnessed the power of Replica sets, I think you're ready for the next step. We will be talking about **Deployments**. Deployments are a higher level abstraction that manages Replica sets. They are the most common way to manage applications in Kubernetes. They provide a lot of features that make it easier to manage applications, such as rolling updates and rollbacks. We will be talking about them in the next lesson.