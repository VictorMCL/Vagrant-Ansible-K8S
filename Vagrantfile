IMAGE ='centos/7' 
NODES = 1
NETWORK = '192.168.80.'
ClUSTER_NAME = 'Desarrollo'
ENV['VAGRANT_NO_PARALLEL'] = 'yes'

Vagrant.configure(2) do |config|
  config.ssh.insert_key = false
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "K8S/cluster.yaml"
    ansible.groups = {
      "master" => ["k8s-master"],
      "node" => ["k8s-node[1:#{NODES}]"]
    }
    ansible.extra_vars = {
      cluster_name: ClUSTER_NAME
    }
  end

  config.vm.define "k8s-master" do |kmaster|
    kmaster.vm.box = IMAGE
    kmaster.vm.hostname = "kmaster.com"
    kmaster.vm.network "private_network", ip: "#{NETWORK}#{200}"
    kmaster.vm.provider "virtualbox" do |v|
      v.name = "k8s-master"
      v.memory = 1024
      v.cpus = 2
    end
  end

  (1..NODES).each do |i|
    config.vm.define "k8s-node#{i}" do |knode|
      knode.vm.box = IMAGE
      knode.vm.hostname = "knode#{i}.com"
      knode.vm.network "private_network", ip: "#{NETWORK}#{i+200}"
      knode.vm.provider "virtualbox" do |v|
        v.name = "k8s-node#{i}"
        v.memory = 1024
        v.cpus = 1
      end
    end
  end
end
