# -*- mode: ruby -*-
# vi: set ft=ruby :

$script_inject_pk =<<-'SCRIPT'
cat /vagrant/id_rsa.pub >> /home/vagrant/.ssh/authorized_keys
SCRIPT

VAGRANTFILE_API_VERSION = "2"
Vagrant.configure(VAGRANTFILE_API_VERSION) do |configuration|
# General Vagrant VM configuration.
 configuration.ssh.insert_key = false

# Dev machine
(1..2).each do |i|
 configuration.vm.define "dev-machine#{i}" do |app|
   app.vm.hostname = "dev-machine#{i}"
   app.vm.box = "ubuntu/focal64"
   app.vm.network :private_network, ip: "192.168.60.10#{i}"
   app.vm.provision "shell", inline: $script_inject_pk
 end
end

# Ansible machine
 configuration.vm.define "ansible" do |app|
   app.vm.hostname = "ansible.dev"
   app.vm.box = "centos/7"
   app.vm.network :private_network, ip: "192.168.60.110"
   app.vm.provision "file", source: "./id_rsa", destination: "/home/vagrant/.ssh/"
end

# Tomcat machine
 configuration.vm.define "tomcat" do |app|
   app.vm.hostname = "tomcat.dev"
   app.vm.box = "centos/7"
   app.vm.network :private_network, ip: "192.168.60.120"
   app.vm.provider "virtualbox" do |vb|
     vb.memory = "4096"
     vb.cpus = "2"
   end
   app.vm.provision "shell", inline: $script_inject_pk
 end


# CI Server Jenkins
 configuration.vm.define "jenkins" do |server|
 server.vm.hostname = "jenkins"
 server.vm.box = "centos/7"
 server.vm.network :private_network, ip: "192.168.60.10"
 server.vm.provider :virtualbox do |v|
       v.memory = 8192
   end
 server.vm.provision "shell", inline: $script_inject_pk
 end

# Docker
 configuration.vm.define "docker" do |server|
   server.vm.hostname = "docker.dev"
   server.vm.box = "centos/7"
   server.vm.network :private_network, ip: "192.168.60.240"
   server.vm.provider :virtualbox do |v|
       v.memory = 2048
   end
   server.vm.provision "shell", inline: $script_inject_pk
 end

end   
