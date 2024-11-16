Vagrant.configure("2") do |config|
  # Define VM 1
  config.vm.define "vm1" do |vm1|
    vm1.vm.box = "generic/ubuntu2204"
    vm1.vm.box_version = "4.3.12"
    vm1.vm.network "private_network", ip: "192.168.56.3"
    vm1.vm.network "public_network", bridge: "wlp0s20f3"
    vm1.vm.provider "virtualbox" do |vb|
      vb.memory = "2048"
    end
  end

  # Define VM 3
  config.vm.define "vm3" do |vm3|
    vm3.vm.box = "generic/ubuntu2204"
    vm3.vm.box_version = "4.3.12"
    vm3.vm.network "private_network", ip: "192.168.56.5"
    vm3.vm.network "public_network", bridge: "wlp0s20f3"
    vm3.vm.provider "virtualbox" do |vb|
      vb.memory = "2048"
    end
  end
  # Define VM 2
  config.vm.define "vm2" do |vm2|
    vm2.vm.box = "generic-x64/centos9s"
    vm2.vm.box_version = "4.3.12"
    vm2.vm.network "private_network", ip: "192.168.56.4"
    vm2.vm.network "public_network", bridge: "wlp0s20f3"
    vm2.vm.provider "virtualbox" do |vb|
      vb.memory = "2048"
    end

    vm2.vm.provision "shell", inline: <<-SHELL
      #!/bin/bash
      sudo rpm --import https://yum.corretto.aws/corretto.key
      sudo curl -L -o /etc/yum.repos.d/corretto.repo https://yum.corretto.aws/corretto.repo

      sudo yum install -y java-17-amazon-corretto-devel wget -y

      mkdir -p /opt/nexus/   
      mkdir -p /tmp/nexus/                           
      cd /tmp/nexus/
      NEXUSURL="https://download.sonatype.com/nexus/3/latest-unix.tar.gz"
      wget $NEXUSURL -O nexus.tar.gz
      sleep 10
      EXTOUT=`tar xzvf nexus.tar.gz`
      NEXUSDIR=`echo $EXTOUT | cut -d '/' -f1`
      sleep 5
      rm -rf /tmp/nexus/nexus.tar.gz
      cp -r /tmp/nexus/* /opt/nexus/
      sleep 5
      useradd nexus
      chown -R nexus.nexus /opt/nexus 
      cat <<EOT>> /etc/systemd/system/nexus.service
      [Unit]                                                                          
      Description=nexus service                                                       
      After=network.target                                                            
                                                                        
      [Service]                                                                       
      Type=forking                                                                    
      LimitNOFILE=65536                                                               
      ExecStart=/opt/nexus/$NEXUSDIR/bin/nexus start                                  
      ExecStop=/opt/nexus/$NEXUSDIR/bin/nexus stop                                    
      User=nexus                                                                      
      Restart=on-abort                                                                
                                                                        
      [Install]                                                                       
      WantedBy=multi-user.target                                                      
      EOT

      echo 'run_as_user="nexus"' > /opt/nexus/$NEXUSDIR/bin/nexus.rc
      systemctl daemon-reload
      systemctl start nexus
      systemctl enable nexus
    SHELL
  end

end
