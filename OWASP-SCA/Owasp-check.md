To automate **SCA (Software Composition Analysis)** for a Java Maven project, you don't need to install anything on the Ubuntu OS itself. Maven handles the installation automatically when you call the plugin.

Here is a structured **README.md** you can provide to your students.

---

```markdown
# Lab: Automating SCA with OWASP Dependency-Check

This guide explains how to integrate the OWASP Dependency-Check plugin into a Java Maven project to identify vulnerabilities in third-party libraries.

## 1. The Plugin Configuration
Add the following XML block to your `pom.xml` file inside the `<build><plugins>...</plugins></build>` section.

```xml
<plugin>
    <groupId>org.owasp</groupId>
    <artifactId>dependency-check-maven</artifactId>
    <version>12.0.0</version>
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

## 2. Running the Scan

You can run the scan using a simple Maven command. Navigate to your project root folder and execute:

```bash
mvn clean verify

```

*Note: Since we added the `<execution>` goal to the `verify` phase, the scan runs automatically whenever you build the project.*

**Alternatively, to run ONLY the scan without a full build:**

```bash
mvn dependency-check:check

```

---

## 3. Understanding the Results

After the command finishes, check the `target/` folder of your project:

1. **HTML Report:** Open `target/dependency-check-report.html` in a browser. It shows the CVE ID, the severity (Low/Medium/High), and the library causing the issue.
2. **Build Status:** * **BUILD SUCCESS:** No vulnerabilities were found, or the scores were below your threshold (7.0).
* **BUILD FAILURE:** One or more libraries have "High" or "Critical" vulnerabilities. Maven will stop the pipeline to prevent insecure code from moving forward.



---

## 4. Why is the first run so slow? (The NVD Update)

The first time you run this tool, it may take **10–20 minutes**.

### What is happening?

The tool is downloading the **National Vulnerability Database (NVD)** hosted by NIST (USA). This is a massive database of every known software vulnerability (CVE) since 1999.

### Why it takes time:

1. **Data Volume:** It downloads thousands of JSON records.
2. **Indexing:** It builds a local H2 database on your EC2 instance so that future scans are instant.
3. **API Rate Limiting:** NIST limits how fast the data can be downloaded to prevent their servers from crashing.

**Future runs will only take seconds because the tool only downloads "delta" (new) updates.**

```

---

### Important Hackathon Tip: The "Centralized Database"
In a real company, we don't let every Jenkins agent download the NVD (it's too slow). We set up one **Central Database** (MySQL or Postgres) and point all projects to it using:
```xml
<connectionString>jdbc:mysql://internal-db-server:3306/dependencycheck</connectionString>

```

### Troubleshooting for Students

If the download fails with a `403 Forbidden` or `Connection Timeout`, it is usually because the NIST NVD servers are overloaded. Tell them to **wait 5 minutes and try again**.

**Since you are doing this in Jenkins, would you like the specific pipeline stage code to show the "Dependency-Check Report" as a tab in the Jenkins UI?**