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
