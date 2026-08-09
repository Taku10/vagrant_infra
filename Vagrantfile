Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-24.04"
  config.vm.hostname = "k3s-server"

  config.vm.network "private_network", ip: "192.168.56.10"

  config.vm.provider "virtualbox" do |vb|
    vb.name = "k3s-server"
    vb.cpus = 4
    vb.memory = 8192
  end

  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y curl git

    if ! command -v k3s >/dev/null 2>&1; then
      curl -sfL https://get.k3s.io | sh -
    fi

    mkdir -p /home/vagrant/.kube
    cp /etc/rancher/k3s/k3s.yaml /home/vagrant/.kube/config
    chown -R vagrant:vagrant /home/vagrant/.kube
    chmod 600 /home/vagrant/.kube/config

    sed -i 's/127.0.0.1/192.168.56.10/' /home/vagrant/.kube/config
  SHELL
end