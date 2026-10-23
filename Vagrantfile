# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
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

  # Kali
  load "config.rb" if File.exist?("config.rb")
  (1..3).each do |i|
    config.vm.define "kali#{i}" do |kali|
      kali.vm.box = "kalilinux/rolling"
      kali.vm.hostname = "kali#{i}"
      kali.vm.network "private_network", ip: "192.168.56.#{10 + i}"

      kali.vm.provider "virtualbox" do |vb|
        if i == 1
          # kali1 is sometimes the only VM running, and will to more demanding
          # computations, so we need more RAM to compensate for that
          vb.memory = defined?(KALI1_RAM_MB) ? KALI1_RAM_MB : 8192
          vb.cpus = defined?(KALI1_CPU_CORES) ? KALI1_CPU_CORES : 2
        else
          # kali2 and kali3 shouldn't run any heavy computation (hashcracking,
          # mapping, hydra, etc), so no need for more RAM
          vb.memory = "1024"
          vb.cpus = 2
        end
        vb.gui = true # `vagrant ssh` doesn't work when you disable NAT
      end

      # For faster provisioning (shared apt cache)
      cache_dir = File.expand_path(".cache", __dir__)
      FileUtils.mkdir_p(cache_dir)
      kali.vm.synced_folder cache_dir.to_s, "/var/cache/apt/archives/"

      # Provisioning, apt updating and installation of gdb-peda
      kali.vm.provision "shell", inline: <<-SHELL
        apt update
        apt install -y gdb-peda python3-pwntools
        echo "source /usr/share/gdb-peda/peda.py" > /home/vagrant/.gdbinit
        chown vagrant:vagrant /home/vagrant/.gdbinit
      SHELL
    end
  end

  # Metasploitable
  config.vm.define "metasploitable" do |metasploitable|
    metasploitable.vm.box = "Sliim/metasploitable2"

    metasploitable.ssh.username = "msfadmin"
    metasploitable.ssh.password = "msfadmin"

    metasploitable.vm.synced_folder ".", "/vagrant", disabled: true

    metasploitable.vm.network "private_network", ip: "192.168.56.21"

    metasploitable.vm.provider "virtualbox" do |vb|
      vb.memory = "1024"
      vb.cpus = 2
      vb.gui = true # `vagrant ssh` doesn't work when you disable NAT
    end
  end
end
