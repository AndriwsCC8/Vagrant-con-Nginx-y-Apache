Vagrant.configure("2") do |config|

    # Configuración de la primera máquina (Apache 1)
    config.vm.define "apache1" do |apache1|
      apache1.vm.box = "ubuntu/bionic64" # Ubuntu 18.04
      apache1.vm.hostname = "apache1"
      apache1.vm.network "private_network", ip: "192.168.56.10"
      apache1.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      apache1.vm.synced_folder "./html", "/var/www/html"
      apache1.vm.provision "shell", inline: <<-SHELL
        apt update
        apt install -y apache2
        echo "<h1>Servidor Apache 1</h1>" > /var/www/html/index.html
        systemctl restart apache2
      SHELL
    end
  
    # Configuración de la segunda máquina (Apache 2)
    config.vm.define "apache2" do |apache2|
      apache2.vm.box = "ubuntu/bionic64"
      apache2.vm.hostname = "apache2"
      apache2.vm.network "private_network", ip: "192.168.56.11"
      apache2.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      apache2.vm.synced_folder "./html", "/var/www/html"
      apache2.vm.provision "shell", inline: <<-SHELL
        apt update
        apt install -y apache2
        echo "<h1>Servidor Apache 2</h1>" > /var/www/html/index.html
        systemctl restart apache2
      SHELL
    end
  
    # Configuración de la tercera máquina (Nginx Load Balancer)
    config.vm.define "nginx" do |nginx|
      nginx.vm.box = "ubuntu/bionic64"
      nginx.vm.hostname = "nginx"
      nginx.vm.network "private_network", ip: "192.168.56.12"
      nginx.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      nginx.vm.provision "shell", inline: <<-SHELL
        apt update
        apt install -y nginx
        cat <<EOF > /etc/nginx/conf.d/load_balancer.conf
        upstream backend {
            server 192.168.56.10;
            server 192.168.56.11;
        }
        server {
            listen 80;
            location / {
                proxy_pass http://backend;
            }
        }
        EOF
        systemctl restart nginx
      SHELL
    end
  
  end
  