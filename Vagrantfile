# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
    config.vm.box = "centos/7"
    config.vm.provider "virtualbox"

    # Application server
    config.vm.define "noodfonds", primary: true do |node|
        node.vm.hostname = "noodfonds"

        node.vm.network "private_network", ip: "10.10.10.70"

        # VM specific memory and CPU settings.
        node.vm.provider "virtualbox" do |node_virtualbox|
            node_virtualbox.memory = 1024
            node_virtualbox.cpus = 1
        end

        # Ansible folder.
        node.vm.synced_folder "ansible", "/vagrant"

        # Application folder
        node.vm.synced_folder ".", "/var/www/vhosts/noodfonds", type: "nfs"

        # Run Ansible from the Vagrant VM
        node.vm.provision "ansible_local" do |ansible|
            ansible.playbook = "playbook.yml"
        end

        node.vm.provision :shell, :inline => "sudo systemctl restart httpd;", run: "always"
    end
end
