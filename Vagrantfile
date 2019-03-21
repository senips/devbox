# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "bento/ubuntu-18.04"
  

  config.vm.provider :virtualbox do |v|
    v.name = "devbox"
    v.memory = 4096
    v.cpus = 2
  end

  config.vm.hostname = "devbox"
  config.vm.network :private_network, ip: "192.168.57.57"
  config.vm.synced_folder "/Users/senthil/keys", "/keys"
  config.vm.synced_folder "/Users/senthil/all-sources", "/all-sources"
  #custom key
  config.ssh.insert_key = false 
  config.ssh.private_key_path = ['~/.vagrant.d/insecure_private_key', '~/.ssh/devbox'] 
  config.vm.provision "file", source: "~/.ssh/devbox.pub", destination: "~/.ssh/authorized_keys" 
  config.vm.provision "file", source: "~/.ssh/devbox", destination: "~/.ssh/devbox"  #for github use but stored in your local VM anyway
  config.vm.provision "shell", inline: <<-EOC
    eval `ssh-agent`
    eval `ssh-add /home/vagrant/.ssh/devbox`
    sudo add-apt-repository ppa:jonathonf/python-3.6
    sudo apt-get -y install python3-pip
    sudo apt-get update
    echo "finished"
  EOC

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
