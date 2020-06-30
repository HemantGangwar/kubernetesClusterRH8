INTRODUCTION
=============

This playbook is intended to install and configure kubernets 1.17 Cluster on RHEL/CentOS 7,8.

Read this document carefully to have more understanding about this ansible playbook.


VARIABLES DECLARATION
-----------------------

Variables need to declared inside deploy_kubernetes/defaults/main.yml

1. KUBERNETES_MASTER => The node fqdn or hostname who will act as Kubernetes Cluster master.

2. MASTER_IP_ADDRESS => IP address of the above kubernetes master 

3. Other variables declared in deploy_kubernetes/vars/main.yml

INVENTORY SETUP
================

One can use it's own inventory or create it's own inventory.
Replace the hostnames in "inventory" file present here with hostnames of your environment. You can leave [kub] as it is if you are not making any changes in playbook.

USAGE
------------------------
1. Download this playbook in your ansible server.

# git clone https://github.com/HemantGangwar/kubernetesCluster.git

2. Update deploy_kubernetes/defaults/main.yml with required parameter (example entry below)

KUBERNETES_MASTER: nodea.lab.example.com

Update your node name and IP which you want to use as kubernetes master.

3. Update inventory file provided here with your node names.

4. kubernetes.yml is the main playbook, you can execute it using

$ ansible-playbook kubernetes.yml 

OR

$ ansible-playbook kubernetes.yml -e KUBERNETES_MASTER=nodea.lab.example.com

5. The playbook is fully idempotent and can be safely used multiple times.

It contains 3 segments:

1. Pre-Requsisites taking care of Run time disabling of selinux and switching off swap.
2. A role named deploy_kubernetes containing master and worker deployment.
3. Post-Requisite taking care of setting up Networking(weave) for Cluster. 

Execution of playbook can be viewed at : https://www.youtube.com/watch?v=Lns4th54zPM&t=106s

AUTHOR
========
This playbook is written by Hemant Gangwar, you can read more about author at https://learningtechnix.wordpress.com or follow on LinkedIN https://www.linkedin.com/in/hemant-gangwar-6a677b19/

BUG FIXES
=========
1. In version 30 June 2020 fixed an issue with playbook requiring static master name while joining Kubernetes nodes
