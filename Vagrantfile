VAGRANT_COMMAND = ARGV[0]

Vagrant.configure("2") do |config|
  config.vm.provider "virtualbox" do |v|
    v.memory = 4096
    v.cpus = 3
  end
  config.vm.box = "bento/ubuntu-18.04"
  config.vm.network "private_network", ip: "192.168.44.44"
  
  if VAGRANT_COMMAND == "ssh"
    config.ssh.username = 'panda'
  end
  
  config.vm.provision "shell", privileged: true, inline: <<-EOF
    adduser --disabled-password --gecos "" panda
    usermod -aG sudo panda
    mkdir -p /home/panda/.ssh/
    cp /home/vagrant/.ssh/authorized_keys /home/panda/.ssh/authorized_keys
    chown -R panda:panda /home/panda
  EOF
end

