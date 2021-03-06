# Ansible Installation

### Pre-requisites

1. An AWS EC2 instance (on Control node)

### Installation steps:
#### on Amazon EC2 instance

1. Validate python, python3, pip --version, if are missing... Install them
   ```sh
   yum install python
   yum install python-pip
   ```
1. This installation method failed for me...Install ansible using pip check for version 
    ```sh
    pip install ansible
   ansible --version
   ```
1. Update the EC2 Amazon Linux Box
    ```sh
    $ sudo yum update -y 
    ```
    
1. Install Ansible this way Worked!
    ```sh
    $ amazon-linux-extras install ansible2
    ```

1. Create a user called ansadmin (on Control node and Managed node)  
   ```sh
   useradd ansadmin
   passwd ansadmin
   ```
1. Grant sudo access to ansadmin user, to execute any cmd without any password (On Control and Managed nodes) 
   ```sh
   echo "ansadmin ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers
   ```

1. Enable password authentication (All nodes) /etc/ssh/sshd.config
   ```sh
   PasswordAuthentication yes     -> (uncomment)

1. On Managed node, /root/.ssh/authorized_keys remove the keypair for ec2-user 


1. Reload ssh service (Both nodes)
    ```sh
    service sshd status --> reload
    ```
   
1. Log in as a ansadmin user on Control node and generate ssh key 
   ```sh 
   sudo su - ansadmin
   ssh-keygen
   ```
   
1. Copy Public keys (id-rsa.pub) into all ansible managed nodes (on Control node) Logged as ansadmin,
   ```sh 
   ssh-copy-id ansadmin@<target-server>
   ```
   Validation: Logging into the Managed node
   ```sh
   ssh ansadmin@172.31.28.186
   ```
   The public key must be copied on /home/ansadmin/.ssh/authorized_keys (Managed node)
   
1. Ansible Inventory setup  --> cd /etc/ansible/hosts
   ```sh
   172.xx.xx.xxx

### Test Ansible 
   ```sh 
   ansible all -m ping
   172.31.28.186 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    }, 
    "changed": false, 
    "ping": "pong"
   }
   ```

   
