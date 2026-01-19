To round out your workshop's security arsenal, **Trivy** is the industry standard for scanning Docker images. While SonarQube looks at your code, Trivy looks at the "Operating System" inside your container (like Alpine, Ubuntu, or Debian layers) to find vulnerabilities in the system libraries.

In MNCs (Major Corporations), Trivy is usually configured to generate **JSON** or **SARIF** reports for machines to read, and **Table** or **HTML** reports for human developers.

---

### README.md: Container Security Scanning with Trivy


# Lab: Docker Image Security Scanning with Trivy

This guide provides the steps to install Trivy on an Ubuntu EC2 host and perform professional-grade vulnerability scans on Docker images.

## 1. Installation on Ubuntu
We will use the official Aqua Security repository to ensure we get the latest stable version.

```bash
# Install dependencies
sudo apt-get install wget apt-transport-https gnupg lsb-release -y

# Add the GPG Key
wget -qO - [https://aquasecurity.github.io/trivy-repo/deb/public.key](https://aquasecurity.github.io/trivy-repo/deb/public.key) | gpg --dearmor | sudo tee /usr/share/keyrings/trivy.gpg > /dev/null

# Add the Repository
echo "deb [signed-by=/usr/share/keyrings/trivy.gpg] [https://aquasecurity.github.io/trivy-repo/deb](https://aquasecurity.github.io/trivy-repo/deb) $(lsb_release -sc) main" | sudo tee -a /etc/apt/sources.list.d/trivy.list

# Update and Install
sudo apt-get update
sudo apt-get install trivy -y

```

---

## 2. Standard Scan (MNC Style)

In a professional environment, we don't just "scan everything." We filter by severity to avoid "noise" and output to files for auditing.

### Option A: The "Developer View" (Table Format)

Best for checking results quickly in the terminal.

```bash
trivy image --severity HIGH,CRITICAL <YOUR_IMAGE_NAME>

```

### Option B: The "Compliance View" (JSON/MNC Format)

MNCs use JSON to feed results into dashboards like DefectDojo or Splunk.

```bash
trivy image --format json --output security-report.json <YOUR_IMAGE_NAME>

```

### Option C: The "CI/CD Gate" (Fail the Build)

In Jenkins, we want the pipeline to **stop** if a critical bug is found.

```bash
trivy image --exit-code 1 --severity CRITICAL <YOUR_IMAGE_NAME>

```

*Note: `--exit-code 1` tells Jenkins that the task failed, which stops the deployment.*

---

## 3. Best Practices for the Workshop

1. **Ignore Unfixed:** Some vulnerabilities don't have a fix yet. To avoid frustrating your students, show them how to hide vulnerabilities that cannot be patched:
```bash
trivy image --ignore-unfixed <YOUR_IMAGE_NAME>

```


2. **Scan Filesystem:** Trivy isn't just for images! It can scan your project folder for hardcoded secrets:
```bash
trivy fs .

```


---

### Teaching Moment: Why Trivy?


Explain to your students that a "Slim" or "Alpine" base image usually has **zero** vulnerabilities, whereas a standard "Ubuntu" or "Node" base image might have **hundreds**. This is why we choose "distroless" or "minimal" images in DevSecOps.



**Final Hackathon Task Idea:** Ask students to scan their original Spring Boot image, count the vulnerabilities, and then try to rewrite their `Dockerfile` using `openjdk:17-alpine` to see if they can get the vulnerability count down to zero.

[How to scan Docker images with Trivy](https://www.youtube.com/watch?v=nXTNEpLT3N4)

This video demonstrates the practical application of Trivy for scanning Docker images, helping students visualize the vulnerability detection process and the resulting reports.


http://googleusercontent.com/youtube_content/3

```