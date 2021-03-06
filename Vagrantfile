# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.define "node1" do |node|
    node.vm.box = "gbarbieru/xenial"
    node.vm.network "private_network", ip: "10.0.0.3", netmask: "255.255.255.0"
    node.vm.hostname = "node1"
  end

  config.vm.define "host1" do |host|

    # Use current version of CoreOS from its 'alpha' channel
    host.vm.box = "coreos-alpha"
    host.vm.box_url = "https://storage.googleapis.com/alpha.release.core-os.net/amd64-usr/current/coreos_production_vagrant.json"

    # Give the virtual machine an IP address
    host.vm.network "private_network", ip: "10.0.0.2", netmask: "255.255.255.0"

    # Set its hostname
    host.vm.hostname = "host1"

    # Give the virtual machine 3 GB of RAM
    host.vm.provider :virtualbox do |vb|
	    vb.customize ['modifyvm', :id, '--memory', '3000']
	  end

    # See https://github.com/spujadas/elk-docker/issues/92
    host.vm.provision "shell", inline: "sudo sysctl -w vm.max_map_count=262144"

    docker_network_setup = %Q{
      if [[ $(docker network ls | grep "elasticsearch_network") = "" ]]; then
        docker network create -d bridge elasticsearch_network
      fi
    }

    host.vm.provision 'shell', inline: docker_network_setup

    # Provision docker containers within the virtual machine
    host.vm.provision "docker" do |docker|
      elasticsearch_image = "docker.elastic.co/elasticsearch/elasticsearch:5.3.1"
      kibana_image = "docker.elastic.co/kibana/kibana:5.3.1"

      es_arguments = [
        '--memory 768m',                       # Max memory for the container.                  
        '-e ES_JAVA_OPTS="-Xms512m -Xmx512m"', # Override default 2GB JVM heap size to be smaller.
        '-e cluster.name=es-cluster',          # Set cluster name. Must be the same for all nodes.
        '-e http.host=_eth0_',                 # Interface to listen on for HTTP requests (port 9200).
        '-e transport.host=_eth0_',            # Interface to listen on when communicating with other nodes (port 9300).
        '-e network.host=_eth0_',              # Interface to broadcast presence to other nodes.
        '-e xpack.security.enabled=false',     # Disable default xpack authentication.
        '--net elasticsearch_network'          # Custom docker "bridge" network for ES nodes.
      ]

      # Elasticsearch node #1 - Master node, exposes port 9200
      docker.run "es1",
        image: elasticsearch_image, 
        args: es_arguments.join(" ") + " -e node.name=es1 -p 9200:9200"

      # Elasticsearch node #2
      docker.run "es2",
        image: elasticsearch_image, 
        args: es_arguments.join(" ") + " -e node.name=es2 -e discovery.zen.ping.unicast.hosts=es1"

      # Kibana
      docker.run "kibana1",
        image: kibana_image,
        args: "--memory 768m -p 5601:5601 --net elasticsearch_network -e ELASTICSEARCH_URL=http://es1:9200"
    end
  end
end
