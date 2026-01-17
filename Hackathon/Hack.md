A 7-hour hackathon is the perfect way to transition from "learning" to "doing." Since they have already built a Jenkins pipeline for a Spring Boot app, the best hackathon challenge is to **"Secure a Broken Application."**

Instead of giving them a clean project, give them one that is intentionally "dirty" (full of vulnerabilities) and ask them to fix it and automate the security gate.

---

## Hackathon Theme: "The Secure DevOps Sprint"

### 1. The Setup (Before the Hackathon)

Give every team a GitHub repository containing:

1. **A "Vulnerable" Spring Boot Project:**
* Include an old version of Log4j or Spring (for SCA).
* Hardcode a dummy AWS Secret or Database password (for SAST).
* Write a "Bad" Dockerfile (running as `root`, using a heavy/unstable base image).


2. **A Blank `Jenkinsfile`:** They must write the pipeline from scratch.

---

### 2. The Tasks (7-Hour Schedule)

#### Phase 1: Pipeline Automation (2.5 Hours)

**Task:** Build a Jenkins Pipeline that automates the entire flow you taught.

* **Goal:** The pipeline must include Stages for: Build → SCA (Dependency-Check) → SAST (SonarQube) → Docker Build & Scan.
* **Success Criteria:** The pipeline should "Fail" automatically because the code is vulnerable.

#### Phase 2: Remediation - "Fixing the Holes" (2.5 Hours)

**Task:** Students must now fix the issues reported by the tools.

* **SCA Task:** Update the `pom.xml` to versions that have no "Critical" CVEs.
* **SAST Task:** Remove hardcoded secrets and fix "Code Smells" in SonarQube.
* **Docker Task:** Optimize the Dockerfile (use multi-stage builds and a non-root user).
* **Success Criteria:** The Jenkins pipeline should now "Pass" all security gates.

#### Phase 3: The "Final Boss" Challenge (2 Hours)

**Task:** Add a new security layer they *haven't* practiced yet.

* **Challenge Options (Pick one):**
* **Trivy Image Scanning:** Integrate `trivy` into Jenkins to fail the build if the Docker OS image has vulnerabilities.
* **DAST Light:** Run a baseline OWASP ZAP scan against the containerized app.
* **Infrastructure as Code (IaC) Scan:** Give them a simple Terraform file and ask them to scan it using `tfsec` or `checkov`.



---

### 3. Scoring Criteria (The "Leaderboard")

| Criteria | Points |
| --- | --- |
| **Pipeline Stability** | 20 pts (Does it run without manual intervention?) |
| **SCA Remediation** | 20 pts (Did they fix all 'High' vulnerabilities?) |
| **SAST Cleanup** | 20 pts (Is the SonarQube dashboard 'Green'?) |
| **Docker Optimization** | 20 pts (Smallest image size + non-root user?) |
| **Bonus Challenge** | 20 pts (Successful integration of a new tool like Trivy) |

---

### 4. Implementation Steps for the Teacher

1. **Environment:** Ensure every team has their own EC2 instance or a isolated workspace in Jenkins.
2. **The "Vulnerability List":** Provide a list of "Known Issues" they need to find, but don't tell them *how* to fix them.
3. **Mentorship:** For the first 4 hours, answer questions. For the last 3 hours, stop helping to see how they solve problems under pressure.

---

### 5. Final Presentation (Last 30 Mins)

Each team shows their:

1. **Green Jenkins Pipeline.**
2. **SonarQube Dashboard** (Comparison of Before vs. After).
3. **Docker Image Size** (Comparison of their optimized image vs. the original).

**Would you like me to generate a "Vulnerable Dockerfile" and "Vulnerable pom.xml" that you can give to your students for this hackathon?**