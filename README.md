# Ansible Automation Platform 2 automated setup

With the Technology preview of [containerized setup of AAP2](https://www.ansible.com/blog/announcing-containerized-ansible-automation-platform) you have a new way of deploying the components of the automation platform in a fast way using containers.

I created this repo to help you get started with a minimal setup and no customizations and start working on it in minutes!

**This playbook is not endorsed or supported by Red Hat and it is not an official way to set up the product!**

## Requirements

### System requirements

Recommended requirements for a smooth installation are:

| Resource   | Quantity |
| ---------- | -------- |
| Memory     | 16Gb     |
| CPU        | 4        |
| Disk space | 40 Gb    |
| Disk IOPs  | 1500     |

The remote user must have sudo privileges.

### Ansible requirements

To install required collections and roles:

```bash
ansible-galaxy install -r collections/requirements.yml -f
```

### Playbook Variables

I made available the [vars_aap2_setup.yml file](./vars_aap2_setup.yml) to collect the required variables. It can be adapted to a vault file to store the information.

The mandatory vars are:

| Variable      | Description                                                                                                 |
| ------------- | ----------------------------------------------------------------------------------------------------------- |
| rhsm_user     | Username for Red Hat Subscription manager, to register the VM                                               |
| rhsm_password | Password for Red Hat Subscription manager, to register the VM                                               |
| rh_api_token  | API token to download the setup bundle - [click here to retrieve](https://access.redhat.com/management/api) |

### Inventory

I already prepared a one-liner for the inventory, you can adjust it to match your preferred authentication method or any option that is required.

```ini
aap2_host ansible_host=YOUR_HOST_HERE ansible_user=YOUR_REMOTE_USERNAME_HERE ansible_password=YOUR_REMOTE_USER_PWD_HERE ansible_ssh_common_args='-o StrictHostKeyChecking=no'
```

**ansible_host variable will be used to set the hostname/IP address where Ansible Automation Platform will be reachable, ensure it matches the FQDN or the IP**

That's all you need to launch the playbook, edit it accordingly if you use SSH keys to connect.

## Launch the setup playbook

The playbook is ready to go, it will:

- Register the instance with RHSM
- Install required packages
- Populate the setup inventory
- Run the setup

Detailed steps can be found in [the documentation](https://access.redhat.com/documentation/en-us/red_hat_ansible_automation_platform/2.4/html-single/containerized_ansible_automation_platform_installation_guide/index#doc-wrapper)

To launch:

```bash
ansible-playbook -i inventory -e @vars_aap2_setup.yml aap2_setup.yml
```

To pass a different file, vaulted or not, just replace the variable file name.

In a bunch of minutes your setup will be available and you will get a prompt with the URLs, username and password to access your instances.
