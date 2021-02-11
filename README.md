# photon-k8-cluster
Install Kubernetes Cluster on Photon VMs

# Preparation
* Get the repository - ```git clone https://github.com/shahkamran/photon-k8-cluster.git```
* Download Photon OS from https://github.com/vmware/photon/wiki/Downloading-Photon-OS

# Build Infrastructure Machines
* Install 1 x Master VM - photon-master
* Install 1 or more Node VMs - photon-node1 and so on

# Configure Infrastructure Machines
* Log-in as root from console to each server installed with Photon OS and perform below tasks.
* Enable SSH on all photon servers.
```vim /etc/ssh/sshd_config```
* Find below line and uncomment by removing the hash "#" in front of the line.
```#PermitRootLogin prohibit-password```
* Check if you have a ssh key set up on your computer ```cat ~/.ssh/id_rsa.pub ```.
* If no key is set up, run ```ssh-keygen -t rsa``` to set one up.
* Take a copy the key output from ```cat ~/.ssh/id_rsa.pub ```
* On each Photon VM created, perform below tasks
```touch ~/.ssh/authorized_keys```
```chmod 700 ~/.ssh; chmod 640 ~/.ssh/authorized_keys```
* Add the copy of key to this file ```vim ~/.ssh/authorized_keys```
* You should now be able to log in without passwrd using ```ssh root@photon-master```.

# Optional
* Install and set up PMD
```tdnf install pmd pmd-cli
systemctl start pmd.service
```

# Define Inventory for the playbook scope
* Update Inventory with Photon VM details in ```hosts``` file, as as example.
```
# hosts
[kubernetes_cluster]
192.168.1.1
192.168.1.2

[masters]
192.168.1.1

[minions]
192.168.1.2

[kubernetes_cluster:vars]
master_hostname=photon-master
master_ip=192.168.1.1
minion_hostname=photon-node
minion_ip=192.168.1.2
```

# Run the Ansible YAML Playbook to build Kubernetes Cluster
* Run the following from the repository location where both hosts and kubernetes_cluster.yml are located.
```ansible-playbook -i hosts --user root  kubernetes_cluster.yml```

*Check your cluster status from Master node.
```ssh root@photon-master```
```kubectl get nodes```


