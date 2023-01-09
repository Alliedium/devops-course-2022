# Alliedium DevOps course 2022

## 01 DevOps introductory and virtualization 20220809

- Virtualization: types, why to use
- Hypervisors: types, which to choose
- Proxmox: how does it work
- Demo: how to create a VM on Proxmox


## 02 Containers-1 20220811

- What is a Container
- Why did Containers appear
- Containers vs Virtual Machines
- How Containers are used
- Basic Concepts


## 03 Virtualization On Windows And ZFS 20220816

ZFS (Zettabyte file system)
- History, Advantages, Limitations
- Features: Pooled Storage, Copy-on-write, Snapshots


## 04a ZFS-2 20220818

ZFS Features
- Data integrity verification and automatic repair
- RAID handling
- Deduplication, Compression

Demo


## 04b Containers-2 20220818

- Some Tools (LXC, Docker, Kubernetes, Podman)


## 05 Docker Basic Commands, PostgreSQL in Docker 20220823

- Managing images and containers
- Demo: run PostgreSQL in a container, connect via psql from host

## 06 ProxMox2 SSH backup 20220825

- LVM Disk resize
- Setting access via SSH
- Backup and Restore

## 07 Docker Best Practices I: building Docker images, PgAdmin in Docker 20220830

- Dockerfile instructions
- Image parameterization
- Example 1: Simple Spring Boot application in a container with multi-stage build
- Example 2: pgAdmin in a container

## 08 Docker Best Practices II: multistage builds, different examples 20220906

- Multistage builds
- Example 1: Simple Spring Boot application improved
- Example 2: Dockerfile of pytorch
- Example 3: Python dependencies in a separate image parameterized by hash
- Example 4: CMD vs ENTRYPOINT
- Example 5: Isolation of containers by user-defined network
- Example 6: Put ca-certificates into base image, parameterize the image by certificates hash

## 09 Lvm Storage ssh Systemd cloud-init 20220908

- LVM extend with additional PV
- Alternative to resize2fs for XFS
- Different Storage Types
- systemd: how to create your own service
- Timers for fstrim
- SSH ciphers
- Cloud-Init
- Proxmox Command Line
- Ansible
- Terraform

## 10 Network part 1 20220913

- Hosts
- IP Address, Network, Repeater, Hub, Bridge, Switch, Router
- OSI and TCP/IP model
- How packets move through a network
- Protocols

## 11 Kubernetes development basic tools 20220915

Kubernetes deployment types
- in the cloud (EKS, AKS, GKE)
- prod-like on-prem / on bare metal (k0s, k3s, rke, rke2, kubespray, kubeadm, etc.)
- local (k3s, k3d, Kind, MicroK8S, Minikube, Docker Desktop, Rancher Desktop)

- Kubernetes development tools (k3d, kubectl + krew, vscode + plugin)
- Demo: managing clusters, contexts, namespaces. Running an app in a pod.

## 12 Network part 2 20220920

- Practice (nmtui, nmcli, arp, ip route)
- DNS (Root level domain, Top level domains, Second level domains, Recursive and Caching, Forwarding)
- TLS/SSL (what is, how protects data, CA)
- Hashing, Data integrity
- Encryption (asymmetric, symmetric; algorithms, public and private keys)
- How SSL/TLS uses Cryptography


## 13 Kubernetes development basic tools (ending), Kubernetes cluster contexts 20220922

- Kubernetes development tools (code-server, k9s, openlens)
- Display Kubernetes current context in zsh command separator

## 14 Container orchestation Kubernetes basics: containers, pods, ephemeral volumes 20220927

Deployment variants of Spring Boot Application with PostgerSQL database
- Example 1. Single pod with sidecar, ephemeral volume
- Example 2. Two pods, database in ephemeral volume

## 15 From network fundamentals to SDN implementation in Proxmox part 3 20220929

