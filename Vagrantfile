# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.configure("2") do |config|
  # Configuração Geral
  config.vm.box = "debian/bookworm64"
  config.ssh.insert_key = false
  
  config.vm.provider :virtualbox do |v|
    v.memory = 1024
    v.check_guest_additions = false
  end
  
  config.vm.define "vm" do |node|
    node.vm.hostname = "marcelino.luis"
    node.vm.network :private_network, ip: "192.168.56.148"
    node.vm.provision "ansible" do |ansible|
      ansible.compatibility_mode = "2.0"
      ansible.playbook = "playbooks/playbook_ansible.yml"
    end
  end
end
