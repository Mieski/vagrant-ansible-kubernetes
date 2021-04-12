IMAGE_NAME = "bento/ubuntu-20.04"
N = 2

Vagrant.configure("2") do |config|
  config.ssh.insert_key = false

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"
    vb.cpus=2
  end

  config.vm.define "k8s-master" do |master|
    master.vm.box = IMAGE_NAME
    master.vm.hostname = "k8s-master"
    master.vm.network "public_network", bridge: "wlp0s20f3", ip: "192.168.1.100", hostname: true
    master.vm.provision "ansible" do |ansible|
      ansible.playbook = "kubernetes-setup/master-playbook.yml"
      ansible.extra_vars = {
        node_ip: "192.168.1.100"
      }
    end
  end

  (1..N).each do |i|
    config.vm.define "node-#{i}" do |node|
      node.vm.box=IMAGE_NAME
      node.vm.hostname = "node-#{i}"
      node.vm.network "public_network", bridge: "wlp0s20f3", ip: "192.168.1.#{100+i}", hostname: true
      node.vm.provision "ansible" do |ansible|
        ansible.playbook = "kubernetes-setup/node-playbook.yml"
        ansible.extra_vars = {
          node_ip: "192.168.1.#{100+i}"
        }
      end
    end
  end
end
