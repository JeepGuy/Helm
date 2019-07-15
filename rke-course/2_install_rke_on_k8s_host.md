2_create-k8s-cluster

    * If wget not installed: sudo yum install wget -y
    * If vim not installed: sudo yum install vim -y

Make executible

chmod +x rke-linux-amd64
or
chmod 0755 rke-linux-amd64

wget https://github.com/rancher/rke/releases/download/v0.2.5-rc3/rke_linux-amd64


./rke_linux-amd64 config
```
WARN[0000] This is not an officially supported version (v0.2.5-rc3) of RKE. Please download the latest official release at https://github.com/rancher/rke/releases/latest 

[+] Cluster Level SSH Private Key Path [~/.ssh/id_rsa]: 
[+] Number of Hosts [1]: 1
[+] SSH Address of host (1) [none]: <copy your host addr from ec2 interface>
[+] SSH Port of host (1) [22]: 22
[+] SSH Private Key Path of host (3.85.233.126) [none]: id_rsa
[+] SSH User of host (3.85.233.126) [ubuntu]: ec2-user
[+] Is host (3.85.233.126) a Control Plane host (y/n)? [y]: y
[+] Is host (3.85.233.126) a Worker host (y/n)? [n]: y 
[+] Is host (3.85.233.126) an etcd host (y/n)? [n]: y
[+] Override Hostname of host (3.85.233.126) [none]: 
[+] Internal IP of host (3.85.233.126) [none]: 
[+] Docker socket path on host (3.85.233.126) [/var/run/docker.sock]: 
[+] Network Plugin Type (flannel, calico, weave, canal) [canal]: 
[+] Authentication Strategy [x509]: 
[+] Authorization Mode (rbac, none) [rbac]: 
[+] Kubernetes Docker image [rancher/hyperkube:v1.14.3-rancher1]: 
[+] Cluster domain [cluster.local]: 
[+] Service Cluster IP Range [10.43.0.0/16]: 
[+] Enable PodSecurityPolicy [n]: 
[+] Cluster Network CIDR [10.42.0.0/16]: 
[+] Cluster DNS Service IP [10.43.0.10]: 
[+] Add addon manifest URLs or YAML files [no]:
```
vim cluster.yaml

Remove the docker socket - caused a problem in a previous version 1.9.
Error: docker.socket not found
 13   docker_socket: /var/run/docker.sock

Remove the ALL 4 (four)  SSH search paths beacuse the ssh-agent key to access the cluster.

 13   docker_socket: /var/run/docker.sock
 13   ssh_key: ""
 14   ssh_key_path: id_rsa


Now copy your SSH key into the host.
id_rsa 

---

chg the perms to 400 ( or 600 both work)

start ssh agent and add id_rsa key


eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa


---

ERRORS:

    Can't retrieve Docker Info: error during connect: Get http://%2Fvar%2Frun%2Fdocker.sock/v1.24/info: Unable to access node with address [35.174.167.250:22] using SSH. Please check if the configured key or specified key file is a valid SSH Private Key. Error: Error configuring SSH: ssh: no key found 

    ssh-add ~/.ssh/id_rsa 
    Identity added: /home/ec2-user/.ssh/id_rsa (/home/ec2-user/.ssh/id_rsa)

    eval$(ssh-agent)
    -bash: evalSSH_AUTH_SOCK=/tmp/ssh-PfiL08KkzMzL/agent.31886;: No such file or directory
    [ec2-user@ip-172-31-85-223 .ssh]$ eval`ssh-agent`

    CORRECT WAY:

    eval "$(ssh-agent -s)"





    [ec2-user@ip-172-31-85-223 ~]$ ./rke_linux-amd64 up 
    WARN[0000] This is not an officially supported version (v0.2.5-rc3) of RKE. Please download the latest official release at https://github.com/rancher/rke/releases/latest 
    INFO[0000] Initiating Kubernetes cluster                
    INFO[0000] [certificates] Generating admin certificates and kubeconfig 
    INFO[0000] Successfully Deployed state file at [./cluster.rkestate] 
    INFO[0000] Building Kubernetes cluster                  
    INFO[0000] [dialer] Setup tunnel for host [35.174.167.250] 
    WARN[0000] Failed to set up SSH tunneling for host [35.174.167.250]: Can't retrieve Docker Info: error during connect: Get http://%2Fvar%2Frun%2Fdocker.sock/v1.24/info: Unable to access node with address [35.174.167.250:22] using SSH. Please check if the configured key or specified key file is a valid SSH Private Key. Error: Error configuring SSH: ssh: no key found 
    WARN[0000] Removing host [35.174.167.250] from node lists 
    FATA[0000] Cluster must have at least one etcd plane host: failed to connect to the following etcd host(s) [35.174.167.250]


    lease check network policies and firewall rules]

### Had to change the security group to allow all traffic from anywhere.
#### On second attempt added the following custom rule to the Rancher security group.

* Open port 2379 for TCP on EC2 this will take some research.

        Custom TCP Rule 

        TCP
            
        2379
            
        0.0.0.0/0
            
        Install port for k8s


## Install Kubectl binary

mv /etc/yum.repos.d/kubernetes.repo /etc/yum.repos.d/kubernetes.repo.bak
cat >> /etc/yum.repos.d/kubernetes.repo <<EOF
[kubernetes]
name=Kubernetes
baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64/
enabled=1
gpgcheck=0
EOF

sudo yum install -y kubectl


Without having the .kube/config set, when using kubectl pass the argument:

kubectl --kubeconfig kube_config_cluster.yml <command>

Or set the KUBECONFIG environment variable.

export KUBECONFIG=kube_config_cluster.yml

OR:

```



kubectl config merge command is not yet available. But you can achieve a config merge by running:

Command format:

KUBECONFIG=config1:config2 kubectl config view --flatten

Example:

Merge a config to ~/.kube/config and write back to ~/.kube/config.

KUBECONFIG=~/.kube/config:/path/to/another/config.yml kubectl config view --flatten > ~/.kube/config

OR: 
If kubectl can read that as a valid config file, you can just use that as your kubeconfig. 
by:    cp kube_config_cluster.yaml $HOME/.kube/config 
- and it should work fine.
 From there it'll read that config file by default and you won't have to specify it.

```










