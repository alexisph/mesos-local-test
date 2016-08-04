# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "peru/ubuntu-16.04-server-amd64"
  config.vm.host_name = "mesos-test"

  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: "192.168.121.138"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  config.vm.synced_folder ".", "/vagrant", disabled: true

  # KVM
  config.vm.provider "libvirt" do |v|
    # v.host = "localhost"
    # v.username = ""
    # v.password = ""
    # v.connect_via_ssh =
    v.memory = 2048
    v.cpus = 2
    # v.nested = "false"
  end

  # Fix the no tty error (see https://github.com/Varying-Vagrant-Vagrants/VVV/issues/517)
   config.vm.provision "fix-no-tty", type: "shell" do |s|
     s.privileged = false
     s.inline = "sudo sed -i '/tty/!s/mesg n/tty -s \\&\\& mesg n/' /root/.profile"
   end

  # Update apt repos
  config.vm.provision "shell", inline: "apt-get update"
  config.vm.provision "shell", inline: "apt-get -y upgrade"

  # Install Apache Mesos and prerequisites
  config.vm.provision "shell", inline: <<-SHELL
    wget http://repos.mesosphere.com/ubuntu/pool/main/m/mesos/mesos_1.0.0-2.0.86.ubuntu1604_amd64.deb
    dpkg -i mesos_1.0.0-2.0.86.ubuntu1604_amd64.deb
    apt-get install -y -f
    apt-get install -y screen vim
  SHELL

  # ALWAYS RUN
  # Start mesos
  config.vm.provision "shell", privileged: false, run: "always", inline: <<-'SHELL'
    echo "
    startup_message off
    sessionname mesos
    screen -t mesos-master 0 sudo mesos-master --ip=192.168.121.138 --work_dir=/var/lib/mesos
    screen -t mesos-agent 1 sudo mesos-agent --master=192.168.121.138:5050 --work_dir=/var/lib/mesos
    " > mesos.screen
    screen -c mesos.screen
    screen -d mesos
  SHELL
end

