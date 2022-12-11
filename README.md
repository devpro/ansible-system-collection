# Ansible System Collection

[![GitLab Pipeline Status](https://gitlab.com/devpro-labs/ansible-system-collection/badges/main/pipeline.svg)](https://gitlab.com/devpro-labs/ansible-system-collection/-/pipelines)

Ansible `devpro.system` collection provides a set of Ansible roles and playbooks to ease system administration by automating common actions. It can be used directly or viewed as an example.

## How to use

* Look at Ansible roles

Ansible role              | Action on the host
--------------------------|----------------------------------------------
`docker_engine`           | Install Docker engine
`kubernetes_controlplane` | Install Kubernetes control plane
`kubernetes_node`         | Install Kubernetes node and join a cluster
`linux_server`            | Install most commonly required Linux packages
`vagrant`                 | Install Vagrant
`vagrant_k8s`             | Provision VM and create a Kubernetes clusters

* Review Ansible playbooks

Ansible playbook          | Action on the host
--------------------------|---------------------------------------------------------------
`kubernetes_controlplane` | Install Kubernetes control plane with kubeadm
`kubernetes_node`         | Install Kubernetes node and join a cluster
`vagrant`                 | Install Vagrant
`vagrant_k8s`             | Install Vagrant, provision VM and a create Kubernetes clusters

* Make sure [Ansible is installed](https://docs.ansible.com/ansible/latest/installation_guide/index.html) and an inventory is defined

* Download the collection from Artifactory and install it locally

```bash
# downloads the file
ansible-galaxy collection download https://devpro.jfrog.io/artifactory/devpro-ansible/devpro-system-1.0.0.tar.gz

# installs the collection
ansible-galaxy collection install collections/devpro-system-1.0.0.tar.gz

# checks "devpro.system" appears in the list
ansible-galaxy collection list
```

* Create a local inventory file

```ini
[lab]
myservername_or_ipaddress
```

* Run a playbook (here Vagrant playbook)

```ini
ansible-playbook devpro.system.vagrant -i ./inventory --ask-become-pass
```

## How to contribute

* Make sure [Ansible is installed](https://docs.ansible.com/ansible/latest/installation_guide/index.html)

* Create local inventories for the tests

* Validate a code change by executing a playbook (use one or create a new one)

```bash
# checks the hosts can be reached ok
ansible all -m ping -i inventories/lab01

# run a playbook
ansible-playbook -i inventories/lab01 playbooks/demo.yml --ask-become-pass
```

* Publish the collection

```bash
ansible-galaxy collection build
```

* Push to Artifactory

```bash
# sets jfrog environment
export JFROG_INSTANCE=devpro.jfrog.io
export JFROG_REPOSITORY=rabbidsincubator-ansible

# sets jfrog authentication credentials
export JFROG_USERNAME=
export JFROG_TOKEN=

# sets artifact information
export ARTIFACT_FILENAME=devpro-system-1.0.0.tar.gz

# pushes the new version of the ansible collection to artifactory
curl -u$JFROG_USERNAME:$JFROG_TOKEN -T $ARTIFACT_FILENAME "https://${JFROG_INSTANCE}/artifactory/${JFROG_REPOSITORY}/${ARTIFACT_FILENAME}"
```

* Lint Ansible files

```bash
# https://hub.docker.com/r/pipelinecomponents/ansible-lint
docker run -it --rm --name ansible-lint -v $PWD:$PWD --workdir $PWD pipelinecomponents/ansible-lint
```

## How to run locally the GitLab pipeline

```bash
mkdir -p .gitlab/runner/local
docker run --rm --name gitlab-runner -v /var/run/docker.sock:/var/run/docker.sock -v $PWD/.gitlab/runner/local/config:/etc/gitlab-runner -v $PWD:$PWD --workdir $PWD gitlab/gitlab-runner exec docker ci
```

## References

* [Kubernetes Setup Using Ansible and Vagrant](https://kubernetes.io/blog/2019/03/15/kubernetes-setup-using-ansible-and-vagrant/) - March 15, 2019
* [Ansible Collection structure](https://docs.ansible.com/ansible/latest/dev_guide/developing_collections_structure.html)
* [Ansible MongoDB Communication Collection](https://github.com/ansible-collections/community.mongodb)
* [Deploy a Kubernetes cluster using Ansible](https://buildvirtual.net/deploy-a-kubernetes-cluster-using-ansible/)
* [MatTerra/ansiblekubernetes](https://github.com/MatTerra/ansiblekubernetes)
* [kairen/kube-ansible](https://github.com/kairen/kube-ansible)
* [How to Install Containerd Container Runtime on Ubuntu 22.04](https://www.howtoforge.com/how-to-install-containerd-container-runtime-on-ubuntu-22-04/)
* [How to install cri-dockerd and migrate nodes from dockershim](https://www.mirantis.com/blog/how-to-install-cri-dockerd-and-migrate-nodes-from-dockershim/) - July 14, 2022
* [How to Install CRI-O on Ubuntu 22.04 / Ubuntu 20.04](https://www.itzgeek.com/how-tos/linux/ubuntu-how-tos/install-cri-o-on-ubuntu-22-04.html) - Apr 30, 2022
* [Deploying Kubernetes Cluster on Azure VMs using kubeadm, CNI and containerd](https://techcommunity.microsoft.com/t5/azure-developer-community-blog/deploying-kubernetes-cluster-on-azure-vms-using-kubeadm-cni-and/ba-p/3690976) - December 06, 2022
* [Installing and Configuring containerd as a Kubernetes Container Runtime](https://www.nocentino.com/posts/2021-12-27-installing-and-configuring-containerd-as-a-kubernetes-container-runtime/) - 2021-12-27
