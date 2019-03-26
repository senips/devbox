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

  username = "#{ENV['USERNAME'] || `whoami`}".chomp
  devbox_profile_fileName = File.expand_path('~/devbox_profile.sh')
  bashvars = "";
  if(File.exist?(devbox_profile_fileName)) 
    bashvars = File.read(devbox_profile_fileName)
  end  
  config.vm.hostname = "devbox"
  config.vm.network :private_network, ip: "192.168.57.57"
  config.vm.synced_folder "/Users/#{username}/", "/home/vagrant/myhost"
  #custom key
  config.ssh.insert_key = false 
  config.ssh.private_key_path = ['~/.vagrant.d/insecure_private_key', '~/.ssh/devbox'] 
  config.vm.provision "file", source: "~/.ssh/devbox.pub", destination: "~/.ssh/authorized_keys" 
  config.vm.provision "file", source: "~/.ssh/devbox", destination: "~/.ssh/devbox"  #for github use but stored in your local VM anyway
  
  config.vm.provision "shell", inline: <<-EOC
    eval `echo '#{bashvars}' >> /home/vagrant/.profile`
    echo "optional: your env variables and other bash setups will be merged from your host machine ~/devbox_profile.sh into vagrant profile"
  EOC

  config.vm.provision "shell", inline: <<-EOC
    eval `ssh-agent`
    eval `ssh-add /home/$(whoami)/.ssh/devbox`
    sudo add-apt-repository ppa:jonathonf/python-3.6
    sudo apt-get -y install python3-pip
    sudo apt-get update
    echo "pre setup finished"
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

  config.vm.provision "shell", inline: <<-EOC
    sudo chown -R $(whoami) /usr/local
    sudo chown -R $(whoami)  /usr/local/lib/npm/bin
    sudo chown -R $(whoami)  /usr/local/lib/npm/lib
    sudo chown -R $(whoami)  /usr/local/lib/npm/lib/node_modules
    eval `npm install -g n`    #by default installed 10.15.3 but this is for switching node versions
    curl -0 -L https://npmjs.com/install.sh | sudo sh  #make sure right npm version installed.
    echo "post setup finished"
  EOC

end
