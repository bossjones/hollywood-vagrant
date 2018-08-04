# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrant multi machine configuration

servers=[
      {
        :hostname => "hollywood-vagrant",
        :ip => "192.168.34.100",
        :ram => 2048,
        :cpu => 2
      }
]

# SOURCE: https://github.com/bossjones/docker-swarm-vbox-lab/blob/master/Vagrantfile
$bootstrap_script = <<SHELL
sudo apt-get update && \
sudo apt-get install -y locales && \
DEBIAN_FRONTEND=noninteractive sudo apt-get install -y --no-install-recommends \
apt-utils && \
DEBIAN_FRONTEND=noninteractive sudo apt-get install -y --no-install-recommends \
ca-certificates \
curl && \
sudo apt-get install -yqq --no-install-recommends \
bzip2 \
unzip \
xz-utils \
wget bzip2 \
libglib2.0-0 libxext6 libsm6 libxrender1 \
git mercurial subversion && \
sudo apt-get update && \
sudo apt-get -y upgrade && \
sudo apt-get install -y \
language-pack-en-base; \
sudo locale-gen en_US.UTF-8; \
sudo localedef -i en_US -c -f UTF-8 -A /usr/share/locale/locale.alias en_US.UTF-8; \
sudo apt-get install -y atop ; \
sudo apt-get install -y bmon ; \
sudo apt-get install -y cmatrix ; \
sudo apt-get install -y dnstop ; \
sudo apt-get install -y ethstatus ; \
sudo apt-get install -y glances ; \
sudo apt-get install -y htop ; \
sudo apt-get install -y ifstat ; \
sudo apt-get install -y iotop ; \
sudo apt-get install -y iptotal ; \
sudo apt-get install -y iptraf-ng ; \
sudo apt-get install -y itop ; \
sudo apt-get install -y jnettop ; \
sudo apt-get install -y kerneltop ; \
sudo apt-get install -y latencytop ; \
sudo apt-get install -y logtop ; \
sudo apt-get install -y netmrg ; \
sudo apt-get install -y nload ; \
sudo apt-get install -y nmon ; \
sudo apt-get install -y ntop ; \
sudo apt-get install -y powertop ; \
sudo apt-get install -y sagan ; \
sudo apt-get install -y slurm ; \
sudo apt-get install -y snetz ; \
sudo apt-get install -y top ; \
sudo apt-get install -y tiptop ; \
sudo apt-get install -y vnstat ; \
sudo apt-get install ccze bmon errno tree speedometer mlocate jp2a apg -y
SHELL

Vagrant.configure(2) do |config|

    # Get's honored normally
    config.vm.synced_folder ".", "/vagrant", disabled: true
    # But not the centos box
    config.vm.synced_folder '.', '/home/vagrant/sync', disabled: true

    servers.each do |machine|

        config.vm.define machine[:hostname] do |node|

            config.ssh.insert_key = false
            node.vm.usable_port_range = (2200..2250)
            node.vm.hostname = machine[:hostname]
            node.vm.network "private_network", ip: machine[:ip]

            # This will be applied to all vms

            # Ubuntu
            node.vm.box = "bento/ubuntu-16.04"

            # Debian 8
            # node.vm.box = "boxcutter/debian8"

            # CentOS 7
            # node.vm.box = "boxcutter/centos7"

            node.vm.provider "virtualbox" do |vb|
                vb.customize ["modifyvm", :id, "--memory", machine[:ram], "--cpus", machine[:cpu]]
                vb.name = machine[:hostname]
            end
        end

    end

  config.vm.provision :shell, inline: $bootstrap_script

  # config.vm.provision "ansible" do |ansible|
  #   ansible.limit = "all"
  #   ansible.playbook = "playbook.yml"
  #   #ansible.inventory_path = "ansible_inventory"
  #   #ansible.host_key_checking = false
  #   ansible.verbose = "vvv"
  # end

end

