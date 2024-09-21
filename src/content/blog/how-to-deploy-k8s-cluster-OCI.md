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

  

Now you do need a free tier OCI account and upgrade it to a Pay as you go (PAYG) account due to the NAT not being included in the free tier.

  

I do recommend setting up a budget in the `Billing & Cost Management`->`Cost Management`->`Budgets`.

  

Setting up a new budget with the minimun value lets you track if anything goes wrong in your deployment and you can avoid paying anything!

![budget](@assets/images/oci-budget.png)

Now we can start to setup our cluster!  

1. Head on to `Kubernetes Clusters (OKE)` and `Create cluster`.
 
2. Click on `Quick create`. 

3. Use a `Public endpoint` so that we can expose your cluster to the internet.

4. Select `Node type` as `Managed` so that we can ony use our free tier resources and not incur any costs.

5. In `Shape and image` select `VM.Standard.A1.Flex` and you can choose the shape as:
	1. 1 OCPU and 6 GB Memory and 4 Node count.
	2. 2 OCPU and 12 GB Memory and 2 Node count.
6. By default the boot size is 46.6 GB, you can increase this to 50 GB in the advanced options to fully use the 200 GB free tier storage.
7. Select next and be sure to select the `Create a Basic cluster` as the Enhanced cluster incur costs each day.

It will take around 10 mins for all your nodes to come online in the nodepool and then your free kubernetes cluster will be active!

You can also check your cluster nodes in the `Instances` section:
 ![oci-instances](@assets/images/oci-instances.png)

Now you do need to install OCI on your laptop to access your cluster.

 1. You need to first run this command and use the defaults or whatever parameters you want
 ```bash
bash -c "$(curl -L https://raw.githubusercontent.com/oracle/oci-cli/master/scripts/install/install.sh)"
```
2. You do need to configure your OCI setup, follow the link for more instructions.
	[Quickstart (oracle.com)](https://docs.oracle.com/en-us/iaas/Content/API/SDKDocs/cliinstall.htm#InstallingCLI__PromptsInstall)

3. Once done, you can access your cluster using the command provided in your OCI console
	It should look like this 
```bash
oci ce cluster create-kubeconfig --cluster-id ocid1.cluster.oc1.ap-mumbai-1.aaaaa --file $HOME/.kube/config --region ap-mumbai-1 --token-version 2.0.0  --kube-endpoint PUBLIC_ENDPOINT
```
4. Now you need to export the KUBECONFIG to enable your kubectl use the configs
  ```bash
  export KUBECONFIG=$HOME/.kube/config
  ```

Now you can check your nodes!
  ```bash
  kubectl get nodes
  ```
![oci-k8s-cluster-nodes](@assets/images/oci-nodes.png)