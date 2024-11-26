pipeline {
    agent any

    environment {
        SONAR_SCANNER_HOME = tool 'sq1' // Utilisez le nom configuré dans Global Tool Configuration
    }

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
                withSonarQubeEnv('sq1') { // 'sq1' correspond au serveur SonarQube configuré dans Jenkins
                    bat """
                    ${SONAR_SCANNER_HOME}\\bin\\sonar-scanner.bat ^
                      -Dsonar.projectKey=angular-sonar-demo ^
                      -Dsonar.projectName=front-demo-angular ^
                      -Dsonar.projectVersion=1.0 ^
                      -Dsonar.sources=src ^
                      -Dsonar.exclusions=**/*.spec.ts,**/node_modules/** ^
                      -Dsonar.tests=src ^
                      -Dsonar.test.inclusions=**/*.spec.ts ^
                      -Dsonar.javascript.lcov.reportPaths=coverage/front-demo-angular/lcov.info ^
                      -Dsonar.host.url=http://localhost:9000 ^
                      -Dsonar.login=squ_06259873b5dc5332bc6f04dd0a846de6634605d9
                    """
                }
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
