#IMAGE_NAME = "bento/ubuntu-20.04"

# For apple mac m1 chip users
IMAGE_NAME = "bento/ubuntu-20.04-arm64"
N = 2

Vagrant.configure("2") do |config|
    config.ssh.insert_key = false

    ### for virtualbox provider ###
    #config.vm.provider "virtualbox" do |v|

    ### for vmware provider ###
    config.vm.provider "vmware_desktop" do |v|
        v.memory = 2048
        v.cpus = 2
    end
      
    config.vm.define "k8s-master" do |master|
        master.vm.box = IMAGE_NAME
        master.vm.network "private_network", ip: "192.168.50.10"
        master.vm.hostname = "k8s-master"
        master.vm.provision "ansible" do |ansible|
            ansible.playbook = "kubernetes-setup/master-playbook.yml"
        end
    end

    (1..N).each do |i|
        config.vm.define "node-#{i}" do |node|
            node.vm.box = IMAGE_NAME
            node.vm.network "private_network", ip: "192.168.50.#{i + 10}"
            node.vm.hostname = "node-#{i}"
            node.vm.provision "ansible" do |ansible|
                ansible.playbook = "kubernetes-setup/node-playbook.yml"
            end
        end
    end
end
