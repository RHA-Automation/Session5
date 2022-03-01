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

Get the container images that we will be using to build off of:
~~~bash
podman login registry.redhat.io
podman pull registry.redhat.io/ansible-automation-platform-21/ee-supported-rhel8:latest
podman pull registry.redhat.io/ansible-automation-platform-21/ee-29-rhel8:latest
podman pull registry.redhat.io/ansible-automation-platform-21/ee-minimal-rhel8:latest
~~~

Create the `execution-environment.yml`, `requirements.yml`, `requirements.txt`, `bindeps.txt`, `ansible.cfg` files that will be used for the build process. Then start the build:
~~~bash
ansible-builder build -f execution-environment.yml -t <name>
~~~

If you only want to generate the Containerfile, use `create` instead of `build`:
~~~bash
ansible-builder create
~~~

Investigate the contents of the image using `ansible-navigator` (use `--pp missing` to not pull the image):
~~~bash
ansible-navigator images --pp missing
~~~

Create a running container and have a look inside of it:
~~~bash
podman run -it --rm <name> /bin/bash
~~~

Let's use the execution environment we've created to run a job using the collections we've baked into it:
~~~bash
ansible-navigator run ping.yml -i inventory --ll debug -m stdout
~~~

## Resources

- [Ansible Builder Guide](https://access.redhat.com/documentation/en-us/red_hat_ansible_automation_platform/2.1/html/ansible_builder_guide/index)