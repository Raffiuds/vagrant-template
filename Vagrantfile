# encoding: utf-8
# -*- mode: ruby -*-
# vi: set ft=ruby :
# Box / OS
VAGRANT_BOX = 'ubuntu/noble64'
# Memorable name for your
VM_NAME = 'vagrant-vm'
# VM User — 'vagrant' by default
VM_USER = 'vagrant'
# Username on your PC
PC_USER = '{USER}'
# Host folder to sync
HOST_PATH = '/Users/' + PC_USER + '/' + VM_NAME
# Where to sync to on Guest — 'vagrant' is the default user name
GUEST_PATH = '/home/' + VM_USER + '/' + VM_NAME
# # VM Port — uncomment this to use NAT instead of DHCP
# VM_PORT = 8080
SYNCED_FOLDER = '/Users/' + PC_USER + '/vagrant/workspace'
VM_SYNCED_FOLDER = '/home/' + VM_USER + '/workspace'

VM_HOME = '/home/' + VM_USER

Vagrant.configure(2) do |config|
  # Vagrant box from Hashicorp
  config.vm.box = VAGRANT_BOX
  # Actual machine name
  config.vm.hostname = VM_NAME
  # Set VM name in Virtualbox
  config.vm.provider "virtualbox" do |v|
    v.name = VM_NAME
    v.memory = 6000
    v.cpus = "1"
  end
  if Vagrant.has_plugin?("vagrant-vbguest")
    config.vbguest.auto_update = false
  end
  # set up a private network in case you require to access something in local with private endpoint
  config.vm.network "private_network", ip: "192.168.56.9"
  opts = {
        type: 'nfs',
        linux__nfs_options: ['no_root_squash'],
        map_uid: 0,
        map_gid: 0
  }
  config.vm.synced_folder SYNCED_FOLDER, VM_SYNCED_FOLDER, opts
  # install the packages required
  config.vm.provision "shell", inline: <<-SHELL
          apt-get update
          apt-get install -y pkg-config
          apt-get install -y git
          apt-get install -y ca-certificates
          apt-get install -y curl
          apt-get install -y gnupg
          apt-get install -y cmake gcc g++
          apt-get install -y libzmq3-dev
          apt-get install -y libev-dev
          apt-get install -y python3.8
          apt-get install -y network-manager
          apt-get install -y net-tools
          apt-get install autoconf -y
          apt-get install -y zsh
          apt-get install -y build-essential
          apt-get install -y wget
          apt-get install -y libcurl4-openssl-dev
          apt-get install -y libmcrypt-dev
          apt-get install -y libreadline-dev
          apt-get install -y libxslt1-dev
          apt-get install -y libxml2-dev
          apt-get install -y openssl
          apt-get install -y libssl-dev
          apt-get install -y liblzo2-dev
          apt-get install -y libpam0g-dev
          apt-get install -y sqlite3
          apt-get install -y libsqlite3-dev
          apt-get install -y zlib1g-dev
          apt-get install -y libbz2-dev
          apt-get install -y libpng-dev
          apt-get install -y libjpeg-dev
          apt-get install -y libonig-dev
          apt-get install -y libtidy-dev
          apt-get install -y libzip-dev

          sudo mkdir -m 0755 -p /etc/apt/keyrings
          curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
          echo \
          "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
          "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
          sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

          apt-get update

          apt-get install -y docker-ce
          apt-get install -y docker-ce-cli
          apt-get install -y containerd.io
          apt-get install -y docker-buildx-plugin
          apt-get install -y docker-compose-plugin

          sudo usermod -aG docker vagrant

          newgrp docker

          sudo chmod 666 /var/run/docker.sock

          curl -L https://raw.githubusercontent.com/phpenv/phpenv-installer/master/bin/phpenv-installer | bash

          #bash
          echo 'export PATH="$HOME/.phpenv/bin:$PATH"' >> ~/.bashrc
          echo 'eval "$(phpenv init -)"' >> ~/.bashrc
          source ~/.bashrc

          #zsh
          echo 'export PATH="$HOME/.phpenv/bin:$PATH"' >> ~/.zshenv
          echo 'eval "$(phpenv init -)"' >> ~/.zshenv
          source ~/.zshenv

  SHELL
end
