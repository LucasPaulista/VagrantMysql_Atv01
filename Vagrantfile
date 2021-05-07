$script = <<SCRIPT
sudo apt-get -qq update

DBHOST=localhost
DBNAME=dbname
DBUSER=dbuser
DBPASSWD=dbpass

debconf-set-selections <<< "mysql-server mysql-server/root_password password $DBPASSWD"
debconf-set-selections <<< "mysql-server mysql-server/root_password_again password $DBPASSWD"
sudo apt-get -y install mysql-server
mysql -uroot -p$DBPASSWD -e "grant all privileges on $DBNAME.* to '$DBUSER'@'%' identified by '$DBPASSWD'"
systemctl enable mysql
sed -i "s/bind-address.*/bind-address = 0.0.0.0/" /etc/mysql/mysql.conf.d/mysqld.cnf
sudo service mysql restart
SCRIPT

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"
  config.vm.network "private_network", ip: "10.80.4.10"
  config.vm.network "forwarded_port", guest: 3306, host: 3306
  config.vm.provision "shell", inline: $script
end