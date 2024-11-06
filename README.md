# Ansible demo for PHP with STF project

## Objective

The server infrastructure supporting PHP is outdated, requiring extensive manual work and is currently managed by a single maintainer, which could potentially become a liability. This infrastructure plays a critical role as it hosts the PHP release files that users download.
As part of the Scope of Work with Sovereign Tech Fund [STF] (https://www.sovereigntechfund.de/) our goal is to modernize and optimize this process by introducing automation wherever suitable, which will reduce the time spent by maintainers in these tasks and improve technical resilience of the project.

## Proposal

Use [Ansible](https://www.ansible.com/) to automate the existing infrastructure maintenance process. Ansible is an open source IT automation engine that automates provisioning, configuration management, application deployment, orchestration, and many other IT processes.

## Setup

This is a demo repository to show how Ansible can be used to link jumphost to property host servers and package installation and upgrades can be made.

1. Three [Digital Ocean droplets](https://www.digitalocean.com/products/droplets) were set up. One to mimic the `jumphost` and the other two for property host servers.
2. This demo repository were set up to run Ansible on the hosts servers.
   a. Inventory: Define the 3 hosts that Ansible works against.
   b. Roles: Define the set of tasks that will be run against the defined hosts. In this example we have tasks to
      - Install Apache2
      - Configure Apache to listen to floating IPs of the host servers
      - Update Apache to latest version
3. Define `test.yml` to execute the tasks defined earlier. Here you need to define the host servers where you want your tasks to be run (in this case we chose the 'service' property host) and also specify remote user who will execute Ansible.

Ansible can SSH directly into the host servers and run the tasks that it needs to. In this example Ansible is run from your local machine and to do so you will need to have access to the example host server droplets on Neighbourhoodie Digital Ocean.

## Running Ansible

Clone the repository and request access to the test droplets.

Ensure you can succesfully `ssh` into the test droplets from your local machine.

Run Ansible as follows:

```bash
ansible-playbook -i inventory/demo test.yml
```

## Using encrypted vars
1. create file `EDITOR=nano ansible-vault create inventory/demo/group_vars/all.yml`
2. edit file `EDITOR=nano ansible-vault edit inventory/demo/group_vars/all.yml`

(Password was added by the create command: - 123)

Now the playbook run is:
```bash
ansible-playbook -i inventory/demo test.yml --ask-vault-pass
```

The demo achieves the following:

- Ansible is configured to operate through `jumphost` server to connect to the two property host servers.
- Ansible [role](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html) are set up to run a groups of similar tasks.
- Usage of encrypted variables
