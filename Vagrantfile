# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.require_version ">2.4.0"
VAGRANT_API_VERSION = "2"
require 'yaml'
servers_list = YAML.load_file(File.join(File.dirname(__FILE__), 'provisioning/servers_list.yml'))

Vagrant.configure(VAGRANT_API_VERSION) do |config|

  servers_list.each do |server|
    config.vm.define server["vagrant_box_host"] do |box|
      box.vm.box = server["vagrant_box"]
      box.vm.hostname = server["vagrant_box_host"]
      box.vm.base_mac = server["vagrant_box_mac"]
      box.vm.base_address = server["vagrant_box_ip"]
#       box.vm.network  "forwarded_port", guest: server["guest_port"], host: server["host_port"]
      box.vm.provider "vmware_desktop" do |vmd|
        vmd.memory = server["vbox_ram"]
        vmd.cpus = server["vbox_cpu"]
        vmd.gui = true
      end

      box.vm.provision "ansible_local" do |ansible|
        ansible.playbook = server["ansible_bootstrap"]
      end
#
      box.vm.provision "shell", inline: <<-SHELL
        echo "======================================================================================="
        hostnamectl status
        echo "======================================================================================="
        SHELL
    end
  end
end
