# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
    config.vm.box = "ubuntu/xenial64"
    config.vm.synced_folder ".", "/vagrant", disabled: true
    config.vm.provider :virtualbox do |vb|
        vb.memory = 1024
        vb.linked_clone = true
    end

    config.vm.define "django" do |app|
        app.vm.hostname = "django.test"
        app.vm.network :private_network, ip: "192.168.10.3"
        app.vm.synced_folder "./django", "/vagrant"

        app.vm.provision "shell", inline: <<-SHELL
            sudo apt-get update
            sudo apt-get install -y python3-venv

            cd /vagrant/
            python3 -m venv venv
            source venv/bin/activate
            pip install -r requirements.txt
            python demo/manage.py migrate
            #python demo/manage.py raven test
            python demo/manage.py runserver 0.0.0.0:8000 & 
        SHELL
    end

    config.vm.define "laravel" do |app|
        app.vm.hostname = "laravel.test"
        app.vm.network :private_network, ip: "192.168.10.4"
        app.vm.synced_folder "./laravel", "/vagrant"

        app.vm.provision "shell", inline: <<-SHELL
            sudo apt-get update
            sudo apt-get upgrade
            sudo apt-get install -y apt-transport-https lsb-release ca-certificates wget

            sudo apt-get install -y python-software-properties
            sudo add-apt-repository -y ppa:ondrej/php
            sudo apt-get update -y

            sudo apt-get install -y php7.1 php7.1-cli php7.1-common php7.1-json \
            php7.1-opcache php7.1-mysql php7.1-mbstring php7.1-mcrypt \
            php7.1-zip php7.1-fpm php7.1-curl php7.1-dom

            cp -Rf /vagrant/composer_config.js /home/vagrant/.config/composer/config.json
            curl -sS https://getcomposer.org/installer | sudo php -- --install-dir=/usr/local/bin --filename=composer

            cd /vagrant
            composer install && php artisan serve --host 0.0.0.0 --port 8000 &
        SHELL
    end

    config.vm.define "springboot" do |app|
        app.vm.hostname = "springboot.test"
        app.vm.network :private_network, ip: "192.168.10.5"
        app.vm.synced_folder "./spring-boot", "/vagrant"

        app.vm.provision "shell", inline: <<-SHELL
            sudo apt-get update
            sudo apt-get -y upgrade
            sudo apt-get install -y openjdk-8-jdk vim curl

            export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/

            cd /vagrant && ./mvnw package
            SENTRY_DSN="http://c268d3e98b15450ab6b09e7a81629cc3:33c037252a1f47e29ca03e5bd29d64a8@192.168.10.1:9000/4" java -jar target/sentry-spring-boot-example-0.0.1.jar &
        SHELL
    end

end
