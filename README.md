# Ansible-kubeadm
### Steps
###### Step 1
```
1- Create a host file and add ips of master and worker node
2-  
```
###### Step 2
```
Then run initial.yml file
This will create following:
Creates the non-root user ubuntu.
    1- Configures the sudoers file to allow the ubuntu user to run sudo commands without a password prompt.
    2- Adds the public key in your local machine (usually ~/.ssh/id_rsa.pub) to the remote ubuntu user’s authorized key list. This will allow you to SSH into each server as the ubuntu user.
```
###### Step 3
```
Run kube-dependencies.yml
This will create following:
   1- Installs Docker, the container runtime.
   2- Installs apt-transport-https, allowing you to add external HTTPS sources to your APT sources list.
   3- Adds the Kubernetes APT repository’s apt-key for key verification.
   4- Adds the Kubernetes APT repository to your remote servers’ APT sources list.
   5- Installs kubelet and kubeadm.

```
###### Step 4
```
Run master.yml
This will create following:
    1- The first task initializes the cluster by running kubeadm init. Passing the argument --pod-network-cidr=10.244.0.0/16 specifies the private subnet that the pod IPs will be assigned from. Flannel uses the above subnet by default; we’re telling kubeadm to use the same subnet.
    2- The second task creates a .kube directory at /home/ubuntu. This directory will hold configuration information such as the admin key files, which are required to connect to the cluster, and the cluster’s API address.
    The third task copies the /etc/kubernetes/admin.conf file that was generated from kubeadm init to your non-root user’s home directory. This will allow you to use kubectl to access the newly-created cluster.
    3- The last task runs kubectl apply to install weave. kubectl apply -f descriptor.[yml|json] is the syntax for telling kubectl to create the objects described in the descriptor.[yml|json] file. This is a pod cluster network plugin which can talk between pods
```
###### Step 5
```
Run worker.yml
This will create following:

    1- The first play gets the join command that needs to be run on the worker nodes. This command will be in the following format:kubeadm join --token <token> <master-ip>:<master-port> --discovery-token-ca-cert-hash sha256:<hash>. Once it gets the actual command with the proper token and hash values, the task sets it as a fact so that the next play will be able to access that info.
    2- The second play has a single task that runs the join command on all worker nodes. On completion of this task, the two worker nodes will be part of the cluster.
```