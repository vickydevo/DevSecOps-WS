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

## Summary of Installed Versions

| Tool | Command | Expected Output |
| --- | --- | --- |
| **Java** | `java -version` | openjdk 21.x.x |
| **Maven** | `mvn -v` | Apache Maven 3.x.x |
| **Docker** | `docker --version` | Docker version 27.x.x |

---


