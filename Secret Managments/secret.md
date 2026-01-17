1. Where to place the "Leaked" SecretInstead of HelloController.java, place your secret in a file that Spring Boot naturally uses for configuration. This makes the demo feel more realistic.Option A: The Standard Config FileCreate or modify src/main/resources/application.properties (or application.yml).Properties# src/main/resources/application.properties
server.port=8080
# !!! LEAKED SECRET FOR DEMO !!!
<!-- aws.access.key=<>>
aws.secret.key=><> -->
Option B: A Dedicated "Leaked" FileCreate a new file called secrets.txt or config/api-keys.json in your root directory. This allows you to show that security scanners find secrets anywhere in the project, not just in Java files.2. How to Detect the Leaked Secret (The Demo)Since you are using Docker and Jenkins, you should show detection in two ways:A. Manual Detection (CLI)Run Gitleaks via Docker on your laptop to show the audience how it generates a report:Bashdocker run --rm -v ${PWD}:/path zricethezav/gitleaks:latest detect --source="/path" -v
What to show: Point out the "Match" (the secret itself) and the "File" where it was found.B. Automated Detection (Jenkins Pipeline)Add a stage to your Jenkinsfile. This is the "DevSecOps" part—showing that the build fails automatically if a secret is found.Groovystage('Secret Scanning') {
    steps {
        // This will exit with code 1 if a leak is found, failing the Jenkins job
        sh "docker run --rm -v ${WORKSPACE}:/path zricethezav/gitleaks:latest detect --source='/path' --exit-code 1"
    }
}
3. How to Remove the Secrets PermanentlySimply deleting the line and committing again is not enough because the secret remains in the Git History.1Step 1: Immediate Rotation (The First Rule)Tell your audience: "The moment a key is leaked, it must be considered compromised. Revoke the key in the AWS/Service dashboard first."Step 2: Cleaning the History (The Professional Fix)To actually "scrub" the secret from your project history so it can't be found by scanning old commits, use the BFG Repo-Cleaner or git-filter-repo.2Create a file called passwords.txt and put the secret string inside it.Run the cleaner:Bash# Using BFG (simple for workshops)
bfg --replace-text passwords.txt
Final Cleanup:Bashgit reflog expire --expire=now --all && git gc --prune=now --aggressive
Summary Checklist for your WorkshopActionToolPurposeCommit Secretapplication.propertiesCreate the vulnerability.Scan RepoGitleaks (Docker)Detect the vulnerability.Fail BuildJenkinsEnforce security policy.Clean Historygit-filter-repoPermanently remove the leak.