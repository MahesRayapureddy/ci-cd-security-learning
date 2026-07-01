pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Building Project...'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    script {
                        def scannerHome = tool 'SonarScanner'
                        bat "${scannerHome}\\bin\\sonar-scanner.bat -Dsonar.projectKey=ci-cd-security-learning -Dsonar.sources=."
                    }
                }
            }
        }

        stage('Docker Build') {
            steps {
                bat 'docker build -t ci-cd-security-learning .'
            }
        }
    }
}
