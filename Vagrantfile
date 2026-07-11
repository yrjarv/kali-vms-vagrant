# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "kalilinux/rolling"
  
  (1..3).each do |i|
    config.vm.define "kali#{i}" do |kali|
      kali.vm.hostname = "kali#{i}"
      kali.vm.network "private_network", ip: "192.168.56.#{10 + i}"
    end

    # config.vm.provider "virtualbox" do |vb|
      # Display the VirtualBox GUI when booting the machine
      # vb.gui = true
   
      # Customize the amount of memory on the VM:
      # vb.memory = "4096"
    # end
  end

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.56.1"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Enable provisioning with a shell script. Additional provisioners such as
  # Ansible, Chef, Docker, Puppet and Salt are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
    # echo "Hello World!"
  # SHELL
end