- Https, Certificates (DSA,CA, Cipher Suites, HSTS, SSL certificates)
- Demo: SSL termination with self-signed certificate

## 16 Network part 4 20221004

- Demo: SSL termination with self-signed certificate (ending)

## 17 Network part 4-1 20221006

- Demo: configure in opnsense plugin nginx for load balancing
- ACME protocol
- Demo: SSL termination with public certificate, using ACME and Route53


## 18 Container orchestration Kubernetes basics 3 20221011

Deployment variants of Spring Boot Application with PostgerSQL database
- Example 3. Added services and persistence via PVC
- Example 4. ReplicaSets, Readiness and Liveness probes

## 19 Container orchestration Kubernetes basics 4 20221013

Deployment variants of Spring Boot Application with PostgerSQL database
- Example 5. Deployment, StatefulSet, ConfigMap, Secret

Backup jobs configuration variants
- Example 6. Simple job with Minio S3 storage

## 20 Container orchestration Kubernetes basics 5 20221018

Backup jobs configuration variants
- Example 7a. CronJob with Localstack AWS S3 Bucket
- Example 7b. CronJob with real AWS S3 Bucket

Installing useful tools in Kubernetes
- Example 8. Installing pgAdmin

## 21 Container orchestration Kubernetes basics 6 20221020

Using already existing Helm charts and operators
- Example 9. Installing PostgreSQL with metrics view via Grafana

## 22 VLANs with examples part 1 20221025

- Network virtualization, VLANs
- Example: Proxmox cluster with three VLANs for VMs and separate VLAN for Proxmox nodes

## 23 VLANs with examples part 2 20221027

- Example: Proxmox cluster with three VLANs for VMs and separate VLAN for Proxmox nodes (ending)
- SDN VLAN setup
- Cloud-init scripts

## 24 VLANs with examples part 3 20221101

- resolv.conf & netplan - correct network configuration setup on ubuntu
- Cloud-Init regenerate image & CLI
- SSH StrictHostKeyChecking
- Customize VMs via libguestfs-tools: virt-customize
- Example: Hyper-V with three VLANs for VMs and separate VLAN for Proxmox nodes

## 25 Container orchestration Kubernetes basics 7 (beginning) 20221101

- Operators and Custom Resource Definitions
- Adding nodes to k3d cluster
- Step 1 of Example 10. Installing scalable PostgreSQL via Kubernetes operator

## 26 Container orchestration Kubernetes basics 7 20221103

- CRDs and Custom Resources
- PostgreSQL High Availability
- How to create a Helm Chart from manifests
- Steps from 2 to 7 of Example 10. Installing scalable PostgreSQL via Kubernetes operator

## 27 Container orchestration Kubernetes basics 8 20221108

- Testing Helm chart
- Helm templates, template functions and pipelines
- Packaging Helm chart with versioning, upgrading Helm release to a new revision, rolling back to an old revision
- Installing PGAdmin4 via Helm chart
- Steps from 7 to 12 of Example 10. Installing scalable PostgreSQL via Kubernetes operator

## 28 Kubernetes, Setting up K3s prod-like: OPNsense template configuration, part 1 20221110

- Create new VM in Proxmox, install OPNsense and convert VM to template

## 29 Kubernetes, Setting up K3s prod-like: Configuring OPNsense,  creating K3s nodes, part 2 20221110

- Clone VM from opnsense-template, configuring OPNsense - adding firewall rules and setup Nginx - Load Balancer
- Create in Proxmox node VMs via Alliedium devops-course-2022 scripts
- Create VM manually in Proxmox node using cloud-init image
  
## 30 Kubernetes, Setting up K3s prod-like: Installing K3s via k3sup, part 3 20221115

- Add IP route on router or workstation for 10.10.0.0 subnet
- Install k3sup on k3s-config VM
- Install k3s cluster via k3sup, in OpenLens open k3s cluster
- Add Longhorn repository and install longhorn chart in k3s cluster

