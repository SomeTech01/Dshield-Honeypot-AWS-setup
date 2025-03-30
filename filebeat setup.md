# Filebeat setup to forward logs

Now we need a glue to bind those two services we created.

1. On your honeypot instance type in the following command to install filebeat:

```bash
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -

sudo apt-get install apt-transport-https

echo "deb https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-8.x.list

echo "deb https://artifacts.elastic.co/packages/oss-8.x/apt stable main" |sudo tee -a /etc/apt/sources.list.d/elastic-8.x.list

sudo apt-get update && sudo apt-get install filebeat
```

![1](/screenshots/filebeat/1.png)

![1](/screenshots/filebeat/2.png)

2. Clone the Dshield repository and make a copy of the filebeat.yml

![1](/screenshots/filebeat/3.png)

3. Use your favorite text editor and edit the hosts entry under Output Event to reflect your local SIEM's IP.
	
    !! I did a few trial and error to make this work. What worked for me was:

	a. using the Virtual Network Editor to create a VM network the same as my home network

	b. switching over to bridged mode and port forwading the SIEM's kibana port!!


![1](/screenshots/filebeat/4.png)

4.  Test filebeat:
```bash
sudo su 

filebeat test config
```

![1](/screenshots/filebeat/5.png)

5.Start filebeat and softflowd:
```bash
sudo systemctl enable filebeat
sudo systemctl start filebeat
sudo systemctl status filebeat
sudo systemctl enable softflowd
sudo systemctl start softflowd
sudo systemctl status softflowd
```
![1](/screenshots/filebeat/6.png)

6. Additionally, I added a firewall rule to only allow my honeypot to connect to my port forwarding.

7. When you are recieving data I suggest you create a dashboard that will help you pivot or drill down on an IOC of interest like this shellshock exploit attempt for an example.
![1](/screenshots/filebeat/7.png)
![1](/screenshots/filebeat/8.png)



previous [Setting up an ELK stack on another device for log analysis](https://github.com/SomeTech01/Dshield-Honeypot-AWS-setup/blob/main/ELK%20stack%20-%20local.md)



References:

Dr. Johannes Ullrich - https://www.youtube.com/watch?v=fMqhoNnyvmE

Dshield git page - https://github.com/DShield-ISC/dshield

Dshield website - https://dshield.org/

Guy Bruneau - https://github.com/bruneaug/DShield-SIEM?tab=readme-ov-file

15HzMonitor - https://github.com/15HzMonitor/Internship-Blog-Post/blob/main/1.%20AWS%20DShield%20Sensor%20Setup.md