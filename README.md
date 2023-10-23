# ansible-libvirt-inventory






# Execution Environment build notes

Ensure you add yourself to the libvirt group
```
usermod -a -G libvirt <username>
```

Ensure your connection is to qemu:///system and not qemu:///session
```
export LIBVIRT_DEFAULT_URI=qemu:///system
```

or for a remote host:
```
export LIBVIRT_DEFAULT_URI=qemu+ssh://bblasco@somehost/system


export LIBVIRT_DEFAULT_URI="qemu+ssh://root:password@192.168.122.1/system?sshauth=ssh-password"

export LIBVIRT_DEFAULT_URI="qemu+ssh://rgomes@host.example.com/system?keyfile=$HOME/.ssh/id_ed25519_laptop.example.com"
```


## With an execution environment

```
export LIBVIRT_DEFAULT_URI=qemu+ssh://bblasco@opti.lan/system

ansible-navigator inventory -i inventory.libvirt.yml --eei localhost/ansible-execution-env --list -m stdout -vvv -e "LIBVIRT_DEFAULT_URI=qemu+ssh://bblasco@opti.lan/system"

```

## The working solution
```

LIBVIRT_DEFAULT_URI='qemu+ssh://bblasco:<annoyingpw>@opti.lan/system?sshauth=ssh-password'

ansible-navigator inventory -i inventory.libvirt.yml --eei localhost/ee-libvirt:latest --list -m stdout -vvv --penv LIBVIRT_DEFAULT_URI
```

## Ad hoc command to ping hosts
```
ansible-navigator exec --eei localhost/ee-libvirt:latest --penv LIBVIRT_DEFAULT_URI -- ansible -i inventory.libvirt.yml all -m ping -u bblasco
```


## Push my images to Private automation hub PAH

```
podman login aapnew.opti.blasco.id.au:444/ee-libvirt-opti:latest

podman push localhost/ee-libvirt-nuc aapnew.opti.blasco.id.au:444/ee-libvirt-opti:latest --tls-verify=false