## 31 Kubernetes, Setting up K3s prod-like: WSL2, iperf3, Hyper-V, part 4 20221117

- Install WSL, chocolatey on Windows, kubectl, krew, ns, ctx, konfig, using konfig
- Kubernetes performance testing using kubernetes-iperf3
- Automatic upgrades/downgrades kubernetes cluster via k3s-Automated Upgrades
- Creating in Hyper-V vms via Hyper-V-Automation scripts, configure OPNsense network interface port as trunk

## 32 Kubernetes, Setting up k3s prod-like, still not production ready? part 5 20221122

- SLA
- High Availability
- WAF
- Certificates
- Kubernetes garbage collection
- K8s distributions
- Hardening
- Load balancers and Ingress
- Cloud Native Ecosystem
- Persistent Storage
- Backups
- Private artifacts repositories
- Autoscaling

## 33 Ansible basic concepts & simple examples, part 1 20221124

- Install ansible
- Create inventory: hosts, groups, variables
- Commands: ansible-inventory, ansible, ansible-playbook
- Ansible tasks, gathering facts
- Ansible modules: ping, debug, apt, pacman, command
- Understanding privilege escalation: become
- Ansible conditionals
- Create training environment of several VMs within Proxmox node, both using cloud-init (Ubuntu, Arch)


## 34 Ansible basic concepts & simple examples, part 2 20221129

- Create training environment of several VMs within Proxmox node, both using cloud-init (Ubuntu, Arch) & manually (Manjaro)
- Configuring and establishing SSH connection 
- Installing packages depending on host type: package, cloud_init_data_facts and stat modules
- Debugger for ansible playbooks
- Registering new variables in playbook, return values
- Variables in inventory, templates in tasks, hostname module
- Regular expressions, replace module

## 35 Nexus intro and basic config, part 1 20221201

- Learned about Nexus analogues (Amazon ECR, Harbor, Chartmuseum, local Arch linux repo)   
- Covered Installation methods: via package manager, manual, docker, kubernetes
- Backup and restore of Nexus instance
- Post-install steps: Nexus directories, anonymous access & local authorizing realm,  setup and applying cleanup policies, Cleanup tasks setup

## 36 Ansible basic concepts & simple examples, part 3 20221206

- Using group_vars and host_vars, variables' precedence
- Asking for sudo password: -K/--ask-sudo-pass argument for ansible-playbook command
- wait_for_connection module
- Command module
- Why do we need qemu-guest-agent
- What is snapd and why we get rid of it
- What is a role in Ansible
- Installing a role from ansible-galaxy (GitHub, GitLab)
- Installing multiple roles using requirements.yml
- Using roles in playbooks
- What is an NTP server
- Converting bash scripts into ansible playbooks
- Editing sudoers
- Package module
- What is yay

## 37 Nexus Docker and Helm repo config, part 2 20221208

