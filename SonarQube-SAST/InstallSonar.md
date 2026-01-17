# SonarQube


## Installation Setup of SonarQube

### Running SonarQube as a Docker Container 

1. **Install Docker**: Follow the [Docker installation guide](https://docs.docker.com/get-docker/).
    ```bash
        sudo apt update -y && sudo apt install docker.io -y
    ```
2. **Pull Image**:
    ```bash
    docker pull sonarqube:lts-community
    ```
3. **Run Container**:
    
    ```bash
    docker run -d --name sonarqube -p 9000:9000 sonarqube:lts-community
    ```
    or
    ```bash
    docker run -d --name sonarqube -p 9000:9000 -e SONAR_JAVA_OPTS="-Djava.version=17" sonarqube:lts-community
    ```
    ![Image](https://github.com/user-attachments/assets/3984f69f-84c2-486f-ab10-6a61e73d5fd4)
4. **Verify**: Use `docker ps` to confirm the container is running.
5. **Access**: Navigate to `http://<instance-public-ip>:9000`.

### Logging into SonarQube

1. **Default Credentials**:
    - Username: `admin`
    - Password: `admin`
    ![Image](https://github.com/user-attachments/assets/6462c55b-b117-4387-8d97-44439ddeef88)
2. **Change Password**: Prompted on first login.
3. **Dashboard**:
    ![Image](https://github.com/user-attachments/assets/a28caa7a-b75c-46dc-9522-33a69fbe6952)

## Quality Profile in SonarQube

A **Quality Profile** defines rules for code analysis, ensuring adherence to standards.

### Features

1. **Customizable Rules**: Enable/disable rules as needed.
2. **Language-Specific**: Profiles for each language.
3. **Default Profiles**: Predefined profiles for quick use.
4. **Inheritance**: Share rules across projects.

### Importance

- Ensures consistent code quality.
- Enforces coding standards.
- Detects issues early.

### Management

- Navigate to **Quality Profiles** in the dashboard.
- Create, edit, or delete profiles.
- Assign profiles to projects.

Refer to the [Quality Profiles Documentation](https://docs.sonarsource.com/latest/analysis/quality-profiles/).

## Quality Gate in SonarQube

A **Quality Gate** is a set of conditions a project must meet for acceptable quality.

### Features

1. **Customizable Conditions**: Define metrics like code coverage, bugs, and duplications.
2. **Pass/Fail Criteria**: Projects pass if all conditions are met.
3. **Default Gate**: "Sonar Way" for basic quality checks.
4. **CI/CD Integration**: Blocks builds if the gate fails.

### Importance

- Ensures consistent quality.
- Detects issues early.
- Reduces production risks.

### Management

- Navigate to **Quality Gates** in the dashboard.
- Create, edit, or delete gates.
- Assign gates to projects.

Refer to the [Quality Gates Documentation](https://docs.sonarsource.com/latest/analysis/quality-gates/).