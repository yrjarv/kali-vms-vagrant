# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "kalilinux/rolling"

  %w[vagrant-hostmanager].each do |plugin|
    unless Vagrant.has_plugin?(plugin)
      raise <<~MSG
        Missing required Vagrant plugin: #{plugin}
        Install it with:
          vagrant plugin install #{plugin}
      MSG
    end
  end

  # Hostmanager, for automatic /etc/hosts
  config.hostmanager.enabled = true
  config.hostmanager.manage_guest = true
  config.hostmanager.ignore_private_ip = false

  # Custom configuration
  load "config.rb" if File.exist?("config.rb")

  (1..3).each do |i|
    config.vm.define "kali#{i}" do |kali|
      kali.vm.hostname = "kali#{i}"
      kali.vm.network "private_network", ip: "192.168.56.#{10 + i}"

      kali.vm.provider "virtualbox" do |vb|
        if i == 1
          # kali1 should start with GUI open, and also have more RAM to compensate
          # for that
          vb.gui = true
          vb.memory = defined?(KALI1_RAM_MB) ? KALI1_RAM_MB : 8192
        else
          # kali2 and kali3 shouldn't be started with GUI or run any heavy
          # computation (hashcracking, mapping, hydra, etc), so no need for more
          # RAM
          vb.memory = "1024"
        end
      end

      # For faster provisioning (shared apt cache)
      cache_dir = File.expand_path(".cache", __dir__)
      FileUtils.mkdir_p(cache_dir)
      kali.vm.synced_folder cache_dir.to_s, "/var/cache/apt/archives/"

      # Provisioning, apt updating and installation of gdb-peda
      kali.vm.provision "shell", inline: <<-SHELL
        apt update
        apt -y upgrade
        apt install -y gdb-peda
        echo "source /usr/share/gdb-peda/peda.py" > /home/vagrant/.gdbinit
        chown vagrant:vagrant /home/vagrant/.gdbinit
      SHELL
    end
  end
end
