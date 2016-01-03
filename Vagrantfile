# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure('2') do |config|
  config.vm.box = "trusty"
  config.vm.box_url = "https://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box"
  config.ssh.forward_agent = true
  #config.ssh.insert_key = false
  config.vm.synced_folder ".", "/vagrant", disabled: true

  config.vm.provider "virtualbox" do |vb|
    vb.customize ["modifyvm", :id, "--natdnsproxy1", "off"]
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "off"]
    vb.customize ["modifyvm", :id, "--memory", "512"]
  end

  config.vm.define "v-htpc", autostart: false do |machine|
    machine.vm.hostname = "v-htpc"
    machine.vm.network :private_network, ip:"172.28.128.127"

  end

  config.vm.provision "ansible" do |ansible|
    ansible.host_key_checking = false
    #ansible.verbose = "vvvv"
    ansible.playbook = "playbook.yml"
    ansible.inventory_path = "development/inventory"

    ansible.extra_vars = {
      ansible_ssh_args: '-o ForwardAgent=yes',
      ansible_ssh_user: 'vagrant',
      host_key_checking: 'False'
    }

    ansible.groups = {
      "htpc" => %w(v-htpc),
    }

  end

end
