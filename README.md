# Kali VMs managed by Vagrant

This is a way to easily set up a simple CTF/security lab environment with 3 Kali
VMs.

## Prerequisites

* Oracle Virtualbox [here](https://www.virtualbox.org/wiki/Downloads)
* Vagrant [here](https://developer.hashicorp.com/vagrant/install)

## Getting started

(Keep in mind that all `vagrant` commands need to be run from the root of this
repository, i.e. the directory that this README is in)

First of all, you need to install the `vagrant` plugin `vagrant-hostmanager`:

```shell
vagrant plugin install vagrant-hostmanager
```

### Creating and starting VMs

```shell
vagrant up          # If you want to create and start all 3 VMs
vagrant up kali1    # If you only want to create and start the first VM
```

`vagrant up` will first create the requested VM/VMs, and then start them. If the
VMs have already been created, and there have not been any changes to
`Vagrantfile`, it will simply start the VM/VMs.

### Shut down all running VMs

```shell
vagrant halt
```

If you want to save the state of the VMs:

```shell
vagrant suspend
```

### Logging in to the VMs

Default credentials are `vagrant`/`vagrant` - the username is `vagrant` and the
password is `vagrant`. This is how you log in using the GUI.

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

### `KALI1_CPU_CORES`

This determines how many CPU cores the `kali1` VM has available. In total, you
should at least keep 2 cores "to yourself". `kali2` and `kali3` have 2 cores, so
you should reserve max `(system_cores)-6` cores to `kali1`.

## The VMs

### `kali1`

IP: `192.168.56.11`

This is the main VM, which by default has 8GB of RAM allocated.

This is the one you should use for most daily use.

### `kali2` and `kali3`

IPs: `192.168.56.12` and `192.168.56.13`

These are auxiliary VMs primarily used for testing network hacking and attacks
on other machines from `kali1`. This means they don't have as much resources
available.

## If this doesn't work

### Mac with Apple Silicon (M-series chips)

See `apple_silicon_kali.pdf`, from the teaching assistant in 2025.

### Any other reason

Download the pre-built VM and set it up manually:
[here](https://www.kali.org/get-kali/#kali-virtual-machines)
