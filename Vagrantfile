Vagrant.configure("2") do |config|
    config.vm.define "apache1" do |apache1|
      apache1.vm.box = "ubuntu/bionic64"
      apache1.vm.network "private_network", ip: "192.168.56.101"
      apache1.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      apache1.vm.synced_folder "./apache1_files", "/var/www/html"
      apache1.vm.provision "shell", inline: <<-SHELL
        apt-get update
        apt-get install -y apache2
        systemctl restart apache2
      SHELL
    end
  
    config.vm.define "apache2" do |apache2|
      apache2.vm.box = "ubuntu/bionic64"
      apache2.vm.network "private_network", ip: "192.168.56.102"
      apache2.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      apache2.vm.synced_folder "./apache2_files", "/var/www/html"
      apache2.vm.provision "shell", inline: <<-SHELL
        apt-get update
        apt-get install -y apache2
        systemctl restart apache2
      SHELL
    end
  
    config.vm.define "nginx" do |nginx|
      nginx.vm.box = "ubuntu/bionic64"
      nginx.vm.network "private_network", ip: "192.168.56.103"
      nginx.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      nginx.vm.provision "shell", inline: <<-SHELL
        apt-get update
        apt-get install -y nginx
        cat <<EOL > /etc/nginx/conf.d/load_balancer.conf
  upstream backend {
      server 192.168.56.101;
      server 192.168.56.102;
  }
  
  server {
      listen 80;
  
      location / {
          proxy_pass http://backend;
      }
  }
  EOL
        # Reiniciar Nginx para aplicar los cambios
        systemctl restart nginx
      SHELL
    end
  end
  