---
layout: page
title: "Installing Kubernetes"
parent: "Module 2: Setting up Kubernetes"
nav_order: 1
---

# Installing Kubernetes

The firs thing we need is a container runtime. A container runtime is the system that actually creates the containers and has the technology to make all that magic happen. Kubernetes, on the other hand, just manages all the containers across devices on the runtime.

The container runtime we will use is called ContainerD. The easiest way to do so would be to just install Docker. [Here](https://docs.docker.com/engine/install/ubuntu/) are the instructions to do so. Make sure to install it on all the worker nodes. 

The next thing you will need to do is disable swap. Swap memory just doesn't work well with kubernetes. Here's the command to disable swap:

```bash
free -h
```

After that, you need to ensure all the right ports are open. There are a lot of challenges that might close some of the ports. Here's  a checklist I created. If you find any more, I would love to add it. Be sure to reach out to me:

- Check the security configurations of your cloud provider. For example, if you're using AWS, check the security groups. If you're using GCP, check the firewall rules.
- **UFW:** In case your ubuntu system has UFW installed, run this command to disable it. If it doesn’t work, that means UFW wasn’t installed in the first place

    ```bash
    sudo ufw disable
    ```

Now, we need to install Kubeadm. This is a tool that will help us set up our cluster. It turns out that kubernetes has many different parts, each with their own configurations. Instlaling each by had is a painful and error-prone process (To the dare-devils who would like to try it, take a look at this github repo: [Kubernetes the Hard Way](https://github.com/kelseyhightower/kubernetes-the-hard-way)). That is why Kubeadm was created. It simplifies the process for us. Let's install it:
```bash
sudo apt-get update && sudo apt-get upgrade -y
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.30/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.30/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo apt update
sudo apt-get install -y kubelet kubeadm kubectl
```
You will have to run these commands on all the nodes (both master and worker).

Now it is time to initialize Master node. With Kubeadm, the process is as simple as running this command:

```bash
kubeadm init --pod-network-cidr=172.31.0.196/16
```

*Depending on your network configurations, you might want to change `--pod-network-cidr.` For most people, though, this should do the job.

Initialize worker node: At the end of the execution of the kubeadm init command, there will be a long command for you to copy. Just paste that and run it on any other computer to make it a worker node. Note that you might have to change the IP address on the command.