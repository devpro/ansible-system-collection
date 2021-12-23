# Rabbids Incubator Ansible Collection

The `rabbids_incubator.system` Ansible collection provides a set of Ansible roles and playbooks to ease system administration by automating common actions.

## How to use

* Look at the Ansible roles

Ansible role | Action on the host
------------ | ------------------
`docker_engine` | Install Docker engine
`kubernetes_controlplane` | Install Kubernetes control plane
`kubernetes_node` | Install Kubernetes node and join a cluster
`linux_server` | Install most commonly required Linux packages
`vagrant` | Install Vagrant

* Review the Ansible playbooks (can be used as is or as examples)

Ansible playbook | Action on the host
---------------- | ------------------
`kubernetes_controlplane` | Install Kubernetes control plane with kubeadm
`kubernetes_node` | Install Kubernetes node and join a cluster
`vagrant` | Install Vagrant

* Make sure Ansible is installed and an inventory is defined

* TODO

## How to contribute

* Make sure Ansible is installed

* Create local inventories for the tests

* Validate a code change by executing a playbook (use one or create a new one)

```bash
# checks the hosts can be reached ok
ansible all -m ping -i inventories/lab01

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
docker run --rm --name gitlab-runner -v /var/run/docker.sock:/var/run/docker.sock -v $PWD/.gitlab/runner/local/config:/etc/gitlab-runner -v $PWD:$PWD --workdir $PWD gitlab/gitlab-runner exec shell ci
```
