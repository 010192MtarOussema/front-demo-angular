pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                echo 'Checking out code...'
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installing dependencies...'
                bat 'npm install'
            }
        }

        stage('Run Unit Tests') {
            steps {
                echo 'Running unit tests with coverage...'
                bat 'npm run test -- --watch=false --code-coverage'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo 'Running SonarQube analysis...'
                withSonarQubeEnv('MySonarQubeServer') { // Nom défini dans Jenkins
                    bat '''
                        sonar-scanner.bat ^
                        -Dsonar.projectKey=angular-sonar-demo ^
                        -Dsonar.sources=src ^
                        -Dsonar.exclusions=**/*.spec.ts,**/node_modules/** ^
                        -Dsonar.tests=src ^
                        -Dsonar.test.inclusions=**/*.spec.ts ^
                        -Dsonar.javascript.lcov.reportPaths=coverage/lcov.info
                    '''
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline terminé quelle que soit l'issue."
        }
        success {
            echo "Pipeline exécuté avec succès."
        }
        failure {
            echo "Pipeline échoué."
        }
    }
}
