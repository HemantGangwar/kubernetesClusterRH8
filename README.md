![image](https://user-images.githubusercontent.com/38517925/86527948-5f983e80-bec1-11ea-9be7-03a6cc7792c8.png)

# INTRODUCTION
---------------

This playbook is intended to install and configure *KUBERNETES Cluster* on RHEL/CentOS 7,8 by **Kubeadm**.

**Note**: With RHEL OS you may face issues while installing docker lastest version if the OS is itself on old version. 

Read this document carefully to have more understanding about this ansible playbook.

![image](https://user-images.githubusercontent.com/38517925/86524357-5abe9500-be97-11ea-8f15-d997b4ce7d3e.png)

## RESOURCE REQUIREMENTS
The playbook is intended to run on VM's with *atleast* **2 cores** and **2GB RAM** allocated. So please make sure, we have this much amount of resources available, else the playbook will fail. 

## VARIABLES DECLARATION
-----------------------

Variables need to declared inside deploy_kubernetes/defaults/main.yml

1. KUBERNETES_MASTER => The node fqdn or hostname who will act as Kubernetes Cluster master.

2. MASTER_IP_ADDRESS => IP address of the above kubernetes master 

3. CRIOVERSION => Required CRIO repository version

4. Other variables declared in deploy_kubernetes/vars/main.yml

###### PRE-REQUISITES

- Basic knowledge of Linux, Git.
- Base OS repository should be configured.
- Internet connectivity of VM's (or all required respoitory configured offline)
- Communication already working between ansible control node and managed nodes.

###### INVENTORY SETUP

One can use it's own inventory or create it's own inventory.
Replace the hostnames in "inventory" file present here with hostnames of your environment. 

# USAGE
------------------------

Step | Description | Commands
------ | ----------- | --------
1 | Download this playbook in your ansible server | # git clone https://github.com/HemantGangwar/kubernetesClusterRH8.git
2 | Enter into the directory created | # cd kubernetesClusterRH8
3 | Update inventory file provided here with your node names. | # vi inventory
4 | Update deploy_kubernetes/defaults/main.yml with required parameter | (example) KUBERNETES_MASTER: master.lab.example.com
5 | Now execute the playbook | # ansible-playbook kubernetes.yml OR ansible-playbook kubernetes.yml -e KUBERNETES_MASTER=master.lab.example.com


Note:  *The playbook is fully idempotent and can be safely used multiple times.*

It contains 3 segments:

1. Pre-Requsisites taking care of Run time disabling of selinux and switching off swap.
2. A role named deploy_kubernetes containing master and worker deployment.
3. Post-Requisite taking care of setting up Networking(weave) for Cluster. 

Execution of playbook can be viewed on https://youtu.be/x7l7Ne2jvsw

![image](https://user-images.githubusercontent.com/38517925/177979974-9521973f-f3de-430a-810b-65c169b4301f.png)
![image](https://user-images.githubusercontent.com/38517925/177980143-d6d2a6bb-2314-4e04-9e50-9b446f5874c6.png)
![image](https://user-images.githubusercontent.com/38517925/177980217-8e310838-0841-4c7f-afb4-ed1863908d00.png)
![image](https://user-images.githubusercontent.com/38517925/177980304-4d566c44-92c8-4d25-a09f-8f750834fb8c.png)
![image](https://user-images.githubusercontent.com/38517925/177980386-7be5141d-22a5-4179-b411-1b4f163e0512.png)
![image](https://user-images.githubusercontent.com/38517925/177980522-4188b252-31c9-4d5c-b7cf-b26d23cfee4f.png)
![image](https://user-images.githubusercontent.com/38517925/177980575-50233602-bb99-4c38-8742-610d2e86904b.png)
![image](https://user-images.githubusercontent.com/38517925/177981041-ff4ae712-8c0e-4d06-91e8-f2470b37ca10.png)
![image](https://user-images.githubusercontent.com/38517925/177981058-e8f836e2-d570-48a1-afb7-8421d3e196dd.png)
![image](https://user-images.githubusercontent.com/38517925/177981098-59b7abe7-68b3-4854-9cb9-73914a82675d.png)



AUTHOR
--------
This playbook is written by Hemant Gangwar, you can read more about author at https://learningtechnix.wordpress.com or follow on LinkedIN https://www.linkedin.com/in/hemant-gangwar-6a677b19/

BUG FIXES
-----------
1. In version 30 June 2020 fixed an issue with playbook requiring static master name while joining Kubernetes nodes
2. In version 10 July 2020 fixed issue of deprecated, package installation warning. And updated Inventory description.
3. In version 5 Aug 2020 fixed dependency issue used in case of RedHat OS.
4. In version 8 July 2022, it is updated to incorporate CRIO deployment as well.
