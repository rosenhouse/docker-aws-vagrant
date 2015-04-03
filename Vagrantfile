# -*- mode: ruby -*-
# vi: set ft=ruby :

ENV['VAGRANT_DEFAULT_PROVIDER'] = 'aws'

Vagrant.configure(2) do |config|
  config.vm.box = "dummy"

  config.vm.provider :aws do |aws, override|
    aws.access_key_id = ENV['AWS_ACCESS_KEY_ID']
    aws.secret_access_key = ENV['AWS_SECRET_ACCESS_KEY']
    aws.keypair_name = ENV['AWS_KEYPAIR_NAME']

    aws.region='us-east-1'
    aws.ami = "ami-4a5b7022" # us-east-1 ubuntu 14.04 LTS (trusty) amd64 hvm:instance-store from 2015-03-25

    aws.instance_type = "m3.large" # 2 vCPU, 7.5 GB RAM, 32 GB SSD

    aws.tags={ "Name" => "Docker host" }

    override.ssh.username = "ubuntu"
    override.ssh.private_key_path = "~/.ssh/id_rsa"

  end

   config.vm.provision "shell", inline: <<-SHELL
     set -e -x

     sudo mkdir -p /mnt/docker
     sudo mkdir -p /mnt/docker-tmp

     which docker || (wget -qO- https://get.docker.com/ | sh)

     sudo usermod -aG docker ubuntu

     sudo cp /vagrant/docker-config /etc/default/docker

     sudo restart docker
   SHELL
end
