# ELK stack localy using docker

Once your dhsield sensor you would want to co-relate and analyze your data. We would need an ELK instance to do this.

1. Install an ubuntu server on your VM (I used  a 24.04.2 version for this instance). Make sure the box is updated and the necessary repositories:

```bash
sudo apt-get install ca-certificates curl gnupg network-manager txt2html
```
![1](/screenshots/elk1/1.png)

2. Download and add the Docker's GPG key , grant read permissions to the GPG key, and create the directory for the key ring with proper permissions:
```bash
    curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

    sudo chmod a+r /etc/apt/keyrings/docker.gpg

    sudo install -m 0755 -d /etc/apt/keyrings
   ```

![2](/screenshots/elk1/2.png)

3. Add the Docker repository to APT sources:
```bash	
echo \
"deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
"$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
![3](/screenshots/elk1/3.png)

4. run another update & upgrade for the newly added packages and reboot the server:
```bash
sudo apt update && sudo apt upgrade -y
```

![4](/screenshots/elk1/4.png)
![3](/screenshots/elk1/5.png)

5) Install docker, jq and pip:
```bash
sudo apt-get install -y jq docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin pip
```
![3](/screenshots/elk1/6.png)

6) Enable docker:
```bash
sudo systemctl enable docker
```
![3](/screenshots/elk1/7.png)

7) Clone the SIEM repository and change the permission for cowrie-setup.sh.
```bash
git clone https://github.com/bruneaug/DShield-SIEM.git
chmod 754 ~/DShield-SIEM/scripts/cowrie-setup.sh
```
![3](/screenshots/elk1/8.png)

8. Make another folder and transfer the parsing_tty.sh and rename_arkime_pcap.sh to the new folder. Update the scripts permissions to 754
```bash	
mkdir scripts
mv DShield-SIEM/AddOnScripts/parsing_tty.sh scripts
mv DShield-SIEM/AddOnScripts/rename_arkime_pcap.sh scripts
chmod 754 scripts/*.sh
```
![3](/screenshots/elk1/9.png)

9. edit the .env file using your favorite text editor.

![3](/screenshots/elk1/10.png)

10 . Update the password for Elastic and Kibana. Also update the Hostname and IP that reflects your SIEM's.
!!This file contains your credentials when logging in to elastic and kibana!!
![3](/screenshots/elk1/11.png)

11. Pull the images (if they aren't already pulled) and starts the containers(this will take a while if doing for the first time):
```bash
sudo docker compose up -d
```
![3](/screenshots/elk1/12.png)
![3](/screenshots/elk1/13.png)

12. Enable docker services and check it's status:
```bash
sudo systemctl enable docker.service
sudo systemctl start docker.service
sudo systemctl status docker.service
```
![3](/screenshots/elk1/15.png)

13. Check the services if running:
```bash
sudo docker container ls
```
![3](/screenshots/elk1/16.png)

14. Check the services on their ports 9200,8220,5601 and 5044
![3](/screenshots/elk1/17.png)

15. Open a browser on your main computer and access the service by going to:
	https://VMip:5601

!! If you get the Cert warning bypass and proceed !!
!!check the .env file if you forgot remember there is an assigned username!!

![3](/screenshots/elk1/18.png)

16 . Click the menu option on the left and head over Fleet > settings > pencil icon to edit

![3](/screenshots/elk1/19.png)

17 On the siem generate a SHA256 certificate:
```bash
sudo cp /var/lib/docker/volumes/dshield-elk_certs/_data/ca/ca.crt /tmp

sudo openssl x509 -fingerprint -sha256 -noout -in /tmp/ca.crt | awk -F"=" {' print $2 '} | sed s/://g
```

You will need these output to fill out CA trusted fingerprint and YAML config. (This is the window that opened when you click the pencil icon earlier)
	 
Enter http://es01:9200 for the Hosts,then Scroll down to save and apply settings.

![3](/screenshots/elk1/20.png)

18. From the page you are on click Add Fleet Server. FIll up with Name:es01 and URL: https://fleet-server:8220 . Then, click Generate Fleet Server Policy.

![3](/screenshots/elk1/21.png)

19. Scroll down, under Install Fleet Server to a centralized host, click RPM. Copy and paste the result to an editor and should look like this:

```bash
elastic-agent enroll \
	--url=https://fleet-server:8220 \
	--fleet-server-es=https://es01:9200 \
	--fleet-server-service-token='copy from your server' \
	--fleet-server-policy=fleet-server-policy \
	--fleet-server-es-ca=/certs/es01/es01.crt \
	--fleet-server-es-ca-trusted-fingerprint='copy from your server' \
	--fleet-server-port=8220 \
	--certificate-authorities=/certs/ca/ca.crt \
	--fleet-server-cert=/certs/fleet-server/fleet-server.crt \
	--fleet-server-cert-key=/certs/fleet-server/fleet-server.key \
	--elastic-agent-cert=/certs/fleet-server/fleet-server.crt \
	--elastic-agent-cert-key=/certs/fleet-server/fleet-server.key \
	--fleet-server-es-cert=/certs/fleet-server/fleet-server.crt \
	--fleet-server-es-cert-key=/certs/fleet-server/fleet-server.key
```

20. Go to the fleet-server's command line and paste the commands you saved earlier:
```bash
sudo docker exec -ti fleet-server bash 
```
![3](/screenshots/elk1/22.png)

!!Type ‘y’ if propmted about the setting changes!! 

![3](/screenshots/elk1/23.png)


21. Go to Fleet > Agents to check the status

![3](/screenshots/elk1/24.png)
!!do an : ./elastic-agent restart on the fleet-server CLI to fix the unhealthy status!!

![3](/screenshots/elk1/25.png)

22. To add integrations go to Fleet > Agent Policies > Fleet Server Policy

![3](/screenshots/elk1/26.png)

23.  Click on the Blue Add integration button

![3](/screenshots/elk1/27.png)

24. Pick out an integration 

![3](/screenshots/elk1/28.png)

25. click to blue button to add the integration

![3](/screenshots/elk1/29.png)

26. Save the changes and continue

![3](/screenshots/elk1/30.png)

27. Feel free to add integrations of choice

![3](/screenshots/elk1/31.png)


previous [Dshield Honeypot AWS setup](https://github.com/SomeTech01/Dshield-Honeypot-AWS-setup)

next [Setting up filebeat](https://github.com/SomeTech01/Dshield-Honeypot-AWS-setup/blob/main/filebeat%20setup.md)

References:

Dr. Johannes Ullrich - https://www.youtube.com/watch?v=fMqhoNnyvmE

Dshield git page - https://github.com/DShield-ISC/dshield

Dshield website - https://dshield.org/

Guy Bruneau - https://github.com/bruneaug/DShield-SIEM?tab=readme-ov-file

15HzMonitor - https://github.com/15HzMonitor/Internship-Blog-Post/blob/main/1.%20AWS%20DShield%20Sensor%20Setup.md