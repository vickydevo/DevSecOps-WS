To install these tools on Ubuntu 24.04, you need to handle Java and Maven through the standard repositories and Docker through the official Docker repository to ensure you get the latest version.


---

# Server Setup Guide: Java 21, Maven, and Docker
This guide provides a step-by-step process to configure an **Ubuntu 24.04 LTS** instance on AWS with the essential DevSecOps stack.

## 1. Update System Packages
Always start by ensuring your package index is up to date.
```bash
sudo apt update && sudo apt upgrade -y
```
---

## 2. Install Java 21 (OpenJDK)

Ubuntu 24.04 includes OpenJDK 21 in its default repositories.

```bash
sudo apt install openjdk-21-jdk -y
# For Java
sudo apt purge --autoremove openjdk-21* -y
# For Java
sudo apt purge --autoremove openjdk-21* -y

```

**Verify Java installation:**

```bash
java -version

```

---

## 3. Install Apache Maven

```bash
sudo apt install maven -y
# For Maven
sudo apt purge --autoremove maven -y

```

**Verify maven  installation:**

```bash
mvn -version

```

---

## 4. Install Docker (Official Repository)

For Ubuntu 24.04, it is best practice to use the official Docker repo rather than the default `apt` version to ensure stability.

### Step 4.1: Install Dependencies & Add GPG Key

```bash
sudo apt update
sudo apt install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

```

### Step 4.2: Add Docker Repository

```bash
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/ubuntu
Suites: $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}")
Components: stable
Signed-By: /etc/apt/keyrings/docker.asc
EOF
```

### Step 4.3: Install Docker Engine

```bash
sudo apt update -y
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
```

---

## 5. Configure Docker Permissions

To run Docker commands without using `sudo` (essential for CI/CD pipelines), add the `ubuntu` user to the docker group.

```bash
# Create the docker group (usually already exists)
sudo groupadd docker

# Add the current user to the group
sudo usermod -aG docker $USER

# Apply the group changes without logging out
newgrp docker

```

**Verify Docker works without sudo:**

```bash
docker run hello-world

```
**Installing Sonarqube Server using  Docker :**

```bash
docker run -d --name sonarqube -p 9000:9000 sonarqube:community
# OR
docker run -d --name sonarqube -p 9000:9000 sonarqube:lts-community
```
**Access Sonarqube Server using  Using browser :**

```bash
http://54.167.94.226:9000

```
default credentials 

```bash
username: admin
Password: admin 
``
<img width="1268" height="793" alt="Image" src="https://github.com/user-attachments/assets/06ce799a-54ce-4b06-aa8f-6bd286155f02" />

---

http://54.167.94.226:9000

## Summary of Installed Versions

| Tool | Command | Expected Output |
| --- | --- | --- |
| **Java** | `java -version` | openjdk 21.x.x |
| **Maven** | `mvn -v` | Apache Maven 3.x.x |
| **Docker** | `docker --version` | Docker version 27.x.x |

```

---

### Pro-Tip for your Students
If they are still getting "Permission Denied" when running `docker ps` after following the steps, they might need to **disconnect and reconnect** to their SSH session. This fully refreshes the user's group memberships in Linux.



**Would you like me to add a section for installing the SonarQube Scanner or the AWS CLI to this README as well?**

```