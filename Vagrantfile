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
    echo 172.16.16.100 bbb.ori.schuledigital.test >> /etc/hosts
    echo 172.16.16.100 mon.bbb.ori.schuledigital.test >> /etc/hosts
    echo 172.16.16.101 bbb1.ori.schuledigital.test >> /etc/hosts
    echo 172.16.16.111 turn1.ori.schuledigital.test >> /etc/hosts
    echo 172.16.16.102 bbb2.ori.schuledigital.test >> /etc/hosts
    echo 172.16.16.112 turn2.ori.schuledigital.test >> /etc/hosts
    echo 172.16.16.2 pebble >> /etc/hosts

    mkdir /usr/share/ca-certificates/extra
    curl https://raw.githubusercontent.com/ORI-s-Source/pebble/master/test/certs/pebble.minica.pem -o /usr/share/ca-certificates/extra/pebble.crt
    echo extra/pebble.crt > /etc/ca-certificates.conf
    update-ca-certificates
  EOC

  config.vm.synced_folder ".", "/vagrant", disabled: true
#   config.vm.network "public_network", type: "dhcp", :bridge => 'en0: Ethernet'

  config.vm.define "bbb.ori.schuledigital.test" do |node|
    node.vm.hostname = "bbb.ori.schuledigital.test"
    node.vm.network "private_network", ip: "172.16.16.100"
    node.vm.provider "virtualbox" do |v|
      v.name = "bbb.ori.schuledigital.test"
      v.memory = 4095
      v.cpus = 3
    end
  end

  config.vm.define "bbb1.ori.schuledigital.test" do |node|
    node.vm.hostname = "bbb1.ori.schuledigital.test"
    node.vm.network "private_network", ip: "172.16.16.101"
    node.vm.network "private_network", ip: "172.16.16.111"
    node.vm.provider "virtualbox" do |v|
      v.name = "bbb1.ori.schuledigital.test"
      v.memory = 4095
      v.cpus = 3
    end
  end

  config.vm.define "bbb2.ori.schuledigital.test" do |node|
    node.vm.hostname = "bbb2.ori.schuledigital.test"
    node.vm.network "private_network", ip: "172.16.16.102"
    node.vm.network "private_network", ip: "172.16.16.112"
    node.vm.provider "virtualbox" do |v|
      v.name = "bbb2.ori.schuledigital.test"
      v.memory = 4095
      v.cpus = 3
    end
  end

  config.vm.define "pebble" do |node|
      node.vm.hostname = "pebble"
      node.vm.network "private_network", ip: "172.16.16.2"
      node.vm.provider "virtualbox" do |v|
        v.name = "pebble"
        v.memory = 4095
        v.cpus = 3
      end
      node.vm.provision "shell", inline: <<-EOC
        curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
        add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
        apt-get update
        apt-get install -y docker-ce

        sudo curl -L https://github.com/docker/compose/releases/download/1.18.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
        sudo chmod +x /usr/local/bin/docker-compose

        git clone https://github.com/ORI-s-Source/pebble.git
        cd pebble

        docker-compose up -d

        curl -X POST -d '{"host":"bbb.ori.schuledigital.test", "addresses":["172.16.16.100"]}' http://localhost:8055/add-a
        curl -X POST -d '{"host":"www.bbb.ori.schuledigital.test", "addresses":["172.16.16.100"]}' http://localhost:8055/add-a
        curl -X POST -d '{"host":"mon.bbb.ori.schuledigital.test", "addresses":["172.16.16.100"]}' http://localhost:8055/add-a
        curl -X POST -d '{"host":"bbb1.ori.schuledigital.test", "addresses":["172.16.16.101"]}' http://localhost:8055/add-a
        curl -X POST -d '{"host":"turn1.ori.schuledigital.test", "addresses":["172.16.16.111"]}' http://localhost:8055/add-a
        curl -X POST -d '{"host":"bbb2.ori.schuledigital.test", "addresses":["172.16.16.102"]}' http://localhost:8055/add-a
        curl -X POST -d '{"host":"turn2.ori.schuledigital.test", "addresses":["172.16.16.112"]}' http://localhost:8055/add-a

      EOC
    end
end


#cp test/config/pebble-config.json ./my-pebble-config.json

        #sed -i 's/"httpPort".*/"httpPort": 80,/g' my-pebble-config.json
        #sed -i 's/"tlsPort".*/"tlsPort": 443,/g' my-pebble-config.json

        #sed -i '/ pebble:/a\ \ \ \ volumes:\n    - ./my-pebble-config.json:/test/config/pebble-config.json' docker-compose.yml
        #sed -i 's/-strict//g' docker-compose.yml


#git clone --branch release-2020-12-14 https://github.com/letsencrypt/boulder.git
        #cd boulder

        #sed -i 's/5002/80/g' test/config/va.json
        #sed -i 's/5001/443/g' test/config/va.json
        #sed -i 's/FAKE_DNS=.*/FAKE_DNS=172.16.16.2/g' docker-compose.yml
