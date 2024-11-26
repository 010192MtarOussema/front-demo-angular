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
                bat """
                C:\\Users\\WINDATA\\Desktop\\vms-tp-ci-cd\\sonarqube-10.7.0.96327\\bin\\windows-x86-64\\sonar-scanner.bat ^
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
