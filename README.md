# SonarQube

- Download SonarQube from the official site
- Install Java 11+ (required for SonarQube to run)
- Start SonarQube using:
```bash
./bin/linux-x86-64/sonar.sh start  # Linux/Mac
./bin/windows-x86-64/StartSonar.bat  # Windows
```

 - Access it at http://localhost:9000 (default credentials: admin/admin)

 - SonarScanner for JavaScript/Node.js
 ```bash
 npm install -g sonarqube-scanner
```


## Setting Up a Node.js Project

- Initialize a project:
```bash
mkdir sonar-node-demo && cd sonar-node-demo
npm init -y
```

- Add ESLint for code analysis:
```bash
npm install eslint --save-dev
npx eslint --init
```

- Configuring SonarQube:
- Create a `sonar-project.properties`file in the root directory:
```
sonar.projectKey=sonar-node-demo
sonar.projectName=SonarQube NodeJS Demo
sonar.sources=.
sonar.host.url=http://localhost:9000
sonar.language=js
sonar.sourceEncoding=UTF-8
sonar.token=sqa_***************b527 # Generate a token from SonarQube
sonar.exclusions=node_modules/**,dist/**
```

- Running SonarQube Analysis
```bash
sonar-scanner
```

## CI/CD
1. GitHub Actions
- Create a `.github/workflows/sonarqube.yml` file:

```
name: SonarQube Analysis

on:
  push:
    branches:
      - main

jobs:
  sonarQubeScan:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Set up Node.js
        uses: actions/setup-node@v3
        with:
          node-version: 18

      - name: Install dependencies
        run: npm install

      - name: Run SonarQube Analysis
        env:
          SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
        run: |
          npx sonar-scanner \
            -Dsonar.projectKey=sonar-node-demo \
            -Dsonar.host.url=http://localhost:9000 \
            -Dsonar.login=$SONAR_TOKEN
```

2. Jenkins (Pipeline)
- Use the SonarQube Scanner Plugin in `Jenkinsfile`:

```
pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/your-repo.git'
            }
        }

        stage('SonarQube Analysis') {
            environment {
                SONAR_TOKEN = credentials('sonarqube-token')
            }
            steps {
                sh 'sonar-scanner -Dsonar.projectKey=sonar-node-demo -Dsonar.host.url=http://localhost:9000 -Dsonar.login=$SONAR_TOKEN'
            }
        }
    }
}
```