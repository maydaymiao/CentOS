记录在CentOS上创建VM，安装MQTT+NodeRed_Freeboard服务<br>

# CentOS
## **Catalogue**
* [1. Mosquitto](#1)
    * [1.1. Install Mosquitto](#1.1)
    * [1.2. Configure MQTT Passwords & Websockets](#1.2)
* [2. Freeboard](#2)
    * [2.1. Install Apache2 & PHP & Nodejs & Npm](#2.1)
    * [2.2. Install Freeboard](#2.2)
    

<h2 id="1">1. Mosquitto</h2>
<h3 id="1.1">1.1 Install Mosquitto</h3>
Mosquitto完整教程参考：https://www.digitalocean.com/community/tutorials/how-to-install-and-secure-the-mosquitto-mqtt-messaging-broker-on-centos-7<br>

安装mosquitto: 
```linux
sudo yum -y install epel-release
sudo yum -y install mosquitto
sudo systemctl start mosquitto
sudo systemctl enable mosquitto
```
安装好后可以用mqtt.fx测试一下<br>

<h3 id="1.2">1.5 Configure MQTT Passwords & Websockets</h3>

```linux
sudo rm /etc/mosquitto/mosquitto.conf
sudo nano /etc/mosquitto/mosquitto.conf
```
接下来在mosquitto.conf里复制粘贴以下内容：
```linux
allow_anonymous false
password_file /etc/mosquitto/passwd

listener 1883
listener 9001
protocol websockets
```

再继续输入命令重启服务，重启好试一下```netstat -tln | grep 1883```，如果服务起不来，再重启mosquitto：
```linux
sudo systemctl restart mosquitto
```
现在可以再到mqtt.fx里测试一下。

<h2 id="2">2. Freeboard</h2>
<h3 id="2.1">2.1 Install Apache2 & PHP & Nodejs & Npm</h3>

安装Apache：
```linux
sudo yum install httpd
sudo systemctl start httpd.service
sudo systemctl enable httpd.service
sudo yum install php php-mysql
sudo systemctl restart httpd.service
```

安装nodejs, npm：先到https://cbs.centos.org/koji/buildinfo?buildID=17663 里x86_64下载3个依赖包，然后FTP拷贝到服务器里，再分别安装)
```linux
rpm –ivh packagename.rpm
sudo yum -y install nodejs
node -v
npm -v
```

<h3 id="2.2">2.2 Install Freeboard</h3>
```linux
git clone https://github.com/maydaymiao/freeboard.git
cd freeboard
sudo npm -g install grunt
sudo npm install -g grunt-cli
npm install
grunt
sudo -s
cd /var/www/html
ln -s /home/maydaymiao/freeboard dashboard
```
