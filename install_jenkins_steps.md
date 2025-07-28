## Demo Project - Install Jenkins on DigitalOcean Droplet Server

This demo shows the process of installing Jenkins on a DigitalOcean droplet server

## Steps to Install Jenkins on a DigitalOcean Droplet Server

1. **I created a droplet for Jenkins on DigitalOcean**

2. **I created a new firewall for the Jenkins droplet server**

**I opened the firewall to SSH via port 22 and added my local machine IP address to the whitelist for port 22. I also opened port 8080 via TCP as this is the port to interact with Jenkins through browser (opened it to inbound rules from all IPv4 and IPv6).

3. **I then connected to the jenkins droplet server through ssh:**

```bash
ssh root@<IPaddress of droplet>
```

<img width="466" height="538" alt="jenkins1" src="https://github.com/user-attachments/assets/5879bc5c-c4bb-4c39-851c-1a8d2dece6a9" />


**Because I was installing Jenkins as a Docker container on the droplet server, I didn't need to install Java or Jenkins directly - all I needed was a Docker runtime.

4. **I then installed Docker runtime on the droplet:**

```bash
docker
apt update
apt install docker.io 
docker
```

<img width="713" height="482" alt="jenkins2" src="https://github.com/user-attachments/assets/96a0e786-b8bb-4b71-86bb-0a5c2a7aee45" />


5. **I could then install a Jenkins container on the droplet:**

```bash
docker run -p 8080:8080 -p 50000:50000 -d \
-v jenkins_home:/var/jenkins_home jenkins/jenkins:lts
docker ps
```

<img width="956" height="441" alt="jenkins3" src="https://github.com/user-attachments/assets/beee84f8-d1e2-40a7-9594-2feea09b75a8" />


**I could see port 8080 was exposed on the server, so I could then access Jenkins from the browser.

<img width="1844" height="115" alt="jenkins4" src="https://github.com/user-attachments/assets/c4974a70-7964-4b75-9af0-9e622ca43527" />


```bash
<IP address of droplet>:8080
```

**I entered the Docker container of Jenkins on the droplet server:**

```bash
docker exec -it <container ID> bash
```

**Here I could see I was logged in as a Jenkins service user and not the root user - good security practice**


<img width="612" height="122" alt="jenkins5" src="https://github.com/user-attachments/assets/ff9b36ef-806d-4954-ba68-c2e7a72cb203" />


**I then looked for the Jenkins password by executing this path:**

```bash
cat /var/jenkins_home/secrets/initialAdminPassword
```

**I could also find the same info on the droplet server itself because the container is mounted there as a volume:**

```bash
exit
docker volume inspect <volume name>
ls /var/lib/docker/volumes/jenkins_home/_data
ls /var/lib/docker/volumes/jenkins_home/_data/secrets
cat /var/lib/docker/volumes/jenkins_home/_data/secrets/initialAdminPassword
```

**I then selected "install suggested plugins" after pasting password into the Jenkins browser UI**


<img width="915" height="948" alt="jenkins6" src="https://github.com/user-attachments/assets/279fd6fe-0c94-423a-b0a5-8d8777222d23" />



