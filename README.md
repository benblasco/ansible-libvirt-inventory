# ansible-libvirt-inventory

Fork of Ansible Libvirt Inventory project by bkmeneguello, that adds creation of an Execution Environment so this can be used
by Ansible Automation Platform

# Libvirt host prep notes

Ensure to add the regular user (you'll be using for Ansible playbooks) to the libvirt group on the hypervisor host
```
usermod -a -G libvirt <username>
```

# Execution environment build and test notes

Wherever you are building the EE:
1. Git clone this repo
2. Generate an SSH key pair where the private key is named `libvirt_ssh_key`, and place the pair under execution_environment/
3. `cd execution_environment`
4. Build the EE, e.g. `ansible-builder build --prune-images -t <your_ee_name>:latest`
5. Copy the public key libvirt_ssh_key.pub to the user@host defined in the Containerfile using the command `ssh-copy-id -i libvirt_ssh_key.pub user@host`
6. Test the execution environment with the following command: `ansible-navigator inventory -i inventory.libvirt.yml --eei <your_ee_name>:latest --list -m stdout -vv
7. Push the EE to Automation Hub or Quay

Note: You will need to edit the execution-environment.yml line where it says `LIBVIRT_DEFAULT_URI` and ensure it contains the correct user name and host name for the hypervisor.

Note: You will have to build a separate EE for every hypervisor you are wanting to collect inventory from, due to the LIBVIRT_DEFAULT_URI being hard coded. I do not yet know a way around this, but am certain there is one.

You may also need to use a python virtual environment for the build:
```
virtualenv ansible-dev-tools
source ansible-dev-tools/bin/activate
pip3 install ansible-dev-tools
```

## Test an execution environment

```
ansible-navigator inventory -i inventory.libvirt.yml --eei localhost/<your_ee_name>:latest --list -m stdout -vv

```

## Test using an ad-hoc command to ping hosts
```
ansible-navigator exec --eei localhost/<your_ee_name>:latest -- ansible -i inventory.libvirt.yml all -m ping -u bblasco
```

# How to use this code in AAP

1. Create a project that points to this repo
2. Create an inventory
3. Create an inventory source with:
    - Source: From a project (ie this project)
    - Inventory file: inventory.libvirt.yml
    - Execution environment: The execution environment created from this repo
    - Credential: None (as they are embedded in the EE)
    - Options: Overwrite, Overwrite variables, Update on launch
4. Add multiple instances of the inventory source if you have multiple instances of your hypervisor to access for retrieval of inventory
