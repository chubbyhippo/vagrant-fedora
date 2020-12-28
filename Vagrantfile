Vagrant.configure("2") do |config|
  config.vm.box = "fedora/33-cloud-base"
  config.vm.box_version = "33.20201019.0"
  config.vm.network "forwarded_port", guest: 3000, host: 3000
  config.vm.provider "virtualbox" do |v|
    v.memory = 4096
    v.cpus = 4
  end
  config.vm.provision "shell", inline: <<-SHELL
    sudo dnf update -y
    sudo dnf groupinstall "Development Tools" -y
    sudo runuser -l vagrant -c "curl https://raw.githubusercontent.com/chubbyhippo/vimrc/master/.vimrc -o ~/.vimrc"
    sudo runuser -l vagrant -c "ssh-keygen -t ed25519 -f /home/vagrant/.ssh/id_ed25519 -q -N ''"

    # install java
    sudo dnf install java -y

    # install nodejs
    curl -sL https://rpm.nodesource.com/setup_12.x | sudo bash -
    sudo dnf module install nodejs:14/default -y
    sudo runuser -l vagrant -c "mkdir ~/.npm-global"
    sudo runuser -l vagrant -c "npm config set prefix '~/.npm-global'"
    sudo runuser -l vagrant -c "echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.bashrc"
    sudo runuser -l vagrant -c "source ~/.bashrc"  
    
    # install docker
    curl -fsSL https://get.docker.com -o get-docker.sh
    sudo sh get-docker.sh
    rm get-docker.sh
    sudo usermod -aG docker vagrant
    sudo systemctl enable docker
    sudo curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
    sudo chmod +x /usr/local/bin/docker-compose
    sudo reboot

  SHELL
end