# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # thinking that we actually want to use the default insecure ssh keys for both VMs
  config.ssh.insert_key = false

  # config.vm.provider "virtualbox" do |vb|
  #   # vb.gui = true
  #   vb.cpus = 2 # two cores
  #   vb.memory = "4096"
  # end

  
  config.vm.define "dev1" do |dev|
    dev.vm.box = "generic/rocky8"

    dev.vm.provider "virtualbox" do |dev_vb| # lol, visual basic.
      dev_vb.cpus = 2
      dev_vb.memory = "4096"
    end 

    dev.vm.hostname = 'dev1.lan'
    dev.vm.network "private_network", ip: "192.168.69.69" # niiiiiice

    dev.vm.synced_folder "./keys", "/home/vagrant/keys"
    # Virtualbox's support of shared folders in/out of the VM is pretty shoddy. 
    # NOT YET dev.vm.synced_folder File.join(ENV['HOMEPATH'], 'code'), "/home/vagrant/code"

    # NOT YET dev.vm.network "forwarded_port", guest: 3000, host: 3000, host_ip: "127.0.0.1"
    # NOT YET dev.vm.network "forwarded_port", guest: 3035, host: 3035, host_ip: "127.0.0.1"
    # NOT YET dev.vm.network "forwarded_port", guest: 5432, host: 54321, host_ip: "127.0.0.1"

    dev.vm.provision "shell", inline: <<-SHELL
      sudo --user=vagrant --login mkdir -p /home/vagrant/.ssh
      sudo --user=vagrant --login chmod 700 /home/vagrant/.ssh
      sudo --user=vagrant --login cat /home/vagrant/keys/id_rsa.pub >> /home/vagrant/.ssh/authorized_keys
      echo "alias ls='ls -alh --color=auto'" >> .bashrc

    SHELL
  end


  config.vm.define "ans1" do |ans|
    ans.vm.box = "generic/rocky8"

    ans.vm.provider "virtualbox" do |ans_vb| 
      ans_vb.cpus = 1
      ans_vb.memory = "512"
    end

    ans.vm.hostname = 'ans1.lan'
    ans.vm.network "private_network", ip: "192.168.69.2"

    ans.vm.synced_folder "./dev_playbook", "/home/vagrant/dev_playbook"
    ans.vm.synced_folder "./keys", "/home/vagrant/keys"

    ans.vm.provision "shell", name: 'install ansible', inline: <<-SHELL
      sudo --user=vagrant --login mkdir -p /home/vagrant/.ssh && chmod 700 /home/vagrant/.ssh
      sudo --user=vagrant --login cp /home/vagrant/keys/id_rsa* /home/vagrant/.ssh && chmod 600 /home/vagrant/.ssh/id_rsa
      echo "alias ls='ls -alh --color=auto'" >> .bashrc

      yum install -y epel-release
      yum update -y
      yum install -y bash-completion htop yum-utils git
      yum install -y python39
      alternatives --set python3 /usr/bin/python3.9
      pip3 install --upgrade pip setuptools virtualenv
      sudo --user=vagrant --login python3 -m pip install --user ansible
    SHELL

    ans.vm.provision "shell", name: 'provision dev1 using ansible inside ans1', after: 'install ansible', communicator_required: true, privileged: false, inline: <<-SHELL
      # echo "----------------------------------------"
      # echo "SSH into this host with:"
      # echo "  vagrant ssh ans1"
      # echo ""
      # echo "Provision the development VM with:"
      # echo "  cd /home/vagrant/dev_playbook && ANSIBLE_CONFIG="./ansible.cfg" /home/vagrant/.local/bin/ansible-playbook playbook.yml -i inventory.ini"
      # echo ""
      # echo "----------------------------------------"
      
      cd /home/vagrant/dev_playbook
      rm -fr roles/zzet.rbenv/
      rm -fr roles/stephdewit.nvm/
      rm -fr roles/geerlingguy.memcached/
      rm -fr roles/geerlingguy.nginx/ 
      rm -fr roles/geerlingguy.repo-epel/
      rm -fr roles/geerlingguy.redis/
      rm -fr roles/avanov.pyenv/

      ansible-galaxy install --role-file=prereq_roles.yml --roles-path=/home/vagrant/dev_playbook/roles --force 
      ANSIBLE_CONFIG="/home/vagrant/dev_playbook/ansible.cfg" /home/vagrant/.local/bin/ansible-playbook /home/vagrant/dev_playbook/playbook.yml -i /home/vagrant/dev_playbook/inventory.ini
    SHELL
  end
  # beg/borrow/steal ideas from https://github.com/ValveSoftware/Proton/blob/proton_6.3/Vagrantfile
end
