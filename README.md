To install these tools on Ubuntu 24.04, you need to handle Java and Maven through the standard repositories and Docker through the official Docker repository to ensure you get the latest version.

Here is a professional `README.md` file you can use for your workshop.

---

```markdown
# Server Setup Guide: Java 21, Maven, and Docker
This guide provides a step-by-step process to configure an **Ubuntu 24.04 LTS** instance on AWS with the essential DevSecOps stack.

## 1. Update System Packages
Always start by ensuring your package index is up to date.

sudo apt update && sudo apt upgrade -y

```

---

## 2. Install Java 21 (OpenJDK)

Ubuntu 24.04 includes OpenJDK 21 in its default repositories.

```bash
sudo apt install openjdk-21-jdk -y

```

**Verify installation:**

```bash
java -version

```

---

## 3. Install Apache Maven

```bash
sudo apt install maven -y

```

**Verify installation:**

```bash
mvn -version

```

---

## 4. Install Docker (Official Repository)

For Ubuntu 24.04, it is best practice to use the official Docker repo rather than the default `apt` version to ensure stability.

### Step 4.1: Install Dependencies & Add GPG Key

```bash
sudo apt install ca-certificates curl gnupg -y
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL [https://download.docker.com/linux/ubuntu/gpg](https://download.docker.com/linux/ubuntu/gpg) | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

```

### Step 4.2: Add Docker Repository

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] [https://download.docker.com/linux/ubuntu](https://download.docker.com/linux/ubuntu) \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

```

### Step 4.3: Install Docker Engine

```bash
sudo apt update
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

---

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