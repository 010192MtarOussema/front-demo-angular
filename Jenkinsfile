pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                // Récupérer le code source depuis le repository
                 checkout scm

            }
        }
        stage('Install Dependencies') {
            steps {
                // Installer les dépendances Node.js
                bat 'npm install'
            }
        }
        stage('Run Tests and Coverage') {
            steps {
                // Exécuter les tests avec génération de la couverture
                bat 'npm run test -- --watch=false --code-coverage'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                // Exécuter l'analyse SonarQube avec la configuration fournie
                withSonarQubeEnv('SonarQube') {
                    bat 'npm run sonar'
                }
            }
        }
    }
    post {
        always {
            echo 'Pipeline terminé.'
        }
        success {
            echo 'Analyse SonarQube réussie.'
        }
        failure {
            echo 'Erreur dans le pipeline ou dans l\'analyse SonarQube.'
        }
    }
}
