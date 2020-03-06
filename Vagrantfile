Vagrant.configure(2) do |config|
    config.vm.box = "generic/ubuntu1804"
    #config.vm.network :forwarded_port, guest: 80, host: 8080
    #config.vm.network :forwarded_port, guest: 3306, host: 3306
    config.vm.provider "virtualbox" do |vb|
        vb.memory = 1000
        vb.cpus = 1
    end
    config.vm.synced_folder "./", "/opt/project"
    config.vm.provision "shell", inline: <<-SHELL
        set -e 

        # Fix dns resolver
        systemctl disable systemd-resolved
        systemctl stop systemd-resolved
        rm /etc/resolv.conf
        echo "nameserver 1.1.1.1" > /etc/resolv.conf

        # Install system dependencies
        echo 'debconf debconf/frontend select Noninteractive' | debconf-set-selections
        sleep 5
        apt-get update && apt-get install -y -q --no-install-recommends \
            curl \
            wget \
            apt-transport-https \
            ca-certificates \
            curl \
            gnupg-agent \
            software-properties-common \
        && rm -rf /var/lib/apt/lists/*

        # Install docker
        curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -
        apt-key fingerprint 0EBFCD88
        add-apt-repository \
           "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
           $(lsb_release -cs) \
           stable"
        apt-get update
        apt-get install -y -q docker-ce docker-ce-cli containerd.io || exit 0

        # Install docker-compose
        curl -s -L "https://github.com/docker/compose/releases/download/1.25.3/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
        chmod +x /usr/local/bin/docker-compose
    SHELL
  end