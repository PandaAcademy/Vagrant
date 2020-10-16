VAGRANT_COMMAND = ARGV[0]

$set_environment_variables = <<SCRIPT
tee "/etc/profile.d/myvars.sh" > "/dev/null" <<EOF
export M2_HOME=/home/panda/maven
export M2=/home/panda/maven/bin
export PATH=$PATH:/home/panda/maven/bin
EOF
SCRIPT

Vagrant.configure("2") do |config|
  config.vm.provider "virtualbox" do |v|
    v.memory = 4096
    v.cpus = 3
  end
  config.vm.box = "bento/ubuntu-18.04"
  config.vm.network "private_network", ip: "192.168.44.44"
  config.vm.provision "shell", inline: $set_environment_variables, run: "always"
  config.vm.provision :docker
  config.vm.provision :docker_compose

  if VAGRANT_COMMAND == "ssh"
    config.ssh.username = 'panda'
  end
  
  config.vm.provision "shell", privileged: true, inline: <<-EOF
    adduser --disabled-password --gecos "" panda
    usermod -aG sudo panda
    usermod -aG docker panda
    mkdir -p /home/panda/.ssh/
    cp /home/vagrant/.ssh/authorized_keys /home/panda/.ssh/authorized_keys
    chown -R panda:panda /home/panda
    chown panda:docker /var/run/docker.sock
  EOF

  config.vm.provision "shell", privileged: true, inline: <<-EOF
    apt-get update
    apt-get install -y default-jre
    sudo su panda
    cd /home/panda
    wget http://ftp.man.poznan.pl/apache/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.tar.gz
    tar -xvf apache-maven-3.6.3-bin.tar.gz
    mv apache-maven-3.6.3 maven
    rm apache-maven-3.6.3-bin.tar.gz
    docker run -d --name jenkins -p 8880:8080 -p 50000:50000 pandaacademy/jenkins:1.0
    docker run -d --name artifactory -p 8081:8081 -p 8082:8082 pandaacademy/artifactory:1.0
  EOF
end
