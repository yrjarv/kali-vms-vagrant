# Kali VMs managed by Vagrant

This is a way to easily set up a simple CTF/security lab environment with 3 Kali
VMs.

## Getting started

Keep in mind that all `vagrant` commands need to be run from the root of this
repository, i.e. the directory that this README is in.

* `vagrant up` to start the VMs.

* `vagrant halt` to shut down the VMs.

Sometimes, you will get the message "Machine already provisioned", in which case
you should run `vagrant provision` to ensure the correct setup of the VMs.

### Only starting `kali1`

As `kali1` is the main VM, you might only want to create and/or boot that one
VM, in which case you run `vagrant up kali1`.

### Logging in to the VMs

Default credentials are `vagrant`/`vagrant` - the username is `vagrant` and the
password is `vagrant`. This is how you log in using the GUI.

You can also access the VMs using SSH: `vagrant ssh <vm name>` (e.g. `vagrant
ssh kali2`). With this method, Vagrant will use a key pair for authentication -
so you will not need to input any password.

## Configuration

`config.rb` is a file which can contain custom configuration. Currently, only
one config key is supported.

If you change a config value, run `vagrant reload` to automatically shut down
and re-configure the necessary VMs.

### `KALI1_RAM_MB`

This determines how many MB of RAM the `kali1` VM should get. You should not to
use more than `(system_ram/2) - 2048MB`, as `kali2` and `kali3` get 1024MB each
and you ideally want to keep at least half of your computer's RAM for the host
OS.

The default value is `8192`, i.e. 8GB.

## The VMs

### `kali1`

IP: `192.168.56.11`

This is the main VM, which by default has 8GB of RAM allocated.

When booting up, this VM shows its GUI - so this is the one you should use for
most daily use.

### `kali2` and `kali3`

IPs: `192.168.56.12` and `192.168.56.13`

These are auxiliary VMs primarily used for testing network hacking and attacks
on other machines from `kali1`, without needing to 

