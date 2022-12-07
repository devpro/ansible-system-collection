# Rabbids Incubator Ansible Collection

[![GitLab Pipeline Status](https://gitlab.com/devpro-labs/ansible-system-collection/badges/main/pipeline.svg)](https://gitlab.com/devpro-labs/ansible-system-collection/-/pipelines)

The `rabbids_incubator.system` Ansible collection provides a set of Ansible roles and playbooks to ease system administration by automating common actions. It can be used directly or viewed as an example.

## How to use

* Look at the Ansible roles

Ansible role | Action on the host
------------ | ------------------
`docker_engine` | Install Docker engine
`kubernetes_controlplane` | Install Kubernetes control plane
`kubernetes_node` | Install Kubernetes node and join a cluster
`linux_server` | Install most commonly required Linux packages
`vagrant` | Install Vagrant
`vagrant_k8s` | Provision VM and create a Kubernetes clusters

* Review the Ansible playbooks

Ansible playbook | Action on the host
---------------- | ------------------
`kubernetes_controlplane` | Install Kubernetes control plane with kubeadm
`kubernetes_node` | Install Kubernetes node and join a cluster
`vagrant` | Install Vagrant
`vagrant_k8s` | Install Vagrant, provision VM and a create Kubernetes clusters

* Make sure Ansible is installed and an inventory is defined

* Download the collection from Artifactory and install it locally

```bash
# downloads the file
ansible-galaxy collection download https://devpro.jfrog.io/artifactory/rabbidsincubator-ansible/rabbids_incubator-system-1.0.0.tar.gz

# installs the collection
ansible-galaxy collection install collections/rabbids_incubator-system-1.0.0.tar.gz

# checks "rabbids_incubator.system" appears in the list
ansible-galaxy collection list
```

* Create a local inventory file

```ini
[lab]
myservername_or_ipaddress
```

* Run a playbook (here Vagrant playbook)

```ini
# runs a playbook
ansible-playbook rabbids_incubator.system.vagrant -i ./inventory --ask-become-pass
```

## How to contribute

* Make sure Ansible is installed

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
export ARTIFACT_FILENAME=rabbids_incubator-system-1.0.0.tar.gz

# pushes the new version of the ansible collection to artifactory
curl -u$JFROG_USERNAME:$JFROG_TOKEN -T $ARTIFACT_FILENAME "https://${JFROG_INSTANCE}/artifactory/${JFROG_REPOSITORY}/${ARTIFACT_FILENAME}"
```

## How to run locally the GitLab pipeline

```bash
mkdir -p .gitlab/runner/local
docker run --rm --name gitlab-runner -v /var/run/docker.sock:/var/run/docker.sock -v $PWD/.gitlab/runner/local/config:/etc/gitlab-runner -v $PWD:$PWD --workdir $PWD gitlab/gitlab-runner exec docker ci
```
