---
layout: page
title: "What you will need"
parent: "Module 1: Introduction"
nav_order: 3
---
# Prerequisites

There aren't a lot of prerequisites for this course, but you will need a few things to get started.

### Items
- A workstation with internet access
- Another, hopefully seperate computer that we will install Kubernetes on. You have some options when it comes to this.
    - **An old laptop that you won't mind wiping:** This is the cheapest way to get started and it doesn't work as bad as you might think. If this is the path you're going down, I recommend this NetworkChuck video that will guide you through this. [Virtual Machines Pt. 2 (Proxmox install w/ Kali Linux)
      ](https://www.youtube.com/watch?v=_u8qTN3cCnQ)
    - **A virtual machine:** I recommend using VirtualBox. Only do this if your computer has enough specs to handle this. You won't need to wipe your workstation to do this. [Download VirtualBox](https://www.virtualbox.org/wiki/Downloads)
    - **A cloud instance:** While you might shy away from this option at the beginning, a lot of this can be done for free as long as you're smart about it. For example, being a new customer for Digital Ocean or Linode gives you free credits that you can use

### Skills
- Basic understanding of the Linux command line: I do not expect you to be an expert. It's totally fine if you have to look up a command every once in a while. If you don't think you qualify for this, I suggest you watch the first couple episodes of this course: [Linux for Hackers](https://www.youtube.com/playlist?list=PLIhvC56v63IJIujb5cyE13oLuyORZpdkL). (Yes, I love Network Chuck)
  - You might want to have one master node and several worker notes. That is how most Kubernetes clusters in the real world operate. That being said, if you don't have access to that, you can use the same node as the master and a worker.
  - Also, you can drop me an email at [krishivpiduri337@gmail.com](mailto://krishivpiduri337@gmail.com) if you want me to make a similar course for Linux

That's about all you need. It's quite minimal. So, let's move on, now.