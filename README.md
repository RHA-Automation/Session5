# Session 5 (Mar 1, 2022)

## Motivation

Ansible Builder is a command line tool that automates the process of building automation execution environments by using the metadata defined in various Ansible Collections, as well as by the user.

Before Ansible Builder was developed, Automation Platform users would potentially run against dependency issues and multiple error messages as they attempted to create a custom virtual environment or container that had all of their required dependencies installed.

Through the use of an easily customizable definition file, Ansible Builder installs Ansible, specified Collections and any of its dependencies so that all of the necessary requirements to get jobs running are fulfilled behind the scenes.

## Installation

If you are not getting `ansible-builder` from your AAP subscription (or even a developer subscription), you can install it using `pip` (adjust to suit your needs):

~~~bash
python3.9 -m venv rha-builder
source rha-builder/bin/activate
pip install --upgrade pip
pip install ansible-builder
~~~

## Usage

~~~bash
podman login registry.redhat.io
podman pull registry.redhat.io/ansible-automation-platform-21/ee-supported-rhel8:latest
podman pull registry.redhat.io/ansible-automation-platform-21/ee-29-rhel8:latest
podman pull registry.redhat.io/ansible-automation-platform-21/ee-minimal-rhel8:latest
~~~

~~~bash
ansible-builder build -f execution-environment.yml -t <name>
ansible-builder create
~~~

#### Examples

### Configuration

`ansible-navigator.yml` is the config file present in each project that will determine how automation is created and executed with `ansible-navigator`.

## Execution Environments

### Download

[Container images at catalog.redhat.com](https://catalog.redhat.com/software/containers/search?q=execution%20environment&p=1&product_listings_names=Red%20Hat%20Ansible%20Automation%20Platform&build_categories_list=Automation%20Execution%20Environment)

~~~bash
podman login
podman pull registry.redhat.io/ansible-automation-platform-21/ee-29-rhel8
podman pull registry.redhat.io/ansible-automation-platform-21/ee-minimal-rhel8
podman pull registry.redhat.io/ansible-automation-platform-21/ee-supported-rhel8
~~~

### Enable Usage

In `ansible-navigator.yml` set `enabled: true` in `execution-environment` section.

What is a `run` doing:

~~~bash
ansible-navigator run ping.yml -i inventory --ll debug -m stdout
~~~

Bonus points:

~~~bash
ansible-navigator run usecase3_rootcontainers_networked.yml -i inventory --ll debug --vault-password-file /home/ansible/gitignored_secret --eei quay.io/kvegh/podautee -e @/home/ansible/vault-auth.yml -m stdout -u ansible 
~~~

## Resources

- [Ansible Builder Guide](https://access.redhat.com/documentation/en-us/red_hat_ansible_automation_platform/2.1/html/ansible_builder_guide/index)