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
                sh 'npm install'
            }
        }
        stage('Run Tests and Coverage') {
            steps {
                // Exécuter les tests avec génération de la couverture
                sh 'npm run test -- --watch=false --code-coverage'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                // Exécuter l'analyse SonarQube avec la configuration fournie
                withSonarQubeEnv('SonarQube') {
                    sh """
                      -Dsonar.projectKey=angular-sonar-demo \
                      -Dsonar.projectName=front-demo-angular \
                      -Dsonar.projectVersion=1.0 \
                      -Dsonar.sources=src \
                      -Dsonar.exclusions=**/*.spec.ts,**/node_modules/** \
                      -Dsonar.tests=src \
                      -Dsonar.test.inclusions=**/*.spec.ts \
                      -Dsonar.javascript.lcov.reportPaths=C:/Users/WINDATA/Desktop/vms-tp-ci-cd/projet/front-demo-angular/coverage/front-demo-angular/lcov.info \
                      -Dsonar.host.url=http://localhost:9000 \
                      -Dsonar.login=squ_06259873b5dc5332bc6f04dd0a846de6634605d9
                    """
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
