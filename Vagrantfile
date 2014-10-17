# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
    config.vm.box = "arvat-test"
    config.vm.hostname = "arvat"

    # kibana3
    config.vm.network :forwarded_port, guest: 80, host: 8080
    # elasticsearch
    config.vm.network :forwarded_port, guest: 9200, host: 9292
    # tomcat 1
    config.vm.network :forwarded_port, guest: 8080, host: 8090
    # tomcat 2
    config.vm.network :forwarded_port, guest: 8081, host: 8091
    # tomcat 3
    config.vm.network :forwarded_port, guest: 8082, host: 8092

    config.vm.network :private_network, ip: "192.168.55.2"

    config.vm.synced_folder "./", "/vagrant", :nfs => true

    config.vm.provider :virtualbox do |vb|
        vb.customize ["modifyvm", :id, "--memory", "2048"]
        # show GUI of virtualbox
        #vb.gui = true
    end
end
