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



previous [Setting up an ELK stack on another device for log analysis](https://github.com/SomeTech01/Dshield-Honeypot-AWS-setup/blob/main/ELK%20stack%20-%20local.md)



References:

Dr. Johannes Ullrich - https://www.youtube.com/watch?v=fMqhoNnyvmE

Dshield git page - https://github.com/DShield-ISC/dshield

Dshield website - https://dshield.org/

Guy Bruneau - https://github.com/bruneaug/DShield-SIEM?tab=readme-ov-file

15HzMonitor - https://github.com/15HzMonitor/Internship-Blog-Post/blob/main/1.%20AWS%20DShield%20Sensor%20Setup.md