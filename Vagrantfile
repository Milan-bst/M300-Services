Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/xenial64"
  config.vm.network "forwarded_port", guest:80, host:187, auto_correct: true
  config.vm.network "forwarded_port", guest:22, host:2222, auto_correct: true
  config.vm.synced_folder ".", "/var/www/html"  
config.vm.provider "virtualbox" do |vb|
  vb.memory = "2048"  
end
config.vm.provision "shell", inline: <<-SHELL
	sudo apt-get update #Update der VM
	sudo apt-get -y install apache2 #Installieren von Apache2
	sudo ufw --force enable #Firewall einschalten
	sudo ufw allow 22 #Port 22 erlauben
	sudo ufw allow 2222 #Port 2222 erlauben
	sudo systemctl start ssh #SSH neustarten
	#Im sshd_config File zwei Einträge mit Ja ersetzen.
	sudo sed -i "s/.*PasswordAuthentication.*/PasswordAuthentication yes/g" /etc/ssh/sshd_config
	sudo sed -i "s/.*ChallengeResponseAuthentication.*/ChallengeResponseAuthentication yes/g" /etc/ssh/sshd_config
	sudo chown -c vagrant /var/mail #/Var/mail Folder dem user Vagrant geben.
	sudo chmod -R 700 /var/mail #Alle anderen User blockieren von dem Verzeichnis
	sudo apt-get -y install nginx #Nginx Reverse-Proxy installieren
	sudo unlink /etc/nginx/sites-enabled/default #Deaktivieren des virtuelen Hosts.
	sudo touch /etc/nginx/sites-available/reverse-proxy.conf #Config File für Reverse Proxy erstellen.
	#Konfigurationen in das File eintragen.
	cat <<%EOF% | sudo tee -a /etc/nginx/sites-available/reverse-proxy.conf
	server {
		listen 187;
		location / {
			proxy_pass http://127.0.0.1;
		}
	}
%EOF%
	sudo ln -s /etc/nginx/sites-available/reverse-proxy.conf /etc/nginx/sites-enabled/reverse-proxy.conf
	sudo systemctl stop apache2 #Apache2 Service anhalten
	sudo systemctl restart nginx #Nginx Server neustarten
	sudo reboot #VM neustarten
SHELL
end	