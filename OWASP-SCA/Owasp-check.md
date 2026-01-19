This version of the **README.md** is organized logically for a classroom setting. It starts with the one-time setup (downloading data), moves to the project configuration, and ends with how to run and interpret the results.

---

# README.md

## Lab: Software Composition Analysis (SCA) with OWASP Dependency-Check

This lab guides you through automating security scans for Java Maven projects to identify vulnerable third-party libraries.

---

## 1. Environment Setup (One-Time Task)

To avoid the slow NVD download and API rate limits (Error 429), follow these steps to use pre-downloaded vulnerability data.

### A. Install Download Tools

Run these commands on your Ubuntu instance:

```bash
sudo apt update && sudo apt install pipx unzip -y
pipx install gdown
export PATH="$HOME/.local/bin:$PATH"

```

### B. Download & Extract NVD Data

This places the vulnerability database in the location Maven expects:

```bash
# Download the 300MB+ data zip
gdown 1GwVC4uUZUxIA3ukz7AGYca745H1bXju9 -O nvd-data.zip

# Create the data directory
mkdir -p ~/.m2/repository/org/owasp/dependency-check-maven/data

# Unzip the data
unzip nvd-data.zip -d ~/.m2/repository/org/owasp/dependency-check-maven/data

```

---

## 2. Project Configuration

Add the following plugin to your `pom.xml` file inside the `<build><plugins>` section.

> **Note:** We set `failBuildOnCVSS` to **7.0** to fail the build only if **High** or **Critical** vulnerabilities are found.

```xml
<plugin>
    <groupId>org.owasp</groupId>
    <artifactId>dependency-check-maven</artifactId>
    <version>12.2.0</version>
    <configuration>
        <failBuildOnCVSS>7.0</failBuildOnCVSS>
        <formats>
            <format>HTML</format>
            <format>JSON</format>
        </formats>
    </configuration>
    <executions>
        <execution>
            <goals>
                <goal>check</goal>
            </goals>
        </execution>
    </executions>
</plugin>

```

---

## 3. Running the Scan

Navigate to your project root (where `pom.xml` is located).

### Option A: Standard Scan (Recommended)

Since we used the manual data download, you **must** use the `-DautoUpdate=false` flag to prevent Maven from trying to connect to the internet.

```bash
mvn dependency-check:check -DautoUpdate=false

```

### Option B: Run via Build Life Cycle

If you want the scan to happen every time you build the project:

```bash
mvn clean verify -DautoUpdate=false

```

### Option C: Running with an API Key (If not using the ZIP file)

If you prefer to download fresh data and have your own API Key:

```bash
mvn dependency-check:check -DnvdApiKey=YOUR_API_KEY_HERE

```

---

## 4. Understanding the Results

### Where is the report?

After a successful scan, the report is generated here:
`springboot/target/dependency-check-report.html`

### How to interpret the Build Status:

* **BUILD SUCCESS:** No vulnerabilities found, or all scores are below 7.0 (Low/Medium).
* **BUILD FAILURE:** Found vulnerabilities with a CVSS score . You must update the affected library version to pass the build.

---

## 5. Troubleshooting

* **NoDataException:** You likely forgot to unzip the data or the path is incorrect. Ensure `odc.mv.db` exists in `~/.m2/repository/org/owasp/dependency-check-maven/data/`.
* **429 Too Many Requests:** NIST is rate-limiting your IP. Ensure you are using `-DautoUpdate=false`.
* **External Environment Error:** If `pip install` fails, ensure you used `pipx` or added `--break-system-packages`.

---

**Would you like me to show you how to add a "Publish HTML Report" step in Jenkins so the students can view these results directly in the Jenkins dashboard?**