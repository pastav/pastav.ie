---
title: Free K8s cluster using Oracle Cloud
author: Pastav
pubDatetime: 2024-09-15T11:20:05Z
slug: deploy-free-k8s-cluster-OCI
featured: true
draft: false
tags:
  - kubernetes
  - linux
  - OCI
ogImage: ""
description: Steps on how to deploy a free K8s custer using Oracle Cloud
---
Now assuming that you have an OCI free tier account,


Deploy two VM.Standard.A1.Flex servers with 12GB of RAM each and two VM.Standard.E2.1.Micro with 1GB of RAM each so that your limit is not reached.

![servers](@assets/images/oci-servers.png)

I will be using the 2 ARM servers as the control plane and the 2 Micro servers as workers.


