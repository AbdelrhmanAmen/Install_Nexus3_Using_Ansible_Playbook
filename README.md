# Install_Nexus3_Using_Ansible_Playbook

## Overview
> Install Latest Sonatype Nexus 3 on CentOS 8 using ansible 
> [Sonatype Nexus3](https://www.sonatype.com/products/nexus-repository) is an open-source repository manager used to manage and organize software components, build artifacts, and third-party dependencies. It is widely used in DevOps and software development workflows to streamline the software development lifecycle.

---

## Requirement
* Ansible master node
  * Install python3 `sudo yum install python3`
  * Install EPEL Repository `yum install epel-release`
  * Install ansible `sudo yum install ansible`
  * Set Passwordless authentication between master and remote hosts `ssh-keygen` , `ssh-copy-id root@IP_address`
  * Add inventory file 
    ```
    [hosts]
    nexus ansible_host= <remote host IP or FQDN> ansible_ssh_private_key_file= <path to private key>
    ```
* Nexus remote node 
  * Minimum 1 VCPU & 2 GB Memory.
  * Install OpenJDK 8.
  * Server firewall opened for port 22 & 8081
  * All Nexus processes should run as a non-root nexus user
  
 ---
 ## main project hierarchy
```
   project
   ├── nexus3
   │   ├── tasks
   |   ├── templates
   |   ├── meta
   │   └── vars
   ├── prerequisite
   │   ├── tasks
   │   └── vars
   ├── inventory.ini
   ├── keys
   └── playbook.yaml
```
 
 ## The Automated Logic
 * Update the `yum` packages
 * Install  `chkconfig`,`openjdk`
 * allow firewall ports `22 and 8081`
 * Create a directory for nexus called `app`
 * Download the latest nexus in the app directory
 * Untar the downloaded file
 * Rename the untared file to `nexus`.
 * Create a service user named `nexus`.
 * Change the ownership of nexus files to `nexus` user
 * Create a nexus systemd unit file `/etc/systemd/system/nexus.service`
 * Start and enable nexus service

---
## Playbook Run
* **`ansible-playbook -i <inventory file path> <playbook file>`**
* hit `http://localhost:8081` in the browser
#### Now you see the nexus homepage, take a tour  
![nexus](https://user-images.githubusercontent.com/73068684/231871738-43094217-c9da-4f35-9348-22378db2ae27.PNG)
* Log in using default username `admin ` and password found in `app/sonatype-work/nexus3/admin.password` 
  >  for ease when running the play book there would be a task to get the password for you

