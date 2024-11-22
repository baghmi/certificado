Vagrant.configure("2") do |config|
  config.vm.define "ismael" do |ismael|
    ismael.vm.box = "debian/bookworm64"
    ismael.vm.network "private_network", ip: "192.168.57.102"
    ismael.vm.provision "shell", inline: <<-SHELL
      apt update
      apt install -y nginx git openssl ufw

      #Creando primera web git
      mkdir -p /var/www/ismael/html
      git clone https://github.com/cloudacademy/static-website-example /var/www/ismael/html
      chown -R www-data:www-data /var/www/ismael/html
      chmod -R 755 /var/www/ismael
      cp -v /vagrant/ismael-sites /etc/nginx/sites-available/ismael
      ln -s /etc/nginx/sites-available/ismael /etc/nginx/sites-enabled/

      # Acceso seguro
      ufw allow ssh
      ufw allow 'Nginx Full'
      ufw delete allow 'Nginx HTTP'
      ufw --force enable
      openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/ismael.key -out /etc/ssl/certs/ismael.crt -subj "/C=ES/ST=Andalucia/L=Granada/O=Organization/OU=OrgUnit/CN=ismael"

      systemctl restart vsftpd
      systemctl restart nginx
      systemctl status nginx
    SHELL
  end
end
