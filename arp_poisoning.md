# ARP Poisoning

This is how to practice ARP poisoning on this setup.

We will use `kali1` as the attacker, since it has enough RAM to run the
`ettercap` GUI.

`kali2` is our victim.

`kali3` is our gateway.

## Setup

The internal network is at `eth1` on all of our VMs.

NAT is at `eth0`.

What we need to do, is disable `eth0` on `kali1` and `kali2`:

```shell
# Run this on both kali1 and kali2
sudo ifconfig eth0 down
```

Then, we set `kali1` and `kali2` to use `kali3` as the default gateway:

```shell
# Run this on both kali1 and kali2
sudo route add default gw 192.168.56.13
```

Finally, we need to set up `kali3` to be a gateway:

```shell
# Run this on kali3
curl https://hackingarena.com/arena-exploits/ipt.sh | sudo sh
```

## Teardown

The best way to ensure no weird stuff will happen in the future, is to run
`vagrant destroy` and then `vagrant up`. That will delete the VMs and create
them from scratch.

But, here is the other option:

### `kali1` and `kali2`

```shell
sudo ifconfig eth0 up
sudo route del default gw 192.168.56.13
```

### `kali3`

```shell
sudo sysctl -w net.ipv4.ip_forward=0
sudo iptables -F
sudo iptables -X
sudo iptables -t nat -F
sudo iptables -t nat -X
```
