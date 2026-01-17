# Guide: Scanning Spring Boot App with SonarQube

This guide explains how to perform Static Application Security Testing (SAST) on a Java Spring Boot project using Maven and a Dockerized SonarQube server.

## Prerequisites
1.  **SonarQube Server:** Running on `http://<EC2-IP>:9000`
2.  **Java & Maven:** Installed on the EC2 host.
3.  **Project:** A Spring Boot application with a `pom.xml`.

---

## Step 1: Generate a SonarQube Analysis Token
To send results to the server, you need an authentication token.
1.  Log in to SonarQube (`admin` / `admin`).
2.  Go to **My Account** (top right icon) > **Security**.
3.  Under **Generate Tokens**, give it a name (e.g., `workshop-token`).
4.  Select Type: **User Token**.
5.  Click **Generate** and **COPY THE TOKEN**. (You won't see it again!)

---

## Step 2: Configure SonarQube URL
You need to tell Maven where the SonarQube server is located. You can do this by adding a `<properties>` section in your `pom.xml` or by passing it in the command line (Recommended for workshops).

---

## Step 3: Run the Scan (The Command)
Navigate to your project root (where `pom.xml` is) and run the following command. 

> **Note:** Replace `<YOUR_TOKEN>` and `<EC2_PUBLIC_IP>` with your actual details.

```bash
mvn clean verify sonar:sonar \
  -Dsonar.projectKey=my-spring-boot-app \
  -Dsonar.projectName='My Spring Boot App' \
  -Dsonar.host.url=http://<EC2_PUBLIC_IP>:9000 \
  -Dsonar.token=<YOUR_TOKEN>

```

### Why we use these flags:

* `clean verify`: Ensures the code is compiled and tests pass before scanning (Sonar needs the binaries/classes).
* `sonar:sonar`: Triggers the SonarQube analysis.
* `sonar.projectKey`: A unique ID for your project in SonarQube.
* `sonar.token`: Authenticates the scanner so it can upload the report.

---

## Step 4: View the Results

1. Wait for the terminal to show `BUILD SUCCESS`.
2. Go to your browser: `http://<EC2_IP>:9000/projects`.
3. Click on **My Spring Boot App** to see the:
* **Bugs** & **Vulnerabilities** (SAST).
* **Security Hotspots** (Code that needs review).
* **Code Smells** (Maintainability issues).

---

### Key Teaching Points for your Workshop



* **SAST vs. DAST:** Explain that this scan is **SAST** (Static) because we are analyzing the source code without running the application.
* **The "Secret" Catch:** Since you intentionally added **AWS Secrets** earlier, show the students the **Security Hotspots** or **Vulnerabilities** tab. SonarQube will flag the dummy AWS keys you pushed!
* **Quality Gates:** Show them the "Passed" or "Failed" status. Explain that in a real CI/CD pipeline (like Jenkins or GitHub Actions), a "Failed" Quality Gate would stop the code from being deployed to production.



**Would you like me to show you how to automate this scan inside a Jenkins Pipeline or a GitHub Action next?**

```