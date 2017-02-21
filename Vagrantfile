# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.box = "centos/7"

  config.vm.network "forwarded_port", guest: 3000, host: 3000 # Grafana
  config.vm.network "forwarded_port", guest: 5601, host: 5601 # Kibana
  config.vm.network "forwarded_port", guest: 9200, host: 9200 # Elasticsearch

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "4096"
  end

  config.vm.provision "shell", inline: <<-SHELL

    cd /tmp

    # elasticsearch kibana

    sudo yum install -y https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.2.0.rpm
    sudo yum install -y https://artifacts.elastic.co/downloads/kibana/kibana-5.2.0-x86_64.rpm
    sudo yum install -y https://artifacts.elastic.co/downloads/logstash/logstash-5.2.1.rpm
    sudo yum install -y java-1.8.0 rng-tools
    sudo /usr/sbin/alternatives --set java /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.121-0.b13.el7_3.x86_64/jre/bin/java
    echo "network.host: 0.0.0.0" | sudo tee /etc/elasticsearch/elasticsearch.yml
    echo "server.host: \"0.0.0.0\"" | sudo tee /etc/kibana/kibana.yml
    sudo systemctl enable elasticsearch.service
    sudo systemctl enable kibana.service
    sudo systemctl enable rngd.service
    sudo service elasticsearch start
    sudo service kibana start

    # x-pac

    # sudo service elasticsearch stop
    # sudo service kibana stop
    # sudo /usr/share/elasticsearch/bin/elasticsearch-plugin install x-pack
    # sudo service elasticsearch start
    # sudo /usr/share/kibana/bin/kibana-plugin install x-pack
    # sudo service kibana start

    # elasticdump

    curl --silent --location https://rpm.nodesource.com/setup_6.x | bash -
    yum install -y nodejs
    npm install elasticdump -g

    # grafana

    sudo yum install -y https://grafanarel.s3.amazonaws.com/builds/grafana-4.1.2-1486989747.x86_64.rpm
    sudo systemctl enable grafana-server.service
    sudo service grafana-server start

  SHELL
  
end
