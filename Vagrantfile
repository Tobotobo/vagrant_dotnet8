# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-22.04"
  config.vm.box_version = "202401.31.0"

  config.vm.synced_folder ".", "/vagrant"

  # config.vm.network "private_network", type: "dhcp"
  # config.vm.network "private_network", ip: "192.168.33.10"
  config.vm.network "public_network", type: "dhcp"

  config.vm.provider "virtualbox" do |vb|
    vb.cpus = "2"
    vb.memory = "2048"
  end

  config.vm.provision "shell", inline: <<-SHELL
    # タイムゾーンの設定
    apt-get install -y tzdata
    ln -fs /usr/share/zoneinfo/Asia/Tokyo /etc/localtime
    dpkg-reconfigure -f noninteractive tzdata

    # Git のインストール
    apt-get install -y git

    # .NET 8 SDK のインストール
    wget https://dot.net/v1/dotnet-install.sh -O dotnet-install.sh
    chmod +x ./dotnet-install.sh
    mkdir -p /usr/share/dotnet
    ./dotnet-install.sh --version 8.0.204 --install-dir /usr/share/dotnet
    # apk add --no-cache icu-libs # 依存パッケージ
    echo 'export PATH=$PATH:/usr/share/dotnet' | sudo tee /etc/profile.d/dotnet.sh
    sudo chmod +x /etc/profile.d/dotnet.sh

    # IP アドレス確認
    ip addr show eth1 | grep 'inet ' | awk '{print $2}' | cut -d/ -f1
  SHELL

end
