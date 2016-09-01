# -*- mode: ruby -*-
# vi: set ft=ruby :

ENV['ANSIBLE_ROLES_PATH'] = "../"

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "AlbanMontaigu/boot2docker"
  config.vm.hostname = "ansible-docker-ssh-tunnel"

  config.vm.provider "virtualbox" do |v|
    v.customize ["modifyvm", :id, "--cpuexecutioncap", "50"]
    v.customize ["modifyvm", :id, "--memory", "512"]
  end

  config.vm.synced_folder ".", "/vagrant", disabled: true

  config.vm.define "master" do |master|
    master.vm.network :private_network, ip: "10.0.0.11"
  end

  config.vm.define "client" do |client|
    client.vm.network :private_network, ip: "10.0.0.12"

    client.vm.provision "ansible" do |ansible|
      ansible.playbook = "tests/vagrant/vagrant.yml"
      ansible.verbose = ""
      ansible.limit = "all"
      ansible.groups = {
        "master" => ["master"],
        "client" => ["client"]
      }
      ansible.host_vars = {
        "master" => { "ssh_tunnel_host": "10.0.0.11" },
        "client" => { "ssh_tunnel_host": "10.0.0.12" }
      }
    end
  end
end
