pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }
        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }
        stage('Run Tests and Coverage') {
            steps {
                bat 'npm run test -- --watch=false --code-coverage'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                bat 'npm run sonar'
            }
        }
    }

    post {
        success {
            echo 'Analyse SonarQube réussie.'
        }
        failure {
            echo 'Erreur dans l\'analyse SonarQube.'
        }
    }
}
