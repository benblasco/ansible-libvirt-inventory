# ansible-libvirt-inventory

Fork of Ansible Libvirt Inventory project by bkmeneguello, that adds creation of an Execution Environment so this can be used
by Ansible Automation Platform

# Libvirt host prep notes

Ensure to add the regular user to the libvirt group on the hypervisor host
```
usermod -a -G libvirt <username>
```

## With an execution environment

```
ansible-navigator inventory -i inventory.libvirt.yml --eei localhost/ee-libvirt-micro:latest --list -m stdout -vv

```

## Ad hoc command to ping hosts
```
ansible-navigator exec --eei localhost/ee-libvirt-micro:latest -- ansible -i inventory.libvirt.yml all -m ping -u bblasco
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
