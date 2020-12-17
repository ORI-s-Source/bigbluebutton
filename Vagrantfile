Vagrant.configure("2") do |config|
  config.vm.box = "generic/ubuntu1604"

  config.ssh.insert_key = false
#   config.ssh.private_key_path = ['~/.vagrant.d/insecure_private_key', '~/.ssh/id_rsa']
  config.vm.provision "file", source: "~/.ssh/id_rsa.pub", destination: "~/.ssh/authorized_keys"

  config.vm.provision "shell", inline: <<-EOC
    sudo sed -i 's/.*PasswordAuthentication.*/PasswordAuthentication no/' /etc/ssh/sshd_config
    sudo sed -i 's/.*PermitRootLogin.*/PermitRootLogin yes/' /etc/ssh/sshd_config
    sudo systemctl restart sshd.service
    echo "finished"
    echo 172.16.16.100 bbb >> /etc/hosts
    echo 172.16.16.101 bbb1 >> /etc/hosts
    echo 172.16.16.102 bbb2 >> /etc/hosts
  EOC

  config.vm.synced_folder ".", "/vagrant", disabled: true
#   config.vm.network "public_network", type: "dhcp", :bridge => 'en0: Ethernet'

  config.vm.define "bbb" do |node|
    node.vm.hostname = "bbb"
    node.vm.network "private_network", ip: "172.16.16.100"
    node.vm.provider "virtualbox" do |v|
      v.name = "bbb"
      v.memory = 2048
      v.cpus = 2
    end
  end

  config.vm.define "bbb1" do |node|
    node.vm.hostname = "bbb1"
    node.vm.network "private_network", ip: "172.16.16.101"
    node.vm.provider "virtualbox" do |v|
      v.name = "bbb1"
      v.memory = 2048
      v.cpus = 2
    end
  end

  config.vm.define "bbb2" do |node|
    node.vm.hostname = "bbb2"
    node.vm.network "private_network", ip: "172.16.16.102"
    node.vm.provider "virtualbox" do |v|
      v.name = "bbb2"
      v.memory = 2048
      v.cpus = 2
    end
  end
end
