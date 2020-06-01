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

PLAYBOOK EXECUTION
-------------------

kubernetes.yml is the main playbook, you can execute it using

$ ansible-playbook kubernetes.yml 

OR

$ ansible-playbook kubernetes.yml -e KUBERNETES_MASTER=nodea.lab.example.com

It contains 3 segments:

1. Pre-Requsisites taking care of Run time disabling of selinux and switching off swap.
2. A role named deploy_kubernetes containing master and worker deployment.
3. Post-Requisite taking care of setting up Networking(weave) for Cluster. 

The playbook is fully idempotent and can be safely used multiple times.

$ cat kubernetes.yml

 -  name: ------------ | Kuberenetes Cluster Deployment | -------------------
   hosts: kub
   become: all
   pre_tasks:
  - name: Pre-Requisite-1 |Switching off swap|
    shell: swapoff -a
    when: ansible_swaptotal_mb > 0
  - name: Pre-Requisite-2 |Gathering selinux status|
    shell: getenforce | grep -i Enforcing | wc -l
    register: se_status
  - name: Pre-Requisite-3 |Switching off selinux|
    shell: setenforce 0
    when: se_status.stdout == '1'

  roles:
  - deploy_kubernetes

  post_tasks:
  - name: Post-Requisite Configuring |weave| network
    shell: kubectl apply -f https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n') >> /tmp/weave_network.txt
    args:
      creates: /tmp/weave_network.txt
    when: ansible_fqdn == '>>> Enter your kubernetes master name here <<<'


AUTHOR
========
This playbook is written by Hemant Gangwar, you can read more about author at https://learningtechnix.wordpress.com or follow on LinkedIN https://www.linkedin.com/in/hemant-gangwar-6a677b19/
