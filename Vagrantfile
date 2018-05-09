BOX_IMAGE = "ubuntu/trusty64"
WEB_COUNT = 2

Vagrant.configure("2") do |config|
    config.vm.provision :docker
    config.vm.provision :docker_compose
    config.vm.box = BOX_IMAGE
    config.vm.define "svmb1" do |subconfig|
      subconfig.vm.hostname = "svmb1"
      subconfig.vm.network :private_network, ip: "192.168.1.10"
	    subconfig.vm.synced_folder "tandem1", "/home/vagrant"
      subconfig.vm.provision "shell", inline: <<-SHELL
      mkdir /docker-mail
      cd /docker-mail
      cp /home/vagrant/docker-compose.yml /docker-mail/docker-compose.yml
      sudo docker pull tvial/docker-mailserver:latest
      sudo curl -o /docker-mail/setup.sh https://raw.githubusercontent.com/tomav/docker-mailserver/master/setup.sh
      sudo chmod a+x /docker-mail/setup.sh
      sudo docker-compose up -d mail
      sudo /bin/sh /docker-mail/setup.sh email list
	  sudo touch /docker-mail/config/postfix-accounts.cf
	  sudo /docker-mail/setup.sh email add test@tandem1.nitinankeel.ch 1234
      SHELL
      config.vm.provider "virtualbox" do |v|
        v.name = "svmb1"
        v.memory = 1024
      end

    end
  config.vm.provision "shell", inline: <<-SHELL
  echo "hallo"
  SHELL

end