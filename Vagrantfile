# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "ubuntu/trusty64"
  #config.ssh.insert_key = false

  config.vm.provider :virtualbox do |v|
    v.name = "devbox"
    v.memory = 4096
    v.cpus = 2
  end

  config.vm.hostname = "devbox"
  config.vm.network :private_network, ip: "192.168.57.57"
  config.vm.synced_folder ".", "/vagrant"

  # Set the name of the VM. See: http://stackoverflow.com/a/17864388/100134
  config.vm.define :devbox do |devbox|
  end

  # Ansible provisioner.
  config.vm.provision "ansible" do |ansible|
    ansible.compatibility_mode = "2.0"
    ansible.playbook = "provisioning/playbook.yml"
    ansible.inventory_path = "provisioning/inventory"
    ansible.become = true
  end

end
