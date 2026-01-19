
# Workshop Guide: Local NVD Data Setup for Dependency-Check

This guide explains how to bypass API rate limits and "NVD API Key" requirements by using a pre-downloaded database.

## 1. Why are we using a manual Zip file?
In a professional workshop environment, if 20+ students attempt to download the NVD data directly from the official **NIST (National Institute of Standards and Technology)** servers simultaneously, the NIST firewalls will often:
* **Rate-limit** the classroom IP address.
* **Block** requests that don't have an official API Key.
* **Slow down** the scan (the NVD database is several hundred MBs).

By using the `gdown` method below, we ensure everyone has the same data instantly without getting blocked.
---

## 2. Installation & Setup

### 2.1 Install gdown
`gdown` is a tool that allows Ubuntu to download large files from Google Drive that usually require a browser "virus scan" confirmation.

```bash
sudo apt update && sudo apt install python3-pip -y
pip3 install gdown

```

---

## 2. Download and Extract NVD Data



The NVD data must be placed in a directory where the Dependency-Check plugin can access it. By default, it uses a H2 database.

### 2.1 Download from Google Drive

Replace the ID in the command below with your file ID if it differs.

```bash
# Download the zip file using its File ID
gdown 1GwVC4uUZUxIA3ukz7AGYca745H1bXju9 -O nvd-data.zip

```

### 2.2 Create Directory and Unzip

Standard practice is to store this in a dedicated data folder within your `.m2` directory or project root.

```bash
# Create the standard data directory
mkdir -p ~/.m2/repository/org/owasp/dependency-check-data/

# Unzip the content directly into the data path
# Note: Dependency-check expects the extracted H2 database files (.mv.db) here
unzip nvd-data.zip -d ~/.m2/repository/org/owasp/dependency-check-data/

```

---

## 3. Configure Maven for Offline Scan

To prevent the plugin from trying to connect to the NVD servers, you must set `autoUpdate` to `false` and point it to your local data directory.

### Option A: Command Line Execution (Recommended)

Run this command from your project root (where `pom.xml` is located):

```bash
mvn org.owasp:dependency-check-maven:check \
  -DautoUpdate=false \
  -DdataDirectory=~/.m2/repository/org/owasp/dependency-check-data/

```

### Option B: Permanent Configuration in `pom.xml`

Add this configuration to your `pom.xml` so you don't have to pass flags every time:

```xml
<plugin>
    <groupId>org.owasp</groupId>
    <artifactId>dependency-check-maven</artifactId>
    <version>9.0.9</version> <configuration>
        <autoUpdate>false</autoUpdate>
        <dataDirectory>${user.home}/.m2/repository/org/owasp/dependency-check-data/</dataDirectory>
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

## 4. Verification

1. Run the scan command from **Step 3.1**.
2. Check the logs. You should **not** see messages like "Updating NVD data" or "Requesting API Key".
3. Verify that the `target/dependency-check-report.html` is generated successfully.

---

## Important Notes

* **Data Freshness:** Since `autoUpdate` is set to `false`, your scan will only be as accurate as the data in your zip file. You will need to manually update the zip file periodically to catch new vulnerabilities.
* **File Permissions:** Ensure the user running the Maven command has read/write access to the `~/.m2/repository/org/owasp/dependency-check-data/` folder.

```

[cite_start]The file referenced is "nvd-data-11.zip"[cite: 1].

```