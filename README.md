# Ansible demo for PHP with STF project

## Objective

The server infrastructure supporting PHP is outdated, requiring extensive manual work and is currently managed by a single maintainer, which could potentially become a liability. This infrastructure plays a critical role as it hosts the PHP release files that users download.
As part of the Scope of Work with Sovereign Tech Fund [STF](https://www.sovereigntechfund.de/) our goal is to modernize and optimize this process by introducing automation wherever suitable, which will reduce the time spent by maintainers in these tasks and improve technical resilience of the project.

## Proposal

We created this project as a test case for [Ansible](https://docs.ansible.com/ansible/latest/index.html) to demonstrate how we can use it to automate server infrastructure setup.

Ansible is a configuration management tool that facilitates the task of setting up and maintaining remote servers.
Ansible doesn’t require any special software to be installed on the nodes that will be managed with this tool. A control machine is set up with the Ansible software, which then communicates with the nodes via standard SSH.

[Here are some tips for making the most of Ansible and Ansible playbooks.](https://docs.ansible.com/ansible/2.8/user_guide/playbooks_best_practices.html#best-practices)

## What the demo aims to do

This demo repository gives an overview of how Ansible can be used to manage PHP infrastructure. It includes the following:

- Ansible is configured to operate through `jumphost` server to connect to the two property host servers.
- An Ansible [role](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html) is set up to install Apache on all the property host servers.
- Encrypted variables are used. Ansible has its own dedicated [Ansible Vault](https://docs.ansible.com/ansible/latest/vault_guide/vault_encrypting_content.html) to manage and store vault passwords.

## Set up for the demo project

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

Ensure you can successfully `ssh` into the test droplets from your local machine.

Run Ansible as follows:

```bash
ansible-playbook -i inventory/demo test.yml --ask-vault-pass
```

## Using encrypted vars

1. create file `EDITOR=nano ansible-vault create inventory/demo/group_vars/all.yml`
2. edit file `EDITOR=nano ansible-vault edit inventory/demo/group_vars/all.yml`

(Password was added by the create command: - 123)

Further details on encryption can be found in [ansible documentation](https://docs.ansible.com/ansible/latest/vault_guide/vault_encrypting_content.html).
