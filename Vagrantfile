VAGRANT_COMMAND = ARGV[0]

$set_environment_variables = <<SCRIPT
tee "/etc/profile.d/myvars.sh" > "/dev/null" <<EOF
export M2_HOME=/home/panda/maven
export M2=/home/panda/maven/bin
export PATH=$PATH:/home/panda/maven/bin
export AWS_ACCESS_KEY_ID='ENTER_YOUR_ID_HERE'
export AWS_SECRET_ACCESS_KEY='ENTER_YOUR_KEY_HERE'
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
    chown -R panda:docker /var/run/docker.sock /usr/local/bin/
  EOF

  config.vm.provision "shell", privileged: true, inline: <<-EOF
    echo "deb http://ppa.launchpad.net/ansible/ansible/ubuntu trusty main" >> /etc/apt/sources.list
    apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 93C4A3FD7BB9C367
    apt-get update
    apt-get install -y default-jre ansible unzip
    chown -R panda:panda /etc/ansible/
    sudo su panda
    cd /home/panda
    curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
    unzip awscliv2.zip
    sudo ./aws/install
    wget https://releases.hashicorp.com/terraform/0.13.2/terraform_0.13.2_linux_amd64.zip
    unzip terraform_0.13.2_linux_amd64.zip
    sudo mv terraform /bin/
    wget http://ftp.man.poznan.pl/apache/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.tar.gz
    tar -xvf apache-maven-3.6.3-bin.tar.gz
    mv apache-maven-3.6.3 maven
    rm apache-maven-3.6.3-bin.tar.gz awscliv2.zip terraform_0.13.2_linux_amd64.zip
    git clone https://github.com/PandaAcademy/panda_env
    git checkout feature/infrastructure
    cd panda_env
    ./start.sh
  EOF
end
