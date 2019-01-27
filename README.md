# nginx
Csak hogy okosodjunk.
Az alábbi cfg fájlban egy alap wordpress eléréshez szükséges beállítás található meg (site-a).

Ubuntu 18.04 telepítése
-----------------------
/etc/apt/sources.list
  deb http://archive.ubuntu.com/ubuntu bionic main
  deb http://archive.ubuntu.com/ubuntu bionic-security main
  deb http://archive.ubuntu.com/ubuntu bionic-updates main
  deb http://archive.ubuntu.com/ubuntu bionic universe
  deb http://archive.ubuntu.com/ubuntu bionic-updates universe
  deb http://archive.ubuntu.com/ubuntu bionic multiverse
  deb http://archive.ubuntu.com/ubuntu bionic-updates multiverse

general setup
  apt update && apt upgrade
  apt install nano mc htop ssh openssh-server
  apt -y install ntp ntpdate
  apt remove apparmor
  apt autoremove
  apt autoclean
  dpkg-reconfigure dash
  apt install nginx
  sudo mkdir -p /var/www/sites-a
  
mariadb 10.3 telepítése
  sudo apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0xF1656F24C74CD1D8
  sudo add-apt-repository 'deb [arch=amd64,arm64,ppc64el] http://ftp.utexas.edu/mariadb/repo/10.3/ubuntu bionic main'
  sudo apt update
  sudo apt install mariadb-server mariadb-client
  mysql -u root -p
    mysql> CREATE DATABASE wordpress CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;
    mysql> GRANT ALL ON wordpress.* TO 'adalon'@'localhost' IDENTIFIED BY '4d4l0n';
    mysql> FLUSH PRIVILEGES;
    mysql> EXIT;
  
php7.2 telepítése
  sudo apt install php7.2-cli php7.2-fpm php7.2-mysql php7.2-json php7.2-opcache php7.2-mbstring php7.2-xml php7.2-gd php7.2-curl

wordpress letöltése és telepítése

  wget https://hu.wordpress.org/latest-hu_HU.tar.gz
