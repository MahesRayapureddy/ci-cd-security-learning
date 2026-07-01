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
                        bat "\"${scannerHome}\\bin\\sonar-scanner.bat\" -Dsonar.projectKey=ci-cd-security-learning -Dsonar.sources=."
                    }
                }
            }
        }

        stage('Trivy File Scan') {
            steps {
                bat 'trivy fs .'
            }
        }

        stage('Docker Build') {
            steps {
                bat 'docker build -t ci-cd-security-learning .'
            }
        }

        stage('Trivy Image Scan') {
            steps {
                bat 'trivy image ci-cd-security-learning'
            }
        }

        stage('Run Docker Container') {
            steps {
                bat 'docker rm -f flask-app || exit 0'
                bat 'docker run -d --name flask-app -p 5000:5000 ci-cd-security-learning'
            }
        }
    }
}