- How to run Nexus in as Docker container without docker-compose
- Which scheduled tasks should be setup in Nexus initially
- How to setup properly Docker Proxy repository in Nexus
- What does HTTP connector means for Docker repositories in Nexus
- How to setup properly Docker Hosted repository in Nexus and what it is used for
- How to setup properly Docker Group repository in Nexus and what it is used for
- How to setup Docker tool in local system to interact with Nexus 
- How to create user roles and assign them to users in order to give limited access for contributing into particular Nexus repositories
- How to Pull from Docker-group repository and Push to Docker-hosted repository in Nexus with examples
- How to check Nexus logs (if it's up in Docker)
- How to inspect and cleanup system via ncdu
- How to setup Kubernetes cluster to refer to Nexus docker images (k3s configuration, k3d configuration)
- How to interact with containerd images via crictl
- How to setup properly Helm Proxy repository in Nexus and what it is used for
- How to setup properly Helm Hosted repository in Nexus and what it is used for
- How to use nelm-nexus-push addon for Nexus to push Helm charts avoiding curl

## 38 Automation of K3s cluster provisioning via Ansible: VyOS intro, part 1 20221213

- Installing and configuring VyOS VM manually
  
## 39 Automation of K3s cluster provisioning via Ansible: VyOS configuration, K3s Ansible project, part 2 20221214

- Installing and configuring VyOS VM manually
- Demonstration of firewall ruleset in Linux using nft command line tool
- Creating of VyOS cloud-init image via Ansible playbook
- Creating of configured VyOS VM on Proxmox node using Ansible playbook

## 40 Automation of K3s cluster provisioning via Ansible: kube-vip, MetalLB, K3s Ansible project, part 3 20221215

- Changing of VyOS firewall configuration
- What is kube-vip and how it works
- What is MetalLB and how it works
- Installing K3s cluster via Ansible playbook (including kube-vip and MetalLB)
- Creating pod and services to expose pod
- Changing MetalLB IP address range in manifest in OpenLens

## 41 Nexus: Maven, Gradle, Conda, pip, poetry, APT, Ansible repo config, part 3 20221216

- How to setup Maven Proxy, Maven Hosted and Maven Group repositories in Nexus
- How to configure settings.xml and pom.xml so that Maven can refer to Nexus repositories
- How to configure Gradle to use Nexus Maven repositories
- How to setup Conda Proxy repository in Nexus
- What is Conda environments, how to use them
- How to configure Conda-based tools (e.g. Micromamba) to use Nexus Proxy repository
- How to setup npm Proxy, npm Hosted and npm Group repositories in Nexus
- How to configure npm client to use Nexus - pull and push examples
- How to workaround npm audit calls
- How to setup PyPI Proxy, PyPI Hosted and PyPI Group repositories in Nexus
- How to configure pip (and other tools e.g. twine) to use Nexus
- How to package Python project before pushing
- What is Realworld projects
- How to setup APT Proxy and APT Hosted repositories in Nexus
- How to configure client side to use APT repositories from Nexus
- How to add Ansible format to list of supported formats in Nexus
- How to add S3 blobstore to Nexus

## 42 Ansible: advanced concepts, part 4 20221220

- Manjaro basic bash scripts
- Making Ansible playbooks working more stable: wait_for_connection, stat modules
- Collecting facts about packages: package_facts module
- What is the Sudoers File and how to modify it via Ansible: sudoers module
- What is AUR, pacman and yay, how to install and use yay on Arch Linux, what is pamac
- Task conditional expressions - Jinja Tests
- How to remove files and directories in Ansible: file module
- Task loops
- Understanding privilege escalation: become
- How to avoid entering sudo password, sudoers syntax
- Ansible roles
- What is Ansible collections, aur - collection including an Ansible module to manage packages from the AUR
- Avoiding copy-pasting I - defining variables in playbook runtime: set_fact module
- Playbook handlers, the execution order of tasks, roles and handlers
- Defining default values for variables, once again variable precedence
- How to run a role multiple times in one play
- Avoiding copy-pasting II - using blocks
- Useful Ansible modules: git, replace, lineinfile, ansible.posix.sysctl
- Makepkg - utilizing multiple cores on compression
- Templating (Jinja2), using filters to manipulate data
- How to install and configure latest version of Ansible on Ubuntu Linux
- Ansible plays
- Task tags
- Testing Ansible playbooks

## 43 Ansible: advanced concepts, part 5 20221222

- Install multiple collections with a requirements file
- Useful Ansible modules I: template, systemd
- Using roles at the play level, pre_tasks and post_tasks
- Using Ansible roles within tasks: include and import
- Manjaro basic bash scripts for users executed by sysadmin and by users
- Managing users and groups in Ansible: getent and user modules
- Difference between include and import for roles: looping for include, attribute inheritance for import
- Becoming a different user in Ansible
- Userful Ansible modules II: git_config, file, copy, find, get_url modules
- Ansible linter
- Testing Ansible playbooks and roles: Molecule
