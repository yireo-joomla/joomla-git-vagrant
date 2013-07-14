# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "base"
  config.vm.box_url = "package.box"
  config.vm.network :forwarded_port, guest: 80, host: 8080
  #config.vm.network :private_network, ip: "192.168.33.10"
  #config.vm.network :public_network
  config.ssh.forward_agent = true
  config.ssh.username = "root"
  config.vm.synced_folder "remote", "/var/www/html"
  config.vm.provision :shell, :path => "remote/setup.sh"
end
