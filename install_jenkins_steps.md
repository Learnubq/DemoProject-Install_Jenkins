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

**Because we are installing Jenkins as a Docker container on the droplet server, we don't need to install Java or Jenkins directly - all we need is a Docker runtime

4. **I then installed Docker runtime on the droplet:**

```bash
docker
apt update
apt install docker.io 
docker
```

5. **I could then install a Jenkins container on the droplet:**

```bash
docker run -p 8080:8080 -p 50000:50000 -d \
-v jenkins_home:/var/jenkins_home jenkins/jenkins:lts
docker ps
```

**I could see port 8080 was exposed on the server, so I could then access Jenkins from the browser.

```bash
<IP address of droplet>:8080
```

**I entered the Docker container of Jenkins on the droplet server:**

```bash
docker exec -it <container ID> bash
```

**Here I could see I was logged in as a Jenkins service user and not the root user - good security practice**

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
